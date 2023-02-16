# Lederdashboard

## Personalesammensætning

<!-- Embed dashboards  -->
<!-- <details><summary markdown="span">HR Lederdashboard</summary>
<center> -->
<iframe src="https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HR%20Lederdashboard/Personalesammens%C3%A6tning?RC:Toolbar=false" style="border:1px #000000 solid;" frameborder="1" height="435" width="100%"></iframe>
<!-- </center>
</details> -->

<!--
<details><summary markdown="span">Strategisk dashboard</summary>
<center>
<iframe src="https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HR%20Strategisk%20Dashboard/HR%20Strategisk%20Dashboard?RC:Toolbar=False" style="border:1px #000000 solid;" frameborder="1" height="565" width="100%"></iframe>
</center>
</details>
-->

I dashboardet udstilles data på enkeltmedarbejderniveau tiltænkt ledere. 
Bruger får her overblik over udvalgte parametre om de medarbejdere, denne i forvejen har adgang til via PersonaleWeb. Se [brugerstyring](./Brugerstyring).

Der filtreres globalt _på fanen_ ud fra kriterierne v_DimAnsættelse[AnsatDagsDato]=J og v_DimAnsættelse[AktuelRække]=J
Som udgangspunkt vises altså alle lederens medarbejdere med en aktuel ansættelse pågældende dag inklusiv evt. tidligere (eller kommende) ansættelser på samme tjenestenummer.
Har bruger adgang til medarbejdere på tværs af organisationsstruktur, kan der filtreres herpå, ligesom der kan filtreres på tværs af stillingshieraki—med undtagelse af laveste stillingsniveau.
Foruden brug af grunddata er oprettet views til brug for beregninger i dette tema. Se [Tabeller](./Personalesammensætning#resume-af-tabeller). 



#### Beregninger

Measuret, [Antal medarbejdere], summerer groft antallet af rækker i v_DimAnsættelse. Det anvendes i en række andre measures uden skelnen imellem populationer (månedslønnede, fuldtidsansatte eller andet). 
```DAX
[Antal medarbejdere] =
IF (
    ISBLANK ( COUNTROWS ( 'v_DimAnsættelse' ) ),
    0,
    COUNTROWS ( 'v_DimAnsættelse' )
)
```
Antallet af måneds- og timelønnede, fuldt- og deltidsansatte, der fremgår af visninger (_cards_) til venstre i dashboardet, er alle baseret på [Antal medarbejdere] evalueret i forskellige filterkontekster ved brug af én eller flere af de dikotomiserede J/N-kolonner i v_DimAnsættelse. Et eksempel herpå er [Antal fuldtidsansatte]
```DAX
[Antal fuldtidsansatte] =
CALCULATE (
    [Antal medarbejdere],
    'v_DimAnsættelse'[Månedslønnet] = "J",
    'v_DimAnsættelse'[Fuldtid] = "J"
)
```

   Antal [Årsværk], vist sammen med de fire føromtalte, beregner summen af beskæftigelsesdecimaler, v_DimAnsættelse[Beskdec], som i definitionen er andelen af en 37 timers arbejdsuge, en person er ansat til og her i praksis oversættes til årsværk. Specifikt på visningen af årsværk filtreres v_DimAnsættelse[Månedslønnede]=’J’ i selve figuren.
    
Til beregning af [Ansættelseslængde] (antal år) anvendes differencen mellem dags dato og v_DimAnsættelse[Ansættelsesdato]. 
I figuren **Ansættelseslængde** anvendes dette measure af [AnsatteAnsættelseslængdeInterval] til at beregne medarbejders ansættelseslængde i nuværende stilling (+evt tidligere ansættelser på samme tjenestenummer). Dette measure er designet til, i kontekst af v_TallyAnsættelseslænge, at beregne [Ansættelseslængde] evalueret i en filterkontekst af tjenestenummer (og AnsættelsesID for at sikre optælling på unikke individer) og returnerer _antallet af medarbejdere i intervallerne_ specificeret i tally-tabellen.
Ved mouse-over vises den enkelte medarbejders ansættelseslængde udregnet med [Ansættelseslængde].

Figuren **Aldersfordeling** viser antallet af medarbejdere, [AnsatteAldersinterval], fordelt på aldersintervaller. Også dette measure er designet til visning i kontekst af en tally-tabel, v_TallyAlder. Metodisk er beregning og opbygning af measure identisk med førnævnte, [AnsatteAnsættelseslængdeInterval], hvor der hér regners på alder i stedet for ansættelseslængde. Measuret, [Alder], beregner blot en persons alder som differencen mellem dennes fødselsdag (v_DimPerson[Fødselsdato]) og dags dato. Specifikt på denne figur er nedre og øvre aldersintervaller filtreret fra via v_TallyAlder[AldersInterval_5år], så kun personer i alderen 15-79 år vises.
Ved mouse-over på **Aldersfordeling** vises enkelte medarbejderes [Alder] inden for hvert interval

Tabellen **Hændelser 14 dg.** frem viser en kronologisk oversigt over udvalgte personrelaterede hændelser i de kommende 14 dage; fx fødselsdage, jubilæer, orlov, tiltrædelse, fratrædelse m.m. (Tabellen, **Hændelser 12 mdr. frem** via bogmærket af samme navn, er identisk med førnævnte men med filteret, v_DimTidDato[OffsetDato]=[0;365].

I **Informationstabel** vises udvalgte medarbejderdata herunder ansættelsessted (L6/L4, afsnit/afdeling), tjenestenummer, stilling, alder, ansættelseslængde, anciennitet m.m. [Anciennitet] er hér ikke nødvendigvis lig [Ansættelseslængde]! Baseret på ansattes nuværende v_DimAnsættelse[Anciennitetsdato], kan denne dato ligge forud for ansættelse i Region H (ifm. tilskrivning af relevant anciennitet), eller den kan være ændret fx ifm. overflytning til anden overenskomst eller konvertering af deltidsenciennitet. Visse ansættelser med fastlåst anciennitetsforløb kan være uden anciennitetsdato. Measuret [Anciennitet] beregner, i antal hele år, differencen mellem v_DimAnsættelse[Anciennitetsdato] og dags dato. For fremtidige anciennitetsdatoer anvendes dags dato.
[Ansat til] angiver en eventuel slutdato på medarbejders senest tildelte tjenestenummer, hvis en sådan findes. [Alder], [Ansættelseslængde] og [Årsværk] beregner, i kontekst af unikke personer (her i kontekst af kombinationen v_DimPerson[Navn] og v_DimAnsættelse[Tjnr]) pågældende persons alder og ansættelseslænge på dagen i hele antal år samt beskæftigelsesdecimal (årsværk) på pågældende ansættelse.




## Sygefravær

<iframe src="https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HR%20Lederdashboard/Sygefrav%C3%A6r?RC:Toolbar=False" style="border:1px #000000 solid;" frameborder="1" height="435" width="100%"></iframe>


#### Beregninger
Fravær er bredt defineret ved lønarterne i v_DimLønartFravær[L1Code]. 
Hvor andet ikke er nævnt, medregnes månedslønnede (v_DimAnsættelse[Månedslønnet]=’J’) ansatte (v_DimAnsættelse[Ansat]=’J’ svt. statuskode {0,1,3} ).
Filteret, v_DimLønartFravær[L1Name]=”Sygefravær”, på fanen Sygefravær sikrer, at kun denne fraværskategori medtages i beregninger her. Via bogmærker føres bruger til faner uden dette filter, hvor også øvrige kategorier vises. 
     I beregninger opgøres fravær overordnet på én af to måder;
Measure, [Fravær – antal arbejdsdage], summerer v_FactFravær[Arbejdsdage] og indgår som byggesten i en række andre measures i dette tema. En arbejdsdag (/fraværsdag) er hér fraværstimer relativt til planlagt tid.
I modsætning hertil vises fravær også som løbende gennemsnit—over korte eller længere perioder—af antal fuldtidsfraværsdage relativt til den, på opgørelsestidspunktet, gennemsnitlige beskæftigelsesdecimal. En fuldtidsfraværsdag er andelen af fraværstimer på en dag af en standard 7,4 timers arbejdsdag. Beskæftigelsesdecimal oversættes direkte til årsværk. Denne, ”fuldtidsfraværsdage pr. årsværk”, er da en normaliseret og vægtet enhed, som derved er sammenlignelig på tværs af afdelinger, uanset at disse har forskellig sammensætning af fuld- og deltidsansatte samt varierende komposition i vagtlag.


**FIGUR: Antal sygefraværsdage de seneste 12 mdr.**
I tabellen vises [Fravær – antal arbejdsdage på tværs af org] i filterkontekst af tid (v_DimTidDato[KortMaanedNavn] og person (v_DimPerson[ID]). Measure tilsidesætter den eksisterende relation, v_DimAnsættelse[OrganisationID]v_DimOrganisation[ID], og beregner [Fravær – antal arbejdsdage] (fraværstimer relativt til planlagt tid) i relationen, v_DimAnsættelse[NuværendeOrganisationID]v_DimOrganisation[ID]. 
```SQL
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
Dette, i kombination med filtret specifikt på denne figur, [Ansat på afdeling, nuværende afd]=1, gør, at bruger også ser data fra evt. tidligere ansættelser—på samme tjenestenummer! 
Desuden filtreres på v_DimAnsættelse[AnsatDagsDato]=’J’, hvorfor kun personer med an aktiv ansættelse dd. vises—også historisk. 

>> TIP! Inaktiv relation mellem v_DimAnsættelse[NuværendeOrganisationID] og v_DimOrganisation[ID] anvendes her til at beregne fravær på individniveau (tjenestenummer). v_DimAnsættelse[NuværendeOrganisationID] viser, på alle tidligere ansættelser med samme tjenestenummer, hvor personen aktuelt er ansat dags dato. Har en medarbejder flere ansættelser på tværs af organisation, og har bruger også adgang til data herfra, vises medarbejderens totale fravær.

Via bogmærket, Fravær pr. type, ses samme beregning i figuren  Antal fraværsdage de seneste 12 mdr. fordelt på fravær, hvor filterkontekst på tid og fraværstype (v_DimLønartFravær[L1Name]) er ophævet. Dermed ses akkumuleret antal fraværsdage i perioden, men nu fordelt på underkategori af fraværstyper, v_DimLønartFravær[L1Name]. Bruger kan til- og fravælge underkategorier via filter med standardindstillingen {”Sygefravær”, ”$56 fravær”, ”Arbejdsskade”, ”Graviditetsgener” }. 
Desuden filtreres på v_DimAnsættelse[AnsatDagsDato]=’J’, hvorfor kun personer med an aktiv ansættelse dd. vises. Desuden ’Ansat på afdelingen, nuværende afdeling’=1.
<br>

**FIGUR: Sygefravær de seneste 12 mdr. fordelt på intervaller**
Med [Fravær – antal i fraværsintervaller Ikke anonymiseret] beregnes fraværsdage (fraværstimer relativt til planlagt tid). Measure er designet til at beregne—i kontekst af v_TallyFraværsintervaller[Interval]—antallet af personer i perioden—på tværs af organisation—med antal fraværsdage indenfor definerede intervaller. (Intervaller inkluderer øvre grænseværdier.)

| [**Daxpatterns.com \ Dynamic segmentation**](https://www.daxpatterns.com/dynamic-segmentation/) | <img src="Images/icons_ref/icon_daxpatterns.png" height="45" width="45"> | 
     
Filtrene, [Ansat på afdeling, nuværende afd]=1 og v_DimAnsættelse[AnsatDagsDato]=’J’, anvendes igen til beregning på personniveau på tværs af organisation, men kun hvis disse er ansat dags dato. 

>>TIP! v_Tally-tabeller er i vores fulde kontrol, hvorfor vi relativt let kan konfigurere i intervalbredde og -antal
<br>


**FIGUR: Gns. løbende sygefravær opgjort over seneste 12 mdr.**
Til beregning anvendes to measures, [Fravær – vægtede fuldtidsfraværsdage gnsnit 12 mdr Ikke anonymiseret] og [Fravær – Benchmark regionen 12 mdr.].

> | [**Sqlbi.com \ Rolling 12 Months Average in DAX**](https://www.sqlbi.com/articles/rolling-12-months-average-in-dax/) | <img src="Images/icons_ref/icon_sqlbi.png" height="45" width="45"> |

Førstnævnte udregner i kontekst af v_DimTidDato[MaanedAar] og valgt(e) organisationsniveau(er) (1) for hver d. 1. i måneden i foregående 12 måneder den løbende gennemssnitssum af beskæftigelsesdecimaler på aktuelle ansættelser pågældende datoer—også selvom de ikke er ansatte længere—, (2) månedlig sum af fuldtidsfraværsdage indeværende måned, v_FactFravær[Fuldtidsdage]. Endeligt (3) andel af fuldtidsfraværsdage af den gennemsnitlige beskæftigelsessum. (4) Kurven ’Aktuel visning’ viser dermed—beregnet i en månedskontekst—for hver måned det løbende gennemsnit af antal fuldtidsfraværsdage pr. årsværk opgjort over seneste 12 måneder.

>> HUSK! v_FactFravær[Fuldtidsdage] er antallet af fraværstimer på en dag relativt til en 7,4 timers arbejdsdag. Dette, i kombination med en gennemsnitssum af beskæftigelsesdecimaler, bruges til at normalisere beregning af fravær i enheden ”antal fuldtidsfraværsdage pr. årsværk”. Dermed muliggøres sammenligning af fravær på tværs af afdelinger, uanset at disse har forskellig sammensætning af fuld- og deltidsansatte samt varierende komposition i vagtlag.

Med figurspecifikt filter på v_DimTidDato[OffsetMaaned]=[-12;-1] udregnes kun på fravær i dette tidsinterval . 
     Til visning af ’Alle medarbejdere jf. rolle’ indgår foregående beregning i measure [Fravær – benchmark regionen 12 mdr.], hvor filterkonteksten v_DimOrganisation ophæves. Inkluderet i denne beregning er da alle personer i v_DimAnsættelse—på tværs af organisation—som bruger har adgang til. Anonymitetsgrænsen er implementeret.
     Regionens måltal på 11,7%  fravær er defineret som en ’Y-axis Constant Line’.
Via bogmærket, Sygefravær pr. måned, kan bruger se næsten samme beregning i Sygefravær de seneste 12 mdr. fordelt på måneder. I denne figur vises [Fravær – vægtede fuldtidsfraværsdage] opgjort pr. måned—fra sidste dag i sidst måned—og ikke et løbende gennemsnit, hvormed sæsonudsving bliver tydeligere. Metodik i measure er identisk med [Fravær – vægtede fuldtidsfraværsdage gnsnit 12 mdr Ikke anonymiseret] med undtagelse af specificering af opgørelsesperiode på én måned i stedet for 12 samt implementering af anonymitetsgrænsen, da der også tælles på historisk ansatte, som ikke skal kunne identificeres. 

**FIGUR: Gns. sygefravær de seneste 12 mdr. fordelt på afdelinger**
Også her anvendes det vægtede gennemsnit. Andel af normaliseret fravær (ift. 7,4timer/dag) relativt til gennemsnitlig sum af beskæftigelsesdecimal (opgjort pr. d. 1. hver måned i seneste 12 måneder). 
Beregnes ved først summering af og dernæst ratioen mellem hhv. [MaskeretFravSum12Mdr] og [MaskeretBeskDec12Mdr] i v_FactAggFravær. 
     Har bruger adgang til medarbejdere på tværs af organisation, vises en bar pr. afdeling (v_DimOrganisation[L4Name]).
     Med figurspecifikt filter på v_DimTidDato[OffsetMaaned]=-1 regnes 12 måneder bagud startende fra sidste hele måned.
     Via fanen, Sygefravær pr. afsnit, kan samme figur ses opgjort på laveste enhedsniveau. 



## Ferieafholdelse