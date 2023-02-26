# Sygefravær

<!-- ERD, datamodel -->
<details><summary markdown="span">Datamodel</summary>
   <center>
      **Udskiftes med ERD fra SSMS eller TE3**
      <img src="Images/erd/erd_pbi_sygefravær.png" height="95%" width="95%" class="center"/>
   </center>
</details>  
<br>


## Resume af tabeller

### v_DimLønartFravær

| **View** | **Opdateres** | **Baseret på** | 
| - | - | - |
| [chru_cube].[v_DimLønartFravær] | Dagligt | [DM_FL_HR].[DimLønart] |

Subset af v_DimLønart med samme primærnøgle, ID. Indeholder kun lønarter relateret til fravær.
Tabellen er i to niveuer. ’L1Code’ og ’L1Name’ er hard-codet kategorisering af visse lønarter i 3 fraværstyper; ’Sygefravær’, ’Barn syg’ og ’Andet’.
’L2Code’ er 3-cifret lønartkode—svt. ’L1Code’ i v_DimLønart—og ’L2Name’ er hard-codet undergruppering af fraværstyper. 
```SQL
,case 
   when l1code in(510, 771, 772, 775,760) then 'Øvrigt fravær'
   when l1code in(520) then 'Omsorgsdag'
   when l1code in(522,525) then '§56 fravær'
   when l1code in(524,590) then 'Arbejdsskade'
   when l1code in(790,526) then 'Sygefravær'
   when L1Code in(780,527) then 'Graviditetsgener'
   when l1code in(570,575,576,770) then 'Barsel og adoption'
   when l1code in(680,950) then 'Kursus'
   when l1code in(774) then 'Seniortimer'
   when l1code in(910,929) then 'Barn syg'
end as l2Name
```
> Vær opmærksom på, at gruppering på L2-niveau ikke er identisk er med SD's *LOENARTTXT*

<!-- ØVELSE -->
{::options parse_block_html="true" /}
<details><summary markdown="span">**ØVELSE - FRAVÆRSTYPER** <img src="Images/icons_ref/icon_git.png" height="35" width="35"></summary>
   
> I CHRU_HRKube anvendes i tabellen v_DimLønartFravær en anden gruppering af fraværstyper (L2Name), end den gruppering SD bruger (LOENARTTXT).
>
> - Gør rede for ligheder og forskelle. 
> - Hvad er grunden til, at der i chru_cube anvendes en anden gruppering?
>
> Se <a href="https://github.com/DataOgDigitalisering/FortroligInformation/blob/main/Exercises/ex_frav%C3%A6rstyper.sql" target="_blank">**løsningsforslag**</a>.

</details>
{::options parse_block_html="false" /}




### v_FactFravær

| **View** | **Opdateres** | **Baseret på** | 
| - | - | - |
| [chru_cube].[v_FActFravær] | Månedligt | [DM_FL_HR].[FactFravær] |

Ikke-indekseret tabel baseret på [DM_FL_HR].[FactFravær]. 
Kolonnerne ’AnsættelsesID’, ’Fraværsdato’, ’LønartsID’, ’TimerFravær’ og ’TimerPlanlagt’ er tilsvarende kolonnerne i SD’s [SD].[SD_FRAVDAG],  ’AnsaettelsesID’, ’FRVDATO’, ’FRVAARSAG’, ’FRVTIM’ og ’PLANTIM’ .
I chru_cube beregnes desuden kolonnerne ’Arbejdsdage’ som andelen af fraværstimer ud af planlagte timer og ’Fuldtidsdage’ som andelen af fraværstimer ud af 7,4t/dag.
Fraværsdage på dage uden planlagte timer medregnes ikke.



### v_FactAggFravær

| **View** | **Opdateres** | **Baseret på** | 
| - | - | - |
| [chru_cube].[v_FactAggFravær] | Månedligt | [DM_FL_HR].[FactFraværAfsnit] |
| | | [chru_cube].[v_DimOrganisation] 

Ikke-indekseret tabel baseret på [DM_FL_HR].[FactFraværAfsnit]. 
Tabellen aggregerer fravær pr. måned pr. afdeling med opgørelses(’Dato’) hver  d. 1. i en måned. ’OrganisationsID’ er F-nøgle til v_DimOrganisation, og L6Name henviser til afsnit i samme.
’AntalMedarbejdere’ er aktuelt ansatte på afsnittet opgjort på dagen, ’BeskSumMd’ er summen af beskæftigelsesdecimaler udgjort af aktuelt ansatte. 
’MaskeretFlag’ har tidligere været brugt til indikation af få ansatte mhp. anonymisering—har i dag ingen funktion.
’MaskeretBeskSum12Mdr’ og ’MaskeretFavSum12Mdr’ er månedligt gennemsnit af hhv. beskæftigelsesdecimaler og fraværsdage i seneste år.



### v_TallyFraværsintervaller

| **View** | **Opdateres** |
| - | - |
| [chru_cube].[v_TallyFraværsintervaller] | Dagligt / efter behov |

Tabel med dagsintervaller i intervaller af 5 dage. ’Intervaller’ er en tekststreng til beskrivelse af intervallet, fx ’5 til 10 dage’. ’ValueLow’ og ’ValueHigh’ er hhv. ned og øvre grænse (inkluderende—i measures defineres inklusionskriterier nærmere) af intervallet. ’Sortering’ er en numerisk værdi svt. rækkefølgen på intervallerne. Denne bruges til at nummerere tekstværdien i ’Interval’, så disse placeres i ønsket, sorteret rækkefølge på figurer og tabeller i Power BI.



### v_TallyFraværMaaltal

| **View** | **Opdateres** |
| - | - |
| [chru_cube].[v_TallyFraværMaaltal] | Dagligt / efter behov |

Tabel med regionens måltal for fravær. To rækker beskrivende hhv. under og over måltallet fx ’>11.7 dg./år’. Kolonnerne ’ValueLow’ og ’ValueHigh’ er ned og øvre grænse (inkluderende) af intervallet. ’Sortering’ er en numerisk værdi svt. rækkefølgen på intervallerne. Denne bruges til at nummerere tekstværdien i ’Interval’, så disse placeres i ønsket, sorteret rækkefølge på figurer og tabeller i Power BI.



### v_SlicerNedslagsdatoer

| **View** | **Opdateres** | **Baseret på** | 
| - | - | - |
| [chru_cube].[v_SlicerNedslagsdatoer] | Dagligt / efter behov | [DM_FL_HR].[DimDato] |

’Dato’ med udvalgte nedslagsdatoer i intervallet fra dags dato og tre år bagud. ’DagKortmaanedAar’ med dato i formatet ”dd. mmm. yyyy”. Kolonnen ’Filterdatoer’ er tekst til nærmere beskrivelse af datoen; kan fx være ”I dag”, ”Den 1. i denne måned” eller ”Den 1. i tidligere måneder”. Bruges bl.a. til at simplificere i measures, hvor fx ansættelsedecimal evalueres for hver d. 1. i måneden. 
