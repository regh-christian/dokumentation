# Sygefravær

Fravær er bredt defineret ved lønarterne i v_DimLønartFravær[L1Code].
Der filtreres globalt _på fanen_ ud fra kriterierne v_DimAnsættelse[Ansat]=J, v_DimAnsættelse[Månedslønnet]=J og v_DimLønartFravær[L1Name]='Fravær'. 
Som udgangspunkt vises altså alle lederens månedslønnede medarbejdere med en aktuel—eller historisk—ansættelse med statuskode 0, 1 eller 3, _hvis_ denne i opgørelsesperioden har registreret fravær i en af førnævnte lønarter. 

Fravær opgøres overordnet på én af to måder. 
Measure, _[Fravær – antal arbejdsdage]_, summerer v_FactFravær[Arbejdsdage]. Med arbejdsdag forstås hér en **fraværsdag** opgjort relativt til planlagt tid, $$ \frac{\text{fraværstimer}}{\text{planlagt arbejdstid}} $$.

I modsætning hertil vises fravær også som løbende gennemsnit—over korte eller længere perioder—af antal fuldtidsfraværsdage relativt til den, på opgørelsestidspunktet, gennemsnitlige beskæftigelsesdecimal. 

> En fuldtidsfraværsdag er andelen af fraværstimer på en dag af en standard 7,4 timers arbejdsdag. Beskæftigelsesdecimal oversættes direkte til årsværk. ”Fuldtidsfraværsdage pr. årsværk” bruges som normaliseret og vægtet enhed og muliggør sammenlignelig på tværs af afdelinger, uanset at disse har forskellig sammensætning af fuld- og deltidsansatte samt varierende komposition i vagtlag.

Har bruger adgang til medarbejdere på tværs af organisationsstruktur, kan der filtreres herpå, ligesom der kan filtreres på tværs af stillingshieraki.

Foruden brug af grunddata er oprettet views og tabeller til brug i beregninger under dette tema. Se [***Resume af tabeller***](./Sygefravær#resume-af-tabeller). 



## Lederdashboard

I dashboardet udstilles data på enkeltmedarbejderniveau tiltænkt ledere. 
Bruger får her overblik over udvalgte parametre om de medarbejdere, denne i forvejen har adgang til via PersonaleWeb. Se [***brugerstyring***](./Brugerstyring).

<iframe src="https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HR%20Lederdashboard/Sygefrav%C3%A6r?RC:Toolbar=False" style="border:1px #000000 solid;" frameborder="1" height="435" width="100%"></iframe>



#### Beregninger
I figur **Antal sygefraværsdage de seneste 12 mdr.** udregnes på individniveau, [Fravær – antal arbejdsdage på tværs af org]. I beregningen ophæves relationen v_DimAnsættelse[OrganisationsID]➔_DimOrganisation[ID] til fordel for v_DimAnsættelse[**Nuværende**OrganisationsID]➔_DimOrganisation[ID] og beregner [Fravær – antal arbejdsdage].
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
```DAX
[Ansat på afdeling, nuværende afd] =
--Er afhængig af den tilsvarende relation under "Relationships".
CALCULATE ( [Ansat på afdeling], 
    USERELATIONSHIP ( 'v_DimAnsættelse'[NuværendeOrganisationID], v_DimOrganisation[ID] )
)
```
hvor
```DAX
[Ansat på afdeling] =
VAR RelevantPersonID =
    MAX ( 'v_DimAnsættelse'[PersonID] )
VAR RelevantePersoner =
    FILTER ( v_DimPerson, [ID] = RelevantPersonID )
VAR AnsatPaaAfdeling =
    IF ( COUNTROWS ( RelevantePersoner ) + 0 > 0, 1, 0 )
RETURN
    AnsatPaaAfdeling
```
Endeligt filtreres på v_DimAnsættelse[AnsatDagsDato]=’J’, hvorfor kun personer med an aktiv ansættelse dd. vises—men altså også historiske asnsættelser, hvis tjenstenummer er bevaret. 

> Inaktiv relation mellem v_DimAnsættelse[NuværendeOrganisationID] og v_DimOrganisation[ID] anvendes her til at beregne fravær på individniveau (tjenestenummer). v_DimAnsættelse[NuværendeOrganisationID] viser, på alle tidligere ansættelser med samme tjenestenummer, hvor personen aktuelt er ansat dags dato. Har en medarbejder flere ansættelser på tværs af organisation, og har bruger også adgang til data herfra, vises medarbejderens totale fravær.

Via bogmærket, **Fravær pr. type**, ses samme beregning (akkumulkeret) i figuren **Antal fraværsdage de seneste 12 mdr. fordelt på type**, hvor filterkontekst på fraværstype (v_DimLønartFravær[L1Name]) er ophævet. Bruger kan til- og fravælge underkategorier via filter med standardindstillingen {”Sygefravær”, ”$56 fravær”, ”Arbejdsskade”, ”Graviditetsgener” }. 
Igen filtreres på figuren med kriterierne, [Ansat på afdelingen, nuværende afdeling]=1 og v_DimAnsættelse[AnsatDagsDato]=J, hvorfor alle ansættelser med samme tjenestenummer på aktuelt ansatte medregnes.
<br>

> Læs: [inspiration til measuret [Fravær – antal i fraværsinterval Ikke anonymiseret]](https://www.daxpatterns.com/dynamic-segmentation/)
> 
> | [**Daxpatterns.com \ Dynamic segmentation**](https://www.daxpatterns.com/dynamic-segmentation/) | <img src="Images/icons_ref/icon_daxpatterns.png" height="45" width="45"> | 

I **Sygefravær de seneste 12 mdr. fordelt på intervaller** beregnes igen relative fraværsdage $$ \frac{\text{fraværstimer}}{\text{planlagt arbejdstid}} $$ på individniveau. [Fravær – antal i fraværsintervaller Ikke anonymiseret] beregner [Fravær – antal arbejdsdage på tværs af org] i kontekst af tally-tabellen, v_TallyFraværsintervaller[Interval.]. Intervaller inkluderer øvre grænseværdier. 
Igen filtreres _på figuren_ med kriterierne, [Ansat på afdelingen, nuværende afdeling]=1 og v_DimAnsættelse[AnsatDagsDato]=J, hvorfor alle ansættelser med samme tjenestenummer på aktuelt ansatte dags dato medregnes.

> Tally-tabeller er i vores fulde kontrol, hvorfor vi let kan konfigurere i intervalgrænser og -antal.

> Læs: [inspiration til measuret [Fravær – vægtede fuldtidsfraværsdage gnsnit 12 mdr Ikke anonymiseret]](https://www.sqlbi.com/articles/rolling-12-months-average-in-dax/)
> 
> | [**Sqlbi.com \ Rolling 12 Months Average in DAX**](https://www.sqlbi.com/articles/rolling-12-months-average-in-dax/) | <img src="Images/icons_ref/icon_sqlbi.png" height="45" width="45"> |

TIl beregning af **'Gns. løbende sygefravær opgjort over seneste 12 mdr.'** anvendes to measures, *[Fravær – vægtede fuldtidsfraværsdage gnsnit 12 mdr Ikke anonymiseret]* og *[Fravær – Benchmark regionen 12 mdr.]*. Førstnævnte udregner 

(1) for hver d. 1. i måneden i foregående 12 måneder den løbende gennemssnitssum af beskæftigelsesdecimaler på aktuelle ansættelser—også selvom de ikke er ansatte længere—, 
```DAX
...fra [Fravær – vægtede fuldtidsfraværsdage gnsnit 12 mdr Ikke anonymiseret])
VAR DenFoerste = FILTER ( Period, DAY ( [Dato] ) = 1 )
VAR AntalDageIPerioden = COUNTROWS ( DenFoerste )
VAR AntalFuldtidsdage =
    CALCULATE ( SUM ( 'v_FactFravær'[Fuldtidsdage] ), Period )
VAR PeriodMedBeskdec =
    GENERATE (
        DenFoerste,
        VAR AktuelDato = [Dato]
        VAR BeskSumPaaDagen =
            CALCULATE (
                SUM ( 'v_DimAnsættelse'[Beskdec] ),
                'v_DimAnsættelse'[Ansat] = "J",
                'v_DimAnsættelse'[Start] <= AktuelDato, AktuelDato <= 'v_DimAnsættelse'[Slut] 
            )
        RETURN ROW ( "BeskSum", BeskSumPaaDagen )
    )
VAR BeskSumPerioden = SUMX ( PeriodMedBeskdec, [BeskSum] )
VAR GennemsnitBesksum = 
	DIVIDE( BeskSumPerioden, AntalDageIPerioden, 0)
...
```
og (2) månedlig sum af fuldtidsfraværsdage indeværende måned, v_FactFravær[Fuldtidsdage]. Endeligt (3) andel af fuldtidsfraværsdage af den gennemsnitlige beskæftigelsessum. 
```DAX
...fra [Fravær – vægtede fuldtidsfraværsdage gnsnit 12 mdr Ikke anonymiseret])

VAR GnsnitFuldtidsdage =
    	DIVIDE ( AntalFuldtidsdage, GennemsnitBesksum, 0 )
RETURN GnsnitFuldtidsdage
...
```

(4) Denne er vist pr. måned med Kurven ’**Aktuel visning**’. _På figuren_ filtreres v_DimTidDato[OffsetMaaned]=[-12;-1].

> v_FactFravær[Fuldtidsdage] er antallet af fraværstimer på en dag relativt til en 7,4 timers arbejdsdag. Dette, i kombination med en gennemsnitssum af beskæftigelsesdecimaler, bruges til at normalisere beregning af fravær til ”antal fuldtidsfraværsdage pr. årsværk”. Enheden muliggør sammenligning af fravær på tværs af afdelinger, uanset at disse har forskellig sammensætning af fuld- og deltidsansatte samt varierende komposition i vagtlag.

(5) I samme figur er desuden, foruden regionens måltal for samme værdi, også benchmarking mod regionsgennemsnit af fuldtidsfraværsdage. 
```DAX
[Fravær - benchmark regionen 12 mdr] =
-- Measuret anvendes til benchmarkberegning af sygefraværet i hele regionen, idet organisatorisk filtrering undtages.
CALCULATE (
    [Fravær - vægtede fuldtidsfraværsdage gnsnit 12 mdr],
    ALL ( v_DimOrganisation )
)
```
Via bogmærket, **'Sygefravær pr. måned'**, ses fuldtidsfraværsdage _over tid_, hvormed sæsonvariation er tideligere.

I **Gns. sygefravær de seneste 12 mdr. fordelt på afdelinger** bennchmarkes fuldtidsfraværsdage mod regionens måltal og internt på tværs af afdelinger(er) (hvis bruger har adgang til flere). 
 ```DAX
[Fravær - Benchmark] = 
VAR BeskSum = SUM ( 'v_FactAggFravær'[MaskeretBeskSum12Mdr] )
VAR FravSum = SUM ( 'v_FactAggFravær'[MaskeretFravSum12Mdr] )
RETURN DIVIDE ( FravSum, BeskSum, 0 )
 ```
     
     
----------
 
## Strategisk dashboard

I dashboard udstilles aggregeret data om fravær på organisations- og stillingsniveau. 
Bruger får her overblik over organisationer, som denne i forvejen har adgang til via PersonaleWeb. Se [***brugerstyring***](./Brugerstyring). 
     
Fælles for alle beregninger er implementering af anonymitetsgrænsen. Hvor beregninger baseres på løbende nedslagsdatoer (fig. 2, 3, 4, 7. [Fravær – vægtede fuldtidsfraværsdage gnsnit 12 mdr]) kontrolleres, at antal inkluderede individer på nedslagsdatoer er tilstrækkeligt. Hvis ikke udelades resultat på nedslagsdato. Hvor kun aktuelt ansatte dags dato indgår i beregning sikres, at_antal aktuelt ansatte dags dato er tilstrækkeligt. 

> Anonymitetsgrænsen indbygges i nogle measures for at forhindre identifikation af enkeltindivider.

<iframe src="https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HR%20Strategisk%20Dashboard/HR%20Strategisk%20Dashboard?RC:Toolbar=False&filterPaneEnabled=false&navContentPaneEnabled=false&$initialTab=Sygefrav%C3%A6r" style="border:1px #000000 solid;" frameborder="1" height="434" width="100%"></iframe>

Fanespecifikt filter, v_DimStilling[Hovedstillingsgruppe],  fravælger grupperne ’Ansat uden løn’, ’Budgetteknik’, ’Politikere/tjm’, ’Samling’ og ’Ukendt’. v_DimLønartFravær[L1Name]=”Sygefravær” fjerner øvrige fraværstyper.



#### **Beregninger**
Til figurerne 2, 3, 4 og 7 anvendes _[Fravær – vægtede fuldtidsfraværsdage gnsnit 12 mdr]_ 

I **figur 2** ses seneste 3 års løbende 12 måneders gennemsnit opgjort sidste dage i respektive år via filter på v_SlicerNedslagsdatoer. Med input fra slicer kan bruger vælge nedslagsdato, og dermed ændre filterkontekst for measure, [Fravær – vægtede fuldtidsfraværsdage gnsnit 12 mdr Benchmark]. Valgt nedslagsdato er bestemmende for, fra hvilken dato dette measure regner 12 måner baglæns. Altså en dynamisk størrelse til benchmarking af aktuelt fravær med tidligere perioder.

**Figur 3 og 4** viser det vægtede fuldtidsgennemsnit 12 måneder bagud relativt til månederne, det er vist for, og figur 7 regner 12 måneder bagud fra dags dato. 

> Læs: [inspiration til measuret [Fravær – antal i fraværsinterval Ikke anonymiseret]](https://www.daxpatterns.com/dynamic-segmentation/)
> 
> | [**Daxpatterns.com \ Dynamic segmentation**](https://www.daxpatterns.com/dynamic-segmentation/) | <img src="Images/icons_ref/icon_daxpatterns.png" height="45" width="45"> | 

I **figurerne 5 og 6** beregnes igen fraværsdage (fraværstimer af planlagt tid), [Fravær – antal arbejdsdage på tværs af org]. I **figur 5** bruges dette i [Fravær – antal i fraværsinterval] til beregning af antal personer i perioden—og på tværs af organisation—med antal fraværsdage indenfor definerede intervaller. Measure er designet til at indgå i kontekst af tally-tabellen, v_TallyFraværsintervaller[Interval]. Intervaller inkluderer øvre grænseværdier. 

I **figur 6** relativt fravær i [Fravær – antal under regionalt niveau] og i kontekst af v_TallyFraværMaaltal til gruppering af antal personer over og under regionens måltal.

 
 
 
 
 
