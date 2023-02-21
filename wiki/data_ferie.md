# Ferieafholdelse

<!-- ERD, datamodel -->
<details><summary markdown="span">Datamodel</summary>
   <center>
      **Udskiftes med ERD fra SSMS eller TE3**
      <img src="Images/erd/erd_pbi_ferieafholdelse.png" height="95%" width="95%" class="center"/>
   </center>
</details>  
<br>


## Resume af tabeller

### v_DimFerieTyper

| **View** | **Opdateres** |
| - | - |
| [chru_cube].[v_DimFerieTyper] | Dagligt |

Tabel i 3 niveauer; ferie-type, -kategori og -element. ID er primærnøgle. Hierarki er arvet fra SD’s ferietabeller, hvor ’FerieType’ angiver enten ordinær ferie eller 6. ferieuge, ’FerieKategori’ beskriver saldoen hhv. feriesaldo, anvendt ferie, planlagt ferie eller forventet tilskrivning. ’FerieElement’ er de enkelte ferieelementer indeholdt i respektive saldi.



### v_DimFerieÅr

| **View** | **Opdateres** |
| - | - |
| [chru_cube].[v_DimFerieÅr] | Dagligt |

Tabel med afviklingsstart- og -slutdatoer på ordinære ferieperioder i sidste, igangværende og næste ferieår samt afviklingsstart- og slutdato for 6. ferieuge i igangværende og næste år. 
```SQL
--fra v_DimFerieÅr
FROM AlleFerieÅr
WHERE 
		(FerieType = 'Ordinær ferie' AND FerieÅrStatus IS NOT NULL)
	OR	(FerieType = '6. ferieuge' AND FerieÅrStatus IN ('Igangværende ferieår', 'Næste ferieår'))
```
Tabellens datofelter opdateres dynamisk, idet afviklingsperioden for ordinær ferie går fra 1.9.yyyy til 31.12.yyyy+1 og for 6. ferieuge fra 1.5.yyyy til 31.4.yyyy+1. 
’FerieÅrStatus’ indikerer, om perioden er i sidste, igangværende eller næste ferieår, og ’FerieÅrTekstLang’ er sammensat af ’FerieType’ og ’AfviklingsSlutdato’. ’NæsteDeadline’ sorterer tabellen stigende på ’AfviklingsSlutdato’.



### v_FactFerie

| **View** | **Opdateres** | **Baseret på** |
| - | - | - |
| [chru_cube].[v_FactFerie] | Dagligt | [DM_FL_HR].[FactFerieOrdinær] |
| [chru_cube].[v_FactFerie6Ferieuge] |  | [DM_FL_HR].[FactFerie6uge] |
| [chru_cube].[v_FactFerieOverført] |  | [DM_FL_HR].[FactFerie6Overfoert] |
|  |  | ([SD].[V_SD_FERIE_FERIE] |
|  |  | ([SD].[V_SD_FERIE_6UGE] |
|  |  | [DM_FL_HR].[SD_OPT_HOURSASSIGNMENT] |
|  |  | [DM_FL_HR].[FactTjenestetid] |

Ikke-indekseret tabel baseret på 3 separate SD-leverancer af 1.-5. ferieuge, 6. ferieuge og overført ferie samt vagtplandata fra Tjenestetid og Optima. 

Første to ferie-tabeller beskriver med 3 rækker (sidste, igangværende og næste afviklingsperiode) pr. medarbejder i bredt format antal timer på alle saldi; med og uden løn, overførte ferietimer og elevferie. Desuden om timer på hver af disse saldi er optjent, anvendt, planlagt eller forventes optjent. 

Til beregning af ferie, som medarbejder har ret til at afholde, reduceres det brede format fra kildetabeller til lang form omkring kolonnen ’FerieTimer’ atomiseret på kategorier (saldo, anvendt, planlagt og forventet) og elementer (’Ferie med løn’, ’Elevferie’, ’Overført ferie’ m.fl.).
```SQL
-- Feriesaldo
WHEN ferietyper.FerieKategori = 'Feriesaldo' AND ferietyper.FerieElement = 'Ferie med løn' THEN MLOENSALDORET
WHEN ferietyper.FerieKategori = 'Feriesaldo' AND ferietyper.FerieElement = 'Ferie uden løn 1' THEN ULOEN1SALDORET
WHEN ferietyper.FerieKategori = 'Feriesaldo' AND ferietyper.FerieElement = 'Ferie uden løn 2' THEN ULOEN2SALDORET
WHEN ferietyper.FerieKategori = 'Feriesaldo' AND ferietyper.FerieElement = 'Overført ferie' THEN OVFSALDORET
WHEN ferietyper.FerieKategori = 'Feriesaldo' AND ferietyper.FerieElement = 'Overført ferie, feriehindring' THEN OVFHSALDORET
WHEN ferietyper.FerieKategori = 'Feriesaldo' AND ferietyper.FerieElement = 'Elevferie' THEN ELEVSALDORET

-- Anvendt ferie
WHEN ferietyper.FerieKategori = 'Anvendt ferie' AND ferietyper.FerieElement = 'Ferie med løn' THEN MLOENANVENDT
WHEN ferietyper.FerieKategori = 'Anvendt ferie' AND ferietyper.FerieElement = 'Ferie uden løn 1' THEN ULOEN1ANVENDT
WHEN ferietyper.FerieKategori = 'Anvendt ferie' AND ferietyper.FerieElement = 'Ferie uden løn 2' THEN ULOEN2ANVENDT
WHEN ferietyper.FerieKategori = 'Anvendt ferie' AND ferietyper.FerieElement = 'Overført ferie' THEN OVFANVENDT
WHEN ferietyper.FerieKategori = 'Anvendt ferie' AND ferietyper.FerieElement = 'Overført ferie, feriehindring' THEN OVFHANVENDT
WHEN ferietyper.FerieKategori = 'Anvendt ferie' AND ferietyper.FerieElement = 'Elevferie' THEN ELEVANVENDT

-- Planlagt ferie
WHEN ferietyper.FerieKategori = 'Planlagt ferie' AND ferietyper.FerieElement = 'Planlagt ferie' 
	THEN MLOENPLANLAGT + ULOEN1PLANLAGT + ULOEN2PLANLAGT + OVFPLANLAGT + OVFHPLANLAGT + ELEVPLANLAGT

-- Forventet yderligere tilskrivning
WHEN OPSLUT >= CONVERT(date, GETDATE()) THEN
	CASE 
		WHEN ferietyper.FerieType = 'Ordinær ferie' AND ferietyper.FerieElement = 'Forventet yderligere ferie med løn'
			THEN 
				CASE 
					WHEN ferie.MLOENSALDOFORVENTET - ferie.MLOENSALDORET < 0 THEN 0
					ELSE ferie.MLOENSALDOFORVENTET - ferie.MLOENSALDORET
				END
		WHEN ferietyper.FerieType = 'Ordinær ferie' AND ferietyper.FerieElement = 'Forventet yderligere ferie uden løn 1' 
			THEN ROUND(ferie.ULOEN1SALDOFORVENTET - ferie.ULOEN1SALDORET + COALESCE(overfoert.OvfSaldo, 0), 2)
		ELSE 0
	END
ELSE 0
END AS FerieTimer
```



### v_FactFerieOrdinær

| **View** | **Opdateres** | **Baseret på** | 
| - | - | - |
| [chru_cube].[v_FactFerieOrdinær] | Dagligt | [DM_DF_HR].[FactFerieOrdinær] |
|  |  | ([SD].[V_SD_FERIE_FERIE]) |



### v_FactFerie6Ferieuge

| **View** | **Opdateres** | **Baseret på** | 
| - | - | - |
| [chru_cube].[v_FactFerie6Ferieuge] | Dagligt | [DM_DF_HR].[FactFerieOrdinær] |
|  |  | ([SD].[V_SD_FERIE_6UGE]) |



### v_FactFerieOverført

| **View** | **Opdateres** | **Baseret på** | 
| - | - | - |
| [chru_cube].[v_FactFerieOverførte] | Dagligt | [DM_DF_HR].[FactFerieOverfoert] |
|  |  | ([SD].[V_SD_FERIE_]) |



### v_TallyRestferieIntervaller

| **View** | **Opdateres** |
| - | - |
| [chru_cube].[v_TallyRestferieIntervaller] | Dagligt |



