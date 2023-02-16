# Personalesammensætning

<!-- ERD, datamodel -->
<details><summary markdown="span">Datamodel</summary>
     <center>
          **Udskiftes med ERD fra SSMS eller TE3**
          <img src="Images/erd/erd_pbi_personalesammensætning.png" height="95%" width="95%" class="center"/>
     </center>
</details>  
<br>


## Resume af tabeller

### v_DimHændelser

| **View** | **Baseret på** | 
| - | - |
| [chru_cube].[v_DimHændelser] | [DM_FL_HR].[DimHændelser] |

Udvalgte hændelser vedrørende en ansættelse. ID er primærnøgle.
Ved en hændelse forstås en af følgende mærkedage: dato for fødsels- og jubilæumsdag, til- og fratrædelser, flytter mellem afdelinger, termin samt orlov. 
Tabellen er niveaudelt i L1 og L2, hvor L2 er en uddybende tekst knyttet til den overordnede hændelse, L1; fx om ’Flytter afdeling’ er et skift _fra_- eller _til_ en afdeling. I kolonnen ’Note’ forklares, hvordan en hændelse er defineret—om den fx er direkte udledt vha. en dato, eller om der kigges på flere på hinanden følgende ansættelser og skift i statuskoder herimellem for at udlede, om en person går på eller kommer retur fra orlov.



### v_FactHændelser

| **View** | **Baseret på** | 
| - | - |
| [chru_cube].[v_FactHændelser] | [chru_cube].[v_DimAnsættelse] |
| | [chru_cube].[v_Dimperson] |
| | [chru_cube].[v_DimTidDato] |
| | [DM_FL_HR].[FactTerminsdato] |

- Kan med fordel dokumenteres grundigere i script
- Vær opmærksom på, at HændelsesID hard-codes i overensstemmelse med definitioner i v_DimHændelser. Dvs. foretages modificeringer i v_DimHændelser, arves det ikke i fact-tabellen.
- Bemærk, at tabellen er koblet på v_SecurityOrganisationBridge via HændelseOrganisationID.

View er baseret på v_DimAnsættelser og beriges af øvrige tabeller. ID er primærnøgle. Historiske og fremtidige hændelser—nærmere defineret i v_DimHændelser—beregnes 13 måneder frem i tid for hvert år en aktuel medarbejder er ansat. Er hændelsen en fødsels- eller jubilæumsdag, er under ’AntalÅr’ anført hhv. personens alder det pågældende år, eller jubilæumslængde hvis det er et 1-, 5-, 10-, 15 (osv.) års jubilæum. 
Endeligt filtreres på hændelser i intervallet $$ \left[ -7\text{dage} ; 13\text{måneder} \right] $$. 

View er defineret i et længere script, der opsummeret udfører følgende step:
- Først sammensættes en temporær tabel, PersonBase, med rækker af fødsels- og jubilæumsdatoer _alle_ år i en persons ansættelsesperiode. 
- Dernæst en temporær tabel, PersonLags, med en række pr. ansættelse sammenfattende aktuelle samt evt. seneste og næstkommende ansættelsesforhold (OrganisationID og statuskode). 
- I en ny temporær tabel, MasterTable, listes for hvert tjenestenummer ’HændelsesID’ og tilsvarende ’HændelsesDato’. ’HændelsesID’ er her hard-codet i overensstemmelse med v_DimHændelser (1 for fødselsdag, 2 for jubilæum, 3 for tiltrædelse osv.). For ’HændelsesID’ svt. ’Fødselsdag’ og ’Jubilæumsdag’ anvendes allerede beregnede ’HændelsesDato’(er) fra PersonBase. Ved ’HændelsesID’er svt. til til- og fratrædelser samt flyt til- og fra afdeling anvendes nu PersonLags; 
     - På ’HændelsesID’ svt. ’Tiltræder’ defineres ’HændelsesDato’ som værende en ansættelses startdato på ansættelser, hvor ’Ansat’=J og dette er forskelligt fra en evt. umiddelbart foregående ansættelse på samme tjenestenummer; 
     - omvendt defineres ved ’HændelsesID’ svt. ’Fratræder’, at ’HændelsesDato’ er slutdatoen på en ansættelse, hvor ’Ansat’=N og dette er forskelligt fra en umiddelbart foregående ansættelse på samme tjenestenummer. 
     - Til ’HændelsesID’ svt. ’Skifter fra anden afdeling’ og ’Skifter til anden afdeling’ anvendes hhv. start- og slutdatoen på den ansættelse hvor ’OrganisationsID’ er forskelligt fra den umiddelbart forrige respektivt efterfølgende ansættelse. 
     - På ’HændelsesID’ svt. ’Går på orlov’ anvendes som ’HændelsesDato’ startdatoen på den ansættelse, hvor statuskode skifter til 3 og denne er forskellig fra foregående; 
     - For ’HændelsesID’ svt. ’Tilbage fra orlov’ anvendes omvendt ’HændelsesDato’ på ansættelsen hvor statuskode skifter fra 3 til 0 eller 1. 
     - På ’HændelsesID’ svt. termin anvendes som ’HændelsesDato’ ’Terminsdato’ fra tabellen [DM-FL-HR].[FactTerminsdato].
- Ovenstående beregninger samles i sidste temporære tabel, FinalTable, hvor tidssvarende ansættelses- og OrganisationsID tilføjes, relativ alder og jubilæumslængde beregnes og alt filtreres i tid, så kun hændelser i intervallet $$ \left[ -7\text{dage} ; 13\text{måneder} \right] $$ medtages. 
Endeligt tilføjes tidssvarende Stillings- og PersonID og rækker indekseres



### v_TallyAlder

| **Tabel baseret på** | 
| - |
| [chru_cube].[v_TallyAlder] |

Talrækken 0-124 genereres og døbes ’Alder’. Næste to kolonner beregner placering af denne i 5- og 10 års intervaller døbt ’AldersInterval_5år’ og ’AldersInterval_10år’. Filtreres på alder fra 0-99 år.



### v_TallyAnsættelseslængde

| **Tabel baseret på** | 
| - |
| [chru_cube].[v_TallyAnsættelseslængde] |

Talrækken 0-124 genereres og døbes Ansættelseslængde’. Næste to kolonner beregner placering af denne i 5- og 10 års intervaller døbt ’AnsættelseslængdeInterval_5år’ og ’ AnsættelseslængdeInterval_10år’. Filtreres på ’ Ansættelseslængde’ fra 0-99 år. Til brug på fx figurakser, hvor kun et begrænset antal intervaller ønskes vist, beregnes kolonnen ’AnsættelseslængdeIntervalVisualisering’. 
Endeligt tilføjes ’AnsættelseslængdeIntervalVisualiseringSortering’ mhp. at kunne sortere forrige kolonne på dens ”numeriske” værdi i Power BI-visninger.



### v_TallyAnciennitet

| **Tabel baseret på** | 
| - |
| [chru_cube].[v_TallyAnciennitet] |

Identisk i opbygning og beregninger som v_TallyAnsættelseslængde. 
