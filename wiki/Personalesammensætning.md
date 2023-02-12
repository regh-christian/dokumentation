# Personalesammensætning



<!-- Embed dashboards 
<details><summary markdown="span">HR Lederdashboard</summary>
<center>
<iframe src="https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HR%20Lederdashboard/HR%20Lederdashboard?RC:Toolbar=False" style="border:1px #000000 solid;" frameborder="1" height="565" width="100%"></iframe>
</center>
</details>
-->


<!--
<details><summary markdown="span">Strategisk dashboard</summary>
<center>
<iframe src="https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HR%20Strategisk%20Dashboard/HR%20Strategisk%20Dashboard?RC:Toolbar=False" style="border:1px #000000 solid;" frameborder="1" height="565" width="100%"></iframe>
</center>
</details>
-->


## Datamodel

<center><img src="Images/erd/erd_pbi_personalesammensætning.png" height="95%" width="95%" class="center"/></center>



## Resume af tabeller

### v_DimHændelser

| **View** | **Baseret på** | 
| - | - |
| [chru_cube].[v_DimHændelser] | [DM_FL_HR].[DimHændelser] |

Udvalgte hændelser vedrørende en ansættelse. ID er primærnøgle.
Ved en hændelse forstås en af følgende mærkedage: dato for fødsels- og jubilæumsdag, til- og fratrædelser, flytter afdeling, termin samt orlov. 
Tabellen er niveaudelt i L1 og L2, hvor L2 er en uddybende tekst knyttet til den overordnede hændelse, L1; fx om ’Flytter afdeling’ er et skift fra- eller til en afdeling. I kolonnen ’Note’ forklares, hvordan en hændelse er defineret—om den fx er direkte udledt vha. en dato, eller om der kigges på flere på hinanden følgende ansættelser og skift i statuskoder herimellem for at udlede, om en person går på eller kommer retur fra orlov.



### v_FactHændelser

| **View** | **Baseret på** | 
| - | - |
| [chru_cube].[v_FactHændelser] | [chru_cube].[v_DimAnsættelse] |
| | [chru_cube].[v_Dimperson] |
| | [chru_cube].[v_DimTidDato] |
| | [DM_FL_HR].[FactTerminsdato] |

- Kan med fordel dokumenteres grundigere i script
- Vær opmærksom på, at HændelsesID er er hard-codet i overensstemmelse med ID ’er i v_DimHændelser. Dvs. foretages modificeringer i **v_DimHændelser**, skal det tilsvarende modificeres i dette view.
- Bemærk, at tabellen er koblet på v_SecurityOrganisationBridge via HændelseOrganisationID.

Viewet er baseret på v_DimAnsættelser. ID er primærnøgle. Historiske og fremtidige hændelser—nærmere defineret i v_DimHændelser—vises for hvert år en aktuel medarbejder er ansat. Er hændelsen en fødsels- eller jubilæumsdag, er under ’AntalÅr’ anført hhv. personens alder det pågældende år, eller jubilæumslængde hvis det er et 1-, 5-, 10-, 15 (osv.) års jubilæum. 
Endeligt filtreres på hændelser i intervallet . 
     Viewet er defineret i et længere script, der opsummeret først sammensætter en temporær tabel, PersonBase, med rækker af fødsels- og jubilæumsdatoer alle år i en persons ansættelsesperiode. Dernæst en temporær tabel, PersonLags, med en række pr. ansættelse sammenfattende aktuelle samt evt. seneste og næstkommende ansættelsesforhold (OrganisationID og statuskode). 
I en ny temporær tabel, MasterTable, listes for hvert tjenestenummer ’HændelsesID’ og tilsvarende ’HændelsesDato’. ’HændelsesID’ er her hard-codet i overensstemmelse med v_DimHændelser (1 for fødselsdag, 2 for jubilæum osv.). 
For ’HændelsesID’ svt. ’Fødselsdag’ og ’Jubilæumsdag’ indsættes allerede beregnede ’HændelsesDato’(er) fra PersonBase. Ved ’HændelsesID’er svt. til til- og fratrædelser samt flyt til- og fra afdeling anvendes nu PersonLags; På ’HændelsesID’ svt. ’Tiltræder’ defineres ’HændelsesDato’ som værende en ansættelses startdato på ansættelser, hvor ’Ansat’=J og dette er forskelligt fra en evt. umiddelbart foregående ansættelse på samme tjenestenummer; omvendt defineres ved ’HændelsesID’ svt. ’Fratræder’, at ’HændelsesDato’ er slutdatoen på en ansættelse, hvor ’Ansat’=N og dette er forskelligt fra en umiddelbart foregående ansættelse på samme tjenestenummer. Til ’HændelsesID’ svt. ’Skifter fra anden afdeling’ og ’Skifter til anden afdeling’ anvendes hhv. start- og slutdatoen på den ansættelse hvor ’OrganisationsID’ er forskelligt fra det umiddelbart forrige respektivt efterfølgende. På ’HændelsesID’ svt. ’Går på orlov’ anvendes som ’HændelsesDato’ startdatoen på den ansættelse, hvor statuskode skifter til 3 og denne er forskellig fra foregående; For ’HændelsesID’ svt. ’Tilbage fra orlov’ anvendes omvendt ’HændelsesDato’ på ansættelsen hvor statuskode skifter fra 3 til 0 eller 1. På ’HændelsesID’ svt. termin anvendes som ’HændelsesDato’ ’Terminsdato’ fra tabellen [DM-FL-HR].[FactTerminsdato].
Ovenstående beregninger samles i sidste temporære tabel, FinalTable, hvor tidssvarende ansættelses- og OrganisationsID tilføjes, relativ alder og jubilæumslængde beregnes og alt filtreres i tid, så kun hændelser i intervallet . medtages. Endeligt tilføjes tidssvarende Stillings- og PersonID og rækker indekseres



### v_TallyAlder

### v_TallyAnsættelseslængde



## Dashboard

<center><img src="Images/dashboard_personalesammensætning_info.png" height="95%" width="95%" alt="Lederdashboard, info om personalesammensætning" class="center"/></center>


### Lederdashboard
I dashboardet udstilles data på enkeltmedarbejderniveau tiltænkt ledere. 
Bruger (ledere) får her overblik over udvalgte parametre om de medarbejdere, denne i forvejen har adgang til via PersonaleWeb. Se >>Brugerstyring<<. Som udgangspunkt vises alle lederens medarbejdere med en aktuel ansættelse pågældende dag. Har bruger adgang til medarbejdere på tværs af organisationsstruktur, kan der filtreres herpå, ligesom der kan filtreres på tværs af stillingshieraki—med undtagelse af laveste niveau, stillingskode/stillingskodetekst.
     Foruden brug af grunddata er oprettet views til brug for beregninger i dette tema. Se >>TABELLER<<. 



### Beregninger

Measuret, [Antal medarbejdere], summerer antallet af rækker i v_DimAnsættelse. Denne sum anvendes i flere andre measures i tilpassede filterkontekster. Antallet af henholdsvis månedslønnede, fuldtidsansatte, deltidsansatte og timelønnede fremgår af visninger (cards) til venstre, og er alle baseret på [Antal medarbejdere] evalueret i forskellige filterkontekster bestemt ved de dikotomiserede J/N-kolonner i v_DimAnsættelser.
Antal [Årsværk], vist sammen med de fire føromtalte, beregner summen af beskæftigelsesdecimaler, v_DimAnsættelse[Beskdec], som i definitionen er andelen af en 37 timers arbejdsuge en person er ansat til, og her i praksis oversættes til årsværk.
Specifikt på visningen af årsværk filtreres v_DimAnsættelse[Månedslønnede]=’J’.
     [Ansættelseslængde] er et andet measure, der anvendes af flere andre. Det beregner antallet år fra en persons ansættelsesdato i kommunen (hvis en sådan findes) til i dag. 
I figuren Ansættelseslængde anvendes dette measure til at beregne medarbejdernes ansættelseslængde i nuværende stilling, [AnsatteAnsættelseslængdeInterval]. Dette measure beregner [Ansættelseslængde] evalueret i en filterkontekst af tjenestenummer (og AnsættelsesID for at sikre optælling på unikke individer) og returnerer [Antal medarbejdere] med en given ansættelseslængde. Brugt i en visning sammen med v_TallyAnsættelseslængde opgøres antallet af medarbejdere med an ansættelseslængde i hvert af disse intervaller. 
Ved mouse-over på Ansættelseslængde vises en oversigt over de enkelte medarbejdere inden for hvert ansættelseslængdeinterval. Dette ved at bruge measuret [Ansættelseslængde] i en tabel med navn og PersonID, hvor filterkontekst da er det unikke PersonID.
     Figuren Aldersfordeling viser antallet af medarbejdere, [AnsatteAldersinterval], fordelt på aldersintervaller. Metodisk er beregning og opbygning af measure identisk med førnævnte, [AnsatteAnsættelseslængdeInterval]. I measuret, [AnsatteAldersinterval], indgår measuret [Alder] dog i stedet for [Ansættelseslængde] ligesom der nu optælles [Antal medarbejdere] pr. ’Aldersinterval’ i stedet for pr. ’Ansættelseslængde’. Measuret, [Alder], beregner blot en persons alder som differencen mellem dags dato og dennes fødselsdag angivet i v_DimPerson. 
Specifikt på denne figur er nedre og øvre aldersintervaller filtreret fra via v_TallyAlder[AldersInterval_5år], så kun intervaller mellem 15 og 79 år vises.
Ved mouse-over på Aldersfordeling vises en oversigt over de enkelte medarbejdere inden for hvert aldersinterval. Dette ved at bruge measuret [Alder] i en tabel med navn og PersonID, hvor filterkontekst da er det unikke PersonID.
     Tabellen Hændelser 14 dg. frem viser en kronologisk oversigt over udvalgte personrelaterede hændelser i de kommende 14 dage, fx fødselsdage, jubilæer, orlov, tiltrædelse, fratrædelse m.m. ____ 
     Tabellen Hændelser 12 mdr. frem er identisk med førnævnte, men med filteret, 0≤ v_DimTidDato[OffsetDato] ≤365.
     Tabellen Informationstabel viser en generel oversigt over udvalgte medarbejderdata i forbindelse med den aktuelle ansættelse, herunder afdeling, afsnit, tjenestenummer, stilling, alder, ansættelseslængde, anciennitet m.m. Ancienniteten af baseret på de ansattes nuværende anciennitetsdato, som både kan ligge forud for ansættelse i Region H ifm. tilskrivning af relevant anciennitet, eller som kan være ændret ifm. oprykning eller overflytning til anden overenskomst. Visse ansættelser med fastlåst anciennitetsforløb kan være uden anciennitetsdato. Measuret [Anciennitet] beregner, i antal hele år, differencen mellem v_DimAnsættelse[Anciennitetsdato] og dags dato. For fremtidige anciennitetsdatoer anvendes dags dato.
[Ansat til] angiver en eventuel slutdato på den aktuelle ansættelse. [Alder], [Ansættelseslængde] og [Årsværk] beregner, i kontekst af unikke personer (her i kontekst af kombinationen v_DimPerson[Navn] og v_DimAnsættelse[Tjnr]) pågældende persons alder og ansættelseslænge på dagen i hele antal år samt beskæftigelsesdecimalen/årsværk på pågældende ansættelse.



> **ØVELSE — BRUG AF ADGANGE**
> 
> Hospital XXX ønsker optælling af alle pr. dd. månedslønnede hhv. fuld- og deltidsansatte sygeplejersker 
> udspecificeret for alle sygeplejegrupper under fællesbetegnelsen STILKO2TXT = ’Sygepl. Pers’
> Hvor mange af disse er autoriserede med speciale indenfor anæstesi?



