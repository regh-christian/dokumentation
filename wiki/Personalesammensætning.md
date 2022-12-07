# Personalesammensætning

## Datamodel
![Power BI-model af personalesammensætning](https://raw.githubusercontent.com/DataOgDigitalisering/dokumentation/master/Images/cube_model_personalesammensætning.png)


## Relationer

|     FRA                             |                                  |      CF      |     TIL                             |                                  |     KARDINALITET    |     AKTIV    |
|-------------------------------------|----------------------------------|--------------|-------------------------------------|----------------------------------|---------------------|--------------|
|     v_SecurityRettigheder           |     [OrganisationID]             |     ↔        |     v_SecurityOrganisationBridge    |     [ID]                         |     *:1             |     J        |
|     v_SecurityOrganisationBridge    |     [ID]                         |     →        |     v_DimAnsættelse                 |     [NuværendeOrganisationID]    |     1:*             |     J        |
|     v_DimAnsættelse                 |     [PersonID]                   |     ↔        |     v_DimPerson                     |     [ID]                         |     *:1             |     J        |
|     v_DimAnsættelse                 |     [OrganisationsID]            |     →        |     v_DimOrganisation               |     [ID]                         |     *:1             |     J        |
|     v_DimAnsættelse                 |     [NuværendeOrganisationID]    |     →        |     v_DimOrganisation               |     [ID]                         |     *:1             |     N        |
|     v_DimAnsættelse                 |     [StillingsID]                |     →        |     v_DimStilling                   |     [ID]                         |     *:1             |     J        |
|     v_FactHændelser                 |     [Hændelsesdato]              |     →        |     v_DimTidDato                    |     [Dato]                       |     *:1             |     J        |
|     v_FactHændelser                 |     [HændelsesID]                |     →        |     v_DimHændelser                  |     [ID]                         |     *:1             |     J        |
|     v_FactHændelser                 |     [StillingsID]                |     →        |     v_DimStilling                   |     [ID]                         |     *:1             |     J        |
|     v_FactHændelser                 |     [HændelseOrganisationsID]    |     →        |     v_DimOrganisation               |     [ID]                         |     *:1             |     J        |
|     v_FactHændelser                 |     [HændelseOrganisationsID]    |     →        |     v_SecurityOrganisationBridge    |     [ID]                         |     *:1             |     J        |


## Resume af tabeller

### v_DimHændelser
Udvalgte hændelser vedrørende en ansættelse.
Ved en hændelse forstås en af følgende mærkedage: datoer for fødsels- og jubilæumsdage, til- og fratrædelser, flytter afdeling, termin samt orlov. 
Tabellen er niveaudelt i L1 og L2, hvor L2 er en uddybende tekst knyttet til den overordnede hændelse, L1; fx om ’Flytter afdeling’ er et skift fra- eller til en afdeling.


### v_FactHændelser
Alle hændelser i intervallet -7dage≤i dag≤13mdr. 
Bemærk, at tabellen er koblet på v_SecurityOrganisationBridge via HændelseOrganisationID.
- TILFØJ DOKUMENTATION I VIEW 
- OPSUMMÉR DEREFTER HER
PersonBase: Alle fødseldags- og jubilæumsdatoer på alle aktuelt ansatte tjnr
PersonLags: Evt. tidligere og efterfølgende ansættelser samt afd. Bruges til bestemmelse af skift mellem afdelinger/inst
MasterTable: Definér hændelser. Tiltrædelse hvis ansat=J og forskellig fra den tidligere. Aftrædelse hvis Ansat=N og forskellig fra den tidligere. SkiftFraAfdeling hvis OrganisationID forskellig fra det tidligere. SkiftTilAfdeling hvis OrganisationID forskellig fra det følgende. Går på orlov hvis statuskode=3 og denne er forskellig fra tidligere. Tilbage fra orlov hvis Statuskode {0,1} og før var 3. Definer unikt ID baseret på rækkenummer.


### v_TallyAlder

### v_TallyAnsættelseslængde


## Dashboard
[HR-Lederdashboard\Personalesammensætning](https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HRDashboard/Personalesammens%C3%A6tning?RC:Toolbar=False)

![Lederdashboard, info om personalesammensætning](https://raw.githubusercontent.com/DataOgDigitalisering/dokumentation/master/Images/dashboard_personalesammensætning_info.png)

- Overordnet om beregninger 
-   Ikke decideret gennemgant af measures, da dette vil kræve omstændig vedligehold af hjemmesiden


## Measures
- Measures dokumenteres grundigt direkte i kuben, så de er selvforklarende, når de importeres på siden hér
