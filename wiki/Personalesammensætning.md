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
<center>
![Power BI-model af personalesammensætning](https://raw.githubusercontent.com/DataOgDigitalisering/dokumentation/master/Images/cube_model_personalesammensætning.png)
</center>


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

![Lederdashboard, info om personalesammensætning](https://raw.githubusercontent.com/DataOgDigitalisering/dokumentation/master/Images/dashboard_personalesammensætning_info.png)

- Overordnet om beregninger 
  - Ikke decideret gennemgant af measures, da dette vil medføre omstændig vedligehold af hjemmesiden


## Measures
- Measures dokumenteres grundigt direkte i kuben, så de er selvforklarende, når de importeres på siden hér
<br>



> **ØVELSE — BRUG AF ADGANGE**
> 
> Hospital XXX ønsker optælling af alle pr. dd. månedslønnede hhv. fuld- og deltidsansatte sygeplejersker 
> udspecificeret for alle sygeplejegrupper under fællesbetegnelsen STILKO2TXT = ’Sygepl. Pers’
> Hvor mange af disse er autoriserede med speciale indenfor anæstesi?



