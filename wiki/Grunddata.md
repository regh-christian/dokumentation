# Grunddata

Med grunddata menes de tabeller, som foruden stamdata (v_DimPerson, v_DimAnsættelse, v_DimOrganisation, v_DimStilling, v_DimLønart), indgår på tværs af temaer, der behandles i kuben. Dvs. datotabellen, tabeller anvendt ved brugerstyring, tabel med info om seneste dataleverancer og tabel med servicemeddelelser.



## Datamodel
![Power BI-model, grunddata](https://raw.githubusercontent.com/DataOgDigitalisering/dokumentation/master/Images/cube_model_basis.png)


     
     
## Relationer
..... ..... .....
<br>




## Resume af tabeller

### v_DimTidDato

| **View** | **Baseret på** | 
| - | - |
| [chru_cube].[v_DimTidDato] | [DM_FL_HR].[DimDato] |


```mermaid
flowchart LR

A[Hard] -->|Text| B(Round)
B --> C{Decision}
C -->|One| D[Result 1]
C -->|Two| E[Result 2]
```


Baseret på tidstabellen, [DM_FL_HR].[DimDato] med en række mindre modifikationer og enkelte tilføjelser. Å erstattes med aa i hht. >>konvention<< om navngivning af kolonner. Formater såsom Y2016-M06 erstattes af 2016-06 og enkelte nye variable indføres pba. eksisterende. Fx er >>’DagKortmaanedAar’<< (10. jan. 2016) sammensat af ’DagMåned’, ’MånedNavn’ og ’År’. 
Enkelte ny kolonner beregnes; Kolonnen ’Arbejdsdag’ er tilføjet og udregner, om dato er en arbejds-, helligdag eller i en weekend. 
[Metode](https://www.computerworld.dk/uploads/eksperten-guider/107-Beregning-af-arbejdsdage-og-skaeve-helligdage.pdf). 
Til beregning heraf anvendes tre stored procedures; funktionen _chru_cube.DanskeHelligdage_ matcher dato mod kendte og faste helligdage og returnerer binært udfald; _chru_cube.PaaskeDage_ implementerer Gauás algoritme til bestemmelse af påskedag det givne år, hvorefter også ikke-faste helligdage da bestemmes (antal dage efter påske); I den sidste bestemmes, om dato er i en weekend. I praksis kaldes første stored procedure med dato som argument. Hvis dato er en kendt helligdag, er output ’Helligdag’. I modsat fald kaldes næste procedure til bestemmelse af påskedag, hvorefter dato igen matches mod de ikke-faste helligdage. Ved fortsat manglende match kaldes sidste procedure til beregning af, om datoen er en lørdag eller søndag.
```sql
CASE WHEN chru_cube.DanskeHelligdage(Dato) = 1 THEN 'Helligdag'
     WHEN chru_cube.DanskeHelligdage(Dato) = 0 AND chru_cube.Arbejdsdage(Dato) = 0 THEN 'Weekend'
     ELSE 'Arbejdsdag' 
  END AS Arbejdsdag
     Kolonnen Filterdatoer markerer udvalgte nedslagsdatoer såsom ’I dag’, ’1. dag i året’ m.fl.  
```
<br>



### v_DimAnsættelse

| **View** | **Baseret på** | 
| - | - |
| [chru_cube].[v_DimAnsættelse] | [DM_FL_HR].[DimAnsættelse] &rarr; CØK: 07_FL_110_SD_DimAnsaettelse.sas &rarr; <a href="https://www.silkeborgdata.dk/sites/default/files/files/start.sd.dk/produkter/Datawarehouse/Dataleverancer/Snitflade%20PERSON.pdf" target="_blank">SD.SD_PERSON</a> |

View er baseret på SD-tabellen, SD_Person. ID er primærnøgle for det enkelte ansættelsesforhold. PersonID er nøgle henvisende til den enkelte person (CPR). En person kan have flere ansættelser på forskellige institutioner/afdelinger overlappende i tid—dog aldrig overlappende i tid på samme lønafsnit med samme tjenestenummer.
På den måde anvendes tabellen både som fact og dimension afhængig af kontekst; om vi henviser til datostyrede variable knyttet til ansættelsen såsom stillingskode, overenskomst og beskæftigelsesdecimal eller foretager optællinger på personniveau. Vi anvender fx denne sondring mellem ansættelsesforhold og person til at sikre, at en leder kun kan se data relevant for det afsnit, hvor leder har beføjelser—via ’NuværendeOrganisationID’. Har en person fx flere samtidige ansættelser på forskellige institutioner, vil respektive ledere i udgangspunktet kun kunne se data for ansættelsesforholdet relevant for dem. Se desuden afsnit om <a href="https://github.com/DataOgDigitalisering/FortroligInformation/blob/main/Brugerstyring.md" target="_blank">Brugerstyring</a>.
     I dataindlæsningen hos CØK fjernes ansættelser med status ’S’, institution ’2P’ (tjenestemandspensioner)
     Baseret på den i SD_Person-tabellen datostyrede del, indfører vi nedenstående dikotomiserede variable mhp. lettere til- og fravalg i population for både os selv i opbygning af scripts og measures og for brugere af dashboards i form af slicere.

**Fuldtid** og **Månedslønnet**: Kolonnerne ’DEL’ og ’BESKDEC’ angiver, om et ansættelsesforhold er fuld- eller deltid hhv. måneds- eller timelønnet. Dette er opsummeret i kolonnerne ’Fuldtid’ og ’Månedslønnet’ hvor Fuldtid=J hvis BESKDEC>=0 og Månedslønnet=N hvis DEL=2 eller 6 OR BESKDEC=0. 

**Ansat**: J hvis statuskode er enten 0, 1 eller 3.

**AktuelRække**: J hvis dags dato ligger indenfor ansættelsesforholdets start- og slutdato.
```sql
-- FRA SØREN: ”07_FL_110_SD_DimAnsaettelse.sas
(CASE  
    WHEN STAT IN ('0', '1', '3') THEN 1 
    ELSE 0 
  END) as Ansat length = 3,
(CASE
    WHEN BESKDEC >= 1 THEN 1
    ELSE 0
  END) as Fuldtid length = 3,
(CASE 
    WHEN TODAY() BETWEEN START AND SLUT THEN 1 
    ELSE 0 
  END) as AktuelRække length = 3,
(CASE 
    WHEN DEL IN ('2', '6') OR BESKDEC = 0 THEN 0
    ELSE 1
  END) as Månedslønnet length = 3,
```

**AnsatDagsDato**: J hvis Ansat=J og AktuelRække=J. Dvs. J hvis statuskode er 0, 1 eller 3 og dags dato ligger indenfor ansættelsesforholdets start- og slutdato.

**AnsatSeneste14Mdr**: J hvis Ansat=J indenfor seneste 14 mdr. Dette for altid at kunne se ét helt år tilbage uanset tilfælde, hvor ansættelsesstart var fx midt- eller sidst i en måned.

**AktuelHovedansættelse**: Hvor flere ansættelsesforhold er sammenfaldende i tid, er et og kun et en hovedansættelse. Samtlige ansættelser en person har / har haft, arrangeres ud fra parametrene AktuelRække, Ansat, MånedsLønnet, Fuldtid og ansættelsesstartdato. Alle i faldende orden. Dette sikrer, at der i ethvert tidsrum hvor en person har et eller flere ansættelsesforhold uagtet beskæftigelsesgrad og aflønningsmetode, kan peges på ét og kun ét ansættelsesforhold som værende en hovedansættelse—nemlig ansættelsen jf. nævnte sortering, der desuden opfylder kriterierne Ansat=J OG AktuelRække=J.
En person kan i sin ansættelseshistorik have flere ansættelser, hvor AktuelHovedansættelse=J, men så fald vil de aldrig overlappe i tid.
EksterntFinansieret: J hvis afdelingen på ansættelsesstarttidspunktet var eksternt finansieret.  

**StandardPopulation**: Aktuelt ansatte, der er månedslønnede, fuldtidsansatte og ikke eksternt finansierede.
>>FIG: Peters matrix<<
   
**NuværendeOrganisationID**: Lønafsnit, hvor ansættelsesforhold er gældende dags dato. Har en person ansættelsesforhold med fremtidig startdato eller tidligere ansættelser med andet tjenestenummer, antager ’NuværendeOrganisationID’ blot værdien af ’OrganisationID’. 
```sql
,CASE
   WHEN [Start] >= CONVERT(date, GETDATE()) THEN OrganisationsID
   ELSE
     (SELECT OrganisationsID 
        FROM DM_FL_HR.DimAnsættelse sub 
      WHERE 1 = 1
         AND sub.Tjnr = src.Tjnr 
         AND CONVERT(date, GETDATE()) BETWEEN sub.[Start] AND sub.Slut
     )
  END AS NuværendeOrganisationID
```

**Hændelse**: Angiver om et månedslønnet ansættelsesforhold er en til- eller fratrædelse. Har ansættelsesforholdet værdien Ansat=J og denne er forskellig fra et evt. tidligere ansættelsesforhold med samme tjenestenummer, er dette en tiltrædelse. I modsat fald en fratrædelse.
```sql
,CASE 
   WHEN Ansat != COALESCE(LAG(Ansat) OVER(PARTITION BY Tjnr ORDER BY [Start] ASC), 99) 
     AND Månedslønnet = 1
   THEN CASE WHEN Ansat = 1 THEN 'Tiltrædelse' ELSE 'Fratrædelse' END
  END AS Hændelse
```

**HændelseMellem**: Angiver, hvor denne er anført, om en hændelse er sket mellem afdelinger (samme institution), mellem institutioner eller om denne er en afbrudt ansættelse på mindre end 3 måneder. Værdien ’Til/fra Region Hovedstaden’ hvis der på ansættelsesforholdet er en hændelseer, men ingen af førnævnte kriterier er opfyldt.

**HændelsesDato**: Startdatoen på ansættelser, hvor ’Hændelse’ er en tiltrædelse. På ansættelser hvor ’Hændelse’ er en fratrædelse, anvendes dagen før ansættelsens startdato som hændelsesdato—svt. sidste egentlige arbejdsdag.

**TillInst**: Hvor ’HændelseImellem’ er ”Mellem institutioner”, er denne udfyldt med institutionen på personens nye/kommende ansættelsesforhold.

***FLERE VARIABLE ER INDFØRT SIDEN SIDST!***
<br>

>> **ØVELSE - ÅRSVÆRK**
>> - Udregn vha. SD.SD_Person hvor mange årsværk, der arbejdes i din sektion baseret på aktuelt ansatte i dag. Gruppér dit resultat på time- hhv. månedslønnede, del- og fuldtidsansatte.
>> - Lav samme beregning og gruppering baseret på data fra kuben.
<br>



### v_DimPerson

| **View** | **Baseret på** |
|-|-|
| [chru_cube].[v_DimPerson] | [DM_FL_HR].[DimPerson] |

Dimensionstabel med ID som primærnøgle. Derudover navn og fødselsdato.
<br>



### v_DimStilling

| **View** |**Baseret på** |
|-|-|
| [chru_cube].[v_DimStilling] | [DM_FL_HR].[DimStillingskode] |

Stillingshieraki i 4 niveauer (L1-L4) med ID som primærnøgle. Hoved-, fag- og stillingsgruppe samt stilling. Afhængig af kontekst og datakilde omtales disse værdier også:

| LEVEL | SD | I TALE | BETYDNING |
|-|-|-|-|
| 1 | STILKO3 | Hovedstillingsgruppe | Hovedstillingsgruppe |
| 2 | STILKO2 | Fagstillingsgruppe | Fagstillingsgruppe |
| 3 | STILKO1 | Stillingsgruppe | Stillingsgruppe |
| 4 | STILKO0 | Stilling | Stilling |

Desuden er stillingskoderne 1133-1139 samt 1161 defineret (hard-codet i viewet) som uddannelseslæger. (Se UDV for øvrige definitioner af ledere og AtypiskeStillinger). Leder antager værdien ’J’ baseret på Stillingruppe eller Stilling—ellers ’N’. AtypiskStilling=’J’, når L1Code=’0000’—ellers ’N’.
Current_row=1 indikerer, at rækken er gældende i nuværende stillingskodehieraki.
<br>




### v_DimOrganisation

| **View** | **Baseret på** |
|-|-|
| [chru_cube].[v_DimOrganisation] | [DM_FL_HR].[DimOrganisation] |

Organisationshieraki i 6 niveauer (L1-L6) med ID som primærnøgle. Afhængig af kontekst og organisation lokalt omtales disse værdier også:

| LEVEL | SD   | I TALE   | BETYDNING                                                   |
|-------|------|----------|-------------------------------------------------------------|
| L1    | ORG6 | Inst     | Institution/virksomhed/hospital                             |
| L2    | ORG5 | NY4      | Drift/finansiering                                          |
| L3    | ORG4 | NY3      | Center                                                      |
| L4    | ORG3 | NY2      | Afdeling. (Klinik på RH og enhed i Koncerncentre)           |
| L5    | ORG2 | NY1      | Samleafsnit                                                 |
| L6    | ORG1 | Afdeling | Afsnit/lønafsnit (på hospitaler), sektion (i koncerncentre) |

Variablen MedarbejdereAnsat=1 angiver, om der er >0 ansatte på afdelingen (optalt i v_DimAnsættelse hvor AnsatDagsDato=J og AktuelRække=J). Bruges fx som filtre på slicere, hvor der ikke ønskes at kunne vælge niveauer med 0 ansatte.
Current_row=1 sikrer opdateret organisationshieraki men ignorerer evt. historiske ændringer.
<br>



### v_DimLønart

| **View** | **Baseret på** |
|-|-|
| [chru_cube].[v_DimLønart] | [DM_FL_HR].[DimLønart] |

<br>



### v_Servicemeddelelser

| **View** | **Baseret på** |
|-|-|
| [chru_cube].[v_Seervicemeddelelser] | - |

View oprettet til at give brugere af dashboardet information om ændringer og nye features, samt dato for og status på disse. Kolonnen ’Dahboard’ indikerer med ’L’, ’S’ eller ’L,S’, om en meddelelse er tilegnet visning på hhv. leder- strategisk- eller begge dashboards.
Tabellen vises datosorteret på velkomstsiden af dashboards uden kolonnen ’Dashboard’—denne skjules og anvendes til at filtrere på relevant for respektive dashboard.
Opdateres hvert 10-15min.
<br>



# Brugerstyring

<!-- Embed iFrame. word-doc: Brugerstyring.docx" på OneDrive-->
<center>
<iframe src="https://regionh-my.sharepoint.com/personal/stefan_sajin-henningsen_regionh_dk/_layouts/15/Doc.aspx?sourcedoc={c652f92d-8025-4f11-9b4c-3e0f0e0dadba}&amp;action=embedview&amp;wdEmbedCode=0&amp;wdPrint=0&wdToolbar=FALSE" width="100%" height="700" frameborder="0" seamless="yes"></iframe>
</center>

<br>
