# Velkommen hos Data & Rapportering

<!-- Embed iFrame. PowerPoint: "lntroduktion og onboarding 2022" s.18-21 -->
<center>
<iframe src="https://regionh-my.sharepoint.com/personal/stefan_sajin-henningsen_regionh_dk/_layouts/15/Doc.aspx?sourcedoc={9eae6cfa-732f-48a1-81f3-246e3b6a2e86}&amp;action=embedview&amp;wdAr=1.7777777&showNavigation=FALSE&wdStart=18&wdEnd=21" width="850" height="503" frameborder="0" seamless="TRUE" start="18" end="21"></iframe>
</center>
<br>



# Introduktion til CHRU_HRKube

## ***Kuben***
I dit arbejde får du brug for at kende og kunne arbejde med data i "**kuben**" (med kuben menes—indtil videre—CHRU_HRKube). 

I kuben processeres data fra mange forskellige kilder til en række forskellige formål. Kuben er datamodel i bl.a. 
<a href="https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HR%20Lederdashboard/HR%20Lederdashboard?RC:Toolbar=False" target="_blank">___HR Lederdashboard___</a> 
og 
<a href="https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HR%20Strategisk%20Dashboard/HR%20Strategisk%20Dashboard?RC:Toolbar=False" target="_blank">___HR Strategisk Dashboard___</a>. 
I disse implementeres vores standardiserede beregningsmetoder indenfor temaer som sygefravær, ferieafholdelse, personalesammensætning, løn, vagtplan m.fl. 
Flere temaer tilføjes løbende i takt med efterspørgsel og tilgængelige datakilder, hvorfor vi altid bestræber os på, at kuben skal være en adaptiv størrelse. Hér er din indsigt i både muligheder og begrænsninger med den nuværende model værdifuld, da vi altid gerne vil kunne tilføje nye temaer *og* samtidig bevare muligheden for at foretage vores nuværende beregninger.

På produktions- og udviklingsserverne findes data under skemaet, **chru_cube**. I Tabular Editor findes kuben under navnet **CHRU_HRKube**.

<details><summary markdown="span">Kuben pr. 2023-01-30</summary>
 <center>
  <img src="Images/chru_hrkube_pbi_erd.png" height="480" weight="800" alt="CHRU_HRKube, 2023-01-30" style="vertical-align:middle"/>
 </center>
</details>
<br> 
Som introduktion til kuben kan anbefales at gennemgå materialet hér på siden ét tema ad gangen. Husk dog, at denne data er processeret til brug i datamodellen, hvorfor det er en nødvendighed, at du også har indblik i rådata fra fx Silkeborg Data (**SD**). 
Du kan med fordel prøve at bygge din egen version af kuben i Power BI; tabel for tabel; measure for measure; figur for figur. 

Til hvert tema findes små **øvelser**, hvor du kan testen din viden. Du vil herigennem få lidt erfaring med datatræk fra både rådata og kuben og få indblik i nogle af de datatransformationer, som nødvendigvis må foretages, før vi kan foretage beregninger.
<br>



### Tabeller

I grove træk falder al data i kuben indenfor kategorierne

| **Grunddata** | **Temaespecifik data** | **Hjælpetabeller** | **Infotabeller** |
 
- Med **grunddata** menes den data, som er fællesmængde på tværs af temaer, uanset om der beregnes på sygefravær, ferieafholdelse eller andet. I grunddata indgår:
  - **Stamdata** (personaledata, organisations-, stillings- og lønarthieraki, tidstabel mm.) 
  - **Brugerstyring** (data om personales brugerroller, som er bestemmende for hvilke data, brugere af vores produkter må se) 
- **Temaspecifik data** er data, der anvendes specifikt til beregning på fx sygefravær, ferieafholdelse eller personalesammensætning. Af screenshottet herover ses, hvordan vores grunddata groft kan grupperes i temaer.

Derudover findes en række øvrige tabeller, herunder:
- Hjælpetabeller er **tally**- og **slicer**-tabeller. Disse bruges til definereing af intervaller (fx aldersintervaller), grupperings-, sorterings- og filtreringsmuligheder
- Info er data som fx dato på **dataleverancer** og **servicemeddelelser** til brugere af dashboard om nye opdateringer eller tilføjelser
<br>



### Measures
Via measures implementeres vores standardiserede beregningsmetoder. De er grupperet temavis. I gruppen ___Diverse_ indgår beregninger på stamdata og dette kan være et godt sted at starte med at skabe overblik over kuben. Foruden indblik i de mere centrale tabeller vil du hér se eksempler på, hvordan viden om stamdata er essentielt for at kunne definere fx hvilken personalegruppe, der skal indgå i en given beregning; hvordan dette kan variere fra tema til tema; hvordan antallet af personer, der indgår i en beregning, er afgørende for, om et resultat nødvendigvis skal anonymiseres mm.

Andre measures—lokaliseret i mapperne, ___Farver_ og ___Tekster_—bruges til kontrol af layout på dashboards; Farvetemaer, akselængder, dynamiske tekstetiketter, meddelelser, kolonnebredder mm. 



# Software
Følgende software er en forudsætning og kan findes i <a href="https://softwarecentral.regionh.top.local/Shop" target"_blank">Softwareshoppen</a>. Hav altid nyeste version af Power BI Desktop installeret for at sikre funktionalitet med raportservere. For versioner af øvrig software spørg en kollega, hvad de bruger.

- Microsoft SQL Server Management Studio
- Power BI desktop
- SQL Server Data Tools Bundle
  - Dax Studio
  - Tabular Editor
<br>



# Dataadgange

<!-- Embed iFrame. word-doc: "Guide til bestilling af adgange.docx" på OneDrive-->
{::options parse_block_html="true" /}
<details><summary markdown="span">Guide til bestilling af dataadgange</summary>
<center>
<iframe src="https://regionh-my.sharepoint.com/personal/stefan_sajin-henningsen_regionh_dk/_layouts/15/Doc.aspx?sourcedoc={c652f92d-8025-4f11-9b4c-3e0f0e0dadba}&amp;action=embedview&amp;wdEmbedCode=0&amp;wdPrint=0&wdToolbar=FALSE" width="100%" height="700" frameborder="0" seamless="yes"></iframe>
</center>
</details>
{::options parse_block_html="false" /}
<br>



<!-- Embed iFrame. PowerPoint: "SQL-kursus.pptx" på OneDrive-->
{::options parse_block_html="true" /}
<details><summary markdown="span">Slides til SQL-kursus</summary>
<center>
<iframe src="https://regionh-my.sharepoint.com/personal/stefan_sajin-henningsen_regionh_dk/_layouts/15/Doc.aspx?sourcedoc={ee7ec7a1-d13c-4855-a459-c1717f9aa646}&amp;action=embedview&amp;wdEmbedCode=0&amp;wdPrint=0&wdToolbar=FALSE" width="100%" height="700" frameborder="0" seamless="yes"></iframe>
</center>
</details>
{::options parse_block_html="false" /}
<br>



> **ØVELSE — KONTROL AF DATAADGANGE**
> - Følg <a href="https://github.com/DataOgDigitalisering/FortroligInformation/blob/main/%C3%98velse1/ex_dataadgange.sql" target="_blank"> dette link til SQL-script</a>.
> - Åbn og eksekver scriptet i SQL Server Management Studio. Kørslen bør tage omkring 10-15 minutter og returnere en tabel, der beskriver dine adgange. Spørg en kollega om du har de adgange, du har brug for.
<br>


<!-- SKAL FLYTTES -->
> **ØVELSE — BRUG AF ADGANGE**
> Hospital XXX ønsker optælling af alle pr. dd. månedslønnede hhv. fuld- og deltidsansatte sygeplejersker 
> udspecificeret for alle sygeplejegrupper under fællesbetegnelsen STILKO2TXT = ’Sygepl. Pers’
> Hvor mange af disse er autoriserede med speciale indenfor anæstesi?






