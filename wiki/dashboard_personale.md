# Personalesammensætning

## Lederdashboard

I dashboardet udstilles data på enkeltmedarbejderniveau tiltænkt ledere. 
Bruger får her overblik over udvalgte parametre om de medarbejdere, denne i forvejen har adgang til via PersonaleWeb. Se [***brugerstyring***](./data_brugerstyring).

Der filtreres globalt _på fanen_ ud fra kriterierne v_DimAnsættelse[AnsatDagsDato]=J og v_DimAnsættelse[AktuelRække]=J .
Som udgangspunkt vises altså alle lederens medarbejdere med en aktuel ansættelse pågældende dag inklusiv evt. tidligere (eller kommende) ansættelser på samme tjenestenummer.
Har bruger adgang til medarbejdere på tværs af organisationsstruktur, kan der filtreres herpå, ligesom der kan filtreres på tværs af stillingshieraki.
Foruden brug af grunddata er oprettet views og tabeller til brug i beregninger under dette tema. Se [***Resume af tabeller***](./data_personalesammensætning#resume-af-tabeller). 

<!-- Embed dashboards  -->
<!-- <details><summary markdown="span">HR Lederdashboard</summary>
<center> -->
<iframe src="https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HR%20Lederdashboard/Personalesammens%C3%A6tning?RC:Toolbar=false" style="border:1px #000000 solid;" frameborder="1" height="435" width="100%"></iframe>
<!-- </center>
</details> -->


#### Beregninger

Measuret, _[Antal medarbejdere]_, summerer groft antallet af rækker i v_DimAnsættelse. Det anvendes i en række andre measures uden skelnen imellem populationer (månedslønnede, fuldtidsansatte eller andet). 
```DAX
[Antal medarbejdere] =
IF (
    ISBLANK ( COUNTROWS ( 'v_DimAnsættelse' ) ),
    0,
    COUNTROWS ( 'v_DimAnsættelse' )
)
```
Antallet af måneds- og timelønnede, fuldt- og deltidsansatte, der fremgår af visninger (_cards_) til venstre i dashboardet, er alle baseret på _[Antal medarbejdere]_ evalueret i forskellige filterkontekster ved brug af én eller flere af de dikotomiserede J/N-kolonner i v_DimAnsættelse. Et eksempel herpå er _[Antal fuldtidsansatte]_
```DAX
[Antal fuldtidsansatte] =
CALCULATE (
    [Antal medarbejdere],
    'v_DimAnsættelse'[Månedslønnet] = "J",
    'v_DimAnsættelse'[Fuldtid] = "J"
)
```
   Antal _[Årsværk]_, vist sammen med de fire føromtalte, beregner summen af beskæftigelsesdecimaler, v_DimAnsættelse[Beskdec], som i definitionen er andelen af en 37 timers arbejdsuge, en person er ansat til og her i praksis oversættes til årsværk. Specifikt på visningen af årsværk filtreres v_DimAnsættelse[Månedslønnede]=’J’ i selve figuren.
```DAX
[Antal årsværk] =
// Kan kun bruges når man kigger på aktuelle ansættelser eller en bestemt dato
IF (
    ISBLANK ( SUM ( 'v_DimAnsættelse'[Beskdec] ) ), 
    0,
    SUM ( 'v_DimAnsættelse'[Beskdec] )
)
```
Til beregning af _[Ansættelseslængde]_ (antal år) anvendes differencen mellem dags dato og v_DimAnsættelse[Ansættelsesdato]. 
I figuren **Ansættelseslængde** anvendes dette measure af _[AnsatteAnsættelseslængdeInterval]_ til at beregne medarbejders ansættelseslængde i nuværende stilling (+evt tidligere ansættelser på samme tjenestenummer). Dette measure er designet til, i kontekst af v_TallyAnsættelseslænge, at beregne _[Ansættelseslængde]_ evalueret i en filterkontekst af tjenestenummer (og AnsættelsesID for at sikre optælling på unikke individer) og returnerer _antallet af medarbejdere i intervallerne_ specificeret i tally-tabellen.
Ved mouse-over vises den enkelte medarbejders ansættelseslængde udregnet med _[Ansættelseslængde]_.

Figuren **Aldersfordeling** viser antallet af medarbejdere, _[AnsatteAldersinterval]_, fordelt på aldersintervaller. Også dette measure er designet til visning i kontekst af en tally-tabel, v_TallyAlder. Metodisk er beregning og opbygning af measure identisk med førnævnte, _[AnsatteAnsættelseslængdeInterval]_, hvor der hér regners på alder i stedet for ansættelseslængde. Measuret, _[Alder]_, beregner blot en persons alder som differencen mellem dennes fødselsdag (v_DimPerson[Fødselsdato]) og dags dato. Specifikt på denne figur er nedre og øvre aldersintervaller filtreret fra via v_TallyAlder[AldersInterval_5år], så kun personer i alderen 15-79 år vises.
Ved mouse-over på **Aldersfordeling** vises enkelte medarbejderes _[Alder]_ inden for hvert interval

Tabellen **Hændelser 14 dg.** frem viser en kronologisk oversigt over udvalgte personrelaterede hændelser i de kommende 14 dage; fx fødselsdage, jubilæer, orlov, tiltrædelse, fratrædelse m.m. (Tabellen, **Hændelser 12 mdr. frem** via bogmærket af samme navn, er identisk med førnævnte men med filteret, v_DimTidDato[OffsetDato]=[0;365].

I **Informationstabel** vises udvalgte medarbejderdata herunder ansættelsessted (L6/L4, afsnit/afdeling), tjenestenummer, stilling, alder, ansættelseslængde, anciennitet m.m. _[Anciennitet]_ er hér ikke nødvendigvis lig _[Ansættelseslængde]_! Baseret på ansattes nuværende v_DimAnsættelse[Anciennitetsdato], kan denne dato ligge forud for ansættelse i Region H (ifm. tilskrivning af relevant anciennitet), eller den kan være ændret fx ifm. overflytning til anden overenskomst eller konvertering af deltidsenciennitet. Visse ansættelser med fastlåst anciennitetsforløb kan være uden anciennitetsdato. Measuret _[Anciennitet]_ beregner, i antal hele år, differencen mellem v_DimAnsættelse[Anciennitetsdato] og dags dato. For fremtidige anciennitetsdatoer anvendes dags dato.
_[Ansat til]_ angiver en eventuel slutdato på medarbejders senest tildelte tjenestenummer, hvis en sådan findes. _[Alder]_, _[Ansættelseslængde]_ og _[Årsværk]_ beregner, i kontekst af unikke personer (her i kontekst af kombinationen v_DimPerson[Navn] og v_DimAnsættelse[Tjnr]) pågældende persons alder og ansættelseslænge på dagen i hele antal år samt beskæftigelsesdecimal (årsværk) på pågældende ansættelse.


----------

## Strategisk Dashboard

<iframe src="https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HR%20Strategisk%20Dashboard/HR%20Strategisk%20Dashboard?RC:Toolbar=False" style="border:1px #000000 solid;" frameborder="1" height="435" width="100%"></iframe>



#### Beregninger

Alle visninger er baseret på beregningen, _[AntalAnsatteNedslagsdatoer]_. Denne er designet som et switch-measure til, i kontekst af valgt **Ansættelsesform**, at beregne enten _[Antal medarbejdere]_, _[Antal personer]_ eller _[Antal årsværk]_. Af ansættelsesformer kan vælges blandt v_TallyAnsættelsesformer[Ansættelsesform].

| Ansaettelsesform  | Sortering |
|       :-          |     -:    |
| Ansættelser       |       1   |
| Månedslønnede	    |       2   |
| Fuldtidsansatte   |       3   |
| Deltidsansatte	|       4   |
| Årsværk           |       5   |
| Timelønnede       |       6   |
| Personer          |       7   |

I measuret er således defineret bereging og inklusionskriterier for hver af disse populationer

```DAX
--...fra [AntalAnsatteNedslagsdatoer] 
...
VAR __Ansaettelser =
    CALCULATE (
        [Antal medarbejdere],
        'v_DimAnsættelse'[Start] <= __Dato, __Dato <= 'v_DimAnsættelse'[Slut],
        'v_DimAnsættelse'[Ansat] = "J",
        'v_DimAnsættelse'[EksterntFinansieret] <> __Filter_EksterntFinansierede
    )
VAR __Maanedsloennede =
    CALCULATE (
        [Antal medarbejdere],
        'v_DimAnsættelse'[Start] <= __Dato, __Dato <= 'v_DimAnsættelse'[Slut],
        'v_DimAnsættelse'[Ansat] = "J",
        'v_DimAnsættelse'[Månedslønnet] = "J",
        'v_DimAnsættelse'[EksterntFinansieret] <> __Filter_EksterntFinansierede
    )
VAR __Aarsvaerk =
    CALCULATE (
        [Antal årsværk],
        'v_DimAnsættelse'[Start] <= __Dato, __Dato <= 'v_DimAnsættelse'[Slut],
        'v_DimAnsættelse'[Ansat] = "J",
        'v_DimAnsættelse'[Månedslønnet] = "J",
        'v_DimAnsættelse'[EksterntFinansieret] <> __Filter_EksterntFinansierede
    )
VAR __Fuldtidsansatte =
    CALCULATE (
        [Antal medarbejdere],
        'v_DimAnsættelse'[Start] <= __Dato, __Dato <= 'v_DimAnsættelse'[Slut],
        'v_DimAnsættelse'[Ansat] = "J",
        'v_DimAnsættelse'[Månedslønnet] = "J",
        'v_DimAnsættelse'[Fuldtid] = "J",
        'v_DimAnsættelse'[EksterntFinansieret] <> __Filter_EksterntFinansierede
    )
VAR __Deltidsansatte =
    CALCULATE (
        [Antal medarbejdere],
        'v_DimAnsættelse'[Start] <= __Dato, __Dato <= 'v_DimAnsættelse'[Slut],
        'v_DimAnsættelse'[Ansat] = "J",
        'v_DimAnsættelse'[Månedslønnet] = "J",
        'v_DimAnsættelse'[Fuldtid] = "N",
        'v_DimAnsættelse'[EksterntFinansieret] <> __Filter_EksterntFinansierede
    )
VAR __Timeloennede =
    CALCULATE (
        [Antal medarbejdere],
        'v_DimAnsættelse'[Start] <= __Dato, __Dato <= 'v_DimAnsættelse'[Slut],
        'v_DimAnsættelse'[Ansat] = "J",
        'v_DimAnsættelse'[Månedslønnet] = "N",
        'v_DimAnsættelse'[EksterntFinansieret] <> __Filter_EksterntFinansierede
    )
VAR __Personer =
    CALCULATE (
        [Antal personer],
        'v_DimAnsættelse'[Start] <= __Dato, __Dato <= 'v_DimAnsættelse'[Slut],
        'v_DimAnsættelse'[EksterntFinansieret] <> __Filter_EksterntFinansierede
    )
...    
```

```DAX
[Antal personer] =
IF (
    ISBLANK ( DISTINCTCOUNT ( 'v_DimAnsættelse'[PersonID] ) ), 
    0,
    DISTINCTCOUNT ( 'v_DimAnsættelse'[PersonID] )
)
```




