# Ferieafholdelse

Ferie er, som det præsenteres i dashboard, bredt defineret som den (rest)ferie, medarbejder har ret til at afholde. Indeholdt i beregningen er positive bidrag fra feriesaldo og forventet tilskrivning samt negative bidrag fra anvendt og planlagt ferie.

Feriesaldo, anvendt og planlagt ferie inkluderer alle saldi; med og uden løn, overførte ferietimer og elevferie. Forventet tilskrivning er udregnet i ikke afsluttede optjeningsperioder, og udgøres af ferietimer, som medarbejder forventes at have til afhold i hele afviklingsåret (baseret på aktuel beskæftigelsesdecimal), fratrukket evt. aktuel optjent feriesaldo. For ferie uden løn medregnes også evt. overført ferie. Fra dags dato indhentes planlagte timer fra Tjenestetid og Optima på lønarterne, 730 og 752. 

Alle beregninger foretages på sidste, igangværende og næste ferieår.
Hvor andet ikke er nævnt, medregnes alle ansatte dags dato (v_DimAnsættelse[AnsatDagsDato]=J, v_DimAnsættelse[AktuelRække]=J).



## Lederdashboard

Dashboardet indeholder data vedrørende ordinær ferie og 6. ferieuge for sidste, igangværende og kommende ferieår. 
I dashboard udstilles data om restferie på enkeltmedarbejderniveau tiltænkt ledere. 
Bruger (ledere) får her overblik over de medarbejdere og enheder, som denne i forvejen har adgang til via PersonaleWeb. Se [***brugerstyring***](./data_brugerstyring).

Hvor andet ikke er nævnt, vises alle lederens medarbejdere med en aktuel ansættelse pågældende dag. Har bruger adgang til medarbejdere på tværs af organisationsstruktur, kan der filtreres herpå, ligesom der kan filtreres på tværs af stillingshieraki—med undtagelse af laveste niveau, stillingskode/stillingskodetekst (’L4Code’ og ’L4Name’). 

Foruden brug af grunddata er oprettet views til brug for beregninger specifikt for dette tema. Se [***Resume af tabeller***](./data_ferie#resume-af-tabeller).     

<iframe src="https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HR%20Lederdashboard/Ferieafholdelse?RC:Toolbar=False" style="border:1px #000000 solid;" frameborder="1" height="435" width="100%"></iframe>

Feriedata opdateres dagligt. 



#### Beregninger

**Feriestatus (timer)** viser summen (*[Restferie]*) af alle ferieelementer i v_FactFerie[FerieTimer] med og uden løn. Summen af feriesaldo og forventet tilskrivning fratrukket summen af anvendt og planlagt ferie vises i kontekst af feriekategorier på individniveau (TJNR).

*[RestferieFlag]* bruges til at indikere, at medarbejder har restferie til snarligt afhold, som endnu ikke er planlagt. Denne beregnes som ratioen af restferiedage (korrigeret ift. den enkeltes beskæftigelsesdecimal) relativt antal dage til aktuel (valgt) afviklingsperiodes afholdelsesfrist. 
```DAX
--(fra [RestferieFlag])
...
VAR BeskDecKorr = IF ( BeskDec = 0, 1, BeskDec )
VAR RestferieDage = DIVIDE ( [Restferie], 7.4 * BeskDecKorr, 0 )
...
```
Har denne værdien ≥15% vises et ikon.

I **Antal medarbejdere fordelt på restferieintervaller** beregnes antallet af medarbejdere i buckets af antal restferietimer til afhold, _[AntalRestferieInterval]_. Øvre grænser inkluderet.











> Læs: [Inspiration til measuret _[AntalRestferieInterval]_](https://www.daxpatterns.com/dynamic-segmentation/)
> 
> | [**Daxpatterns.com \ Dynamic segmentation**](https://www.daxpatterns.com/dynamic-segmentation/) | <img src="Images/icons_ref/icon_daxpatterns.png" height="45" width="45"> | 


> Læs: [Inspiration til measuret _[Fravær – vægtede fuldtidsfraværsdage gnsnit 12 mdr Ikke anonymiseret]_](https://www.sqlbi.com/articles/rolling-12-months-average-in-dax/)
> 
> | [**Sqlbi.com \ Rolling 12 Months Average in DAX**](https://www.sqlbi.com/articles/rolling-12-months-average-in-dax/) | <img src="Images/icons_ref/icon_sqlbi.png" height="45" width="45"> |
> 
