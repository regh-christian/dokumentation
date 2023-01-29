# Velkommen hos Data & Rapportering

<!-- Embed iFrame. PowerPoint: "lntroduktion og onboarding 2022" s.18-21   
&wdStart=1 - this parameter sets the starting page number of the embedded document
&wdEnd=10 - this parameter sets the ending page number of the embedded document
-->
<center>
<iframe src="https://regionh-my.sharepoint.com/personal/stefan_sajin-henningsen_regionh_dk/_layouts/15/Doc.aspx?sourcedoc={9eae6cfa-732f-48a1-81f3-246e3b6a2e86}&amp;action=embedview&amp;wdAr=1.7777777&showNavigation=FALSE&wdStart=18&wdEnd=21" width="850" height="503" frameborder="0" seamless="TRUE" start="18" end="21"></iframe>
</center>
<br>



# Introduktion til CHRU_HRKube

## ***Kuben***
I dit arbejde får du brug for at kende og kunne arbejde med data i "**kuben**" (med kuben menes—indtil videre—CHRU_HRKube). 
I kuben processeres data fra mange forskellige kilder til en række forskellige formål. Kuben er datamodel i bl.a. [___HR Lederdashboard___](https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HR%20Lederdashboard/HR%20Lederdashboard?RC:Toolbar=False) og [___Strategisk Dahboard___](https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HR%20Strategisk%20Dashboard/HR%20Strategisk%20Dashboard?RC:Toolbar=False). I disse implementeres vores standardiserede beregningsmetoder indenfor temaer som sygefravær, ferieafholdelse, personalesammensætning, løn, vagtplan m.fl.

På produktions- og udviklingsserverne findes data under skemaet, **chru_cube**. I Tabular Editor findes kuben under navnet CHRU_HRKube.

Som introduktion til kuben anbefales at gennemgå materialet hér på siden ét tema ad gangen. Du kan med fordel prøve at bygge din egen version af kuben tabel for tabel, measure for measure, figur for figur. I hvert tema findes små øvelser, hvor du kan testen din viden.



### Tabeller

I grove træk falder al data i kuben indenfor kategorierne

| **Grunddata** | **Temaespecifik** | **Hjælpetabeller** | **Info** |

- Med **grunddata** menes den data, som er fællesmængde på tværs af temaer, uanset om der beregnes på sygefravær eller ferieafholdelse. I grunddata indgår
  - **Stamdata** (personaledata, organisations- og stillingshieraki, tidstabel mm.) 
  - **Brugerstyring** (data om personales brugerroller, som er bestemmende for hvilke data, de må se) 
- **Temaspecifik data** er data, der indhentes specifik til beregning på fx sygefravær, ferieafholdelse eller personalesammensætning
- Hjælpetabeller er **tally**- og **slicer**-tabeller. Disse bruges til definereing af intervaller (fx aldersintervaller), grupperings-, sorterings- og filtreringsmuligheder
- Info er data som fx dato på **dataleverancer** og **servicemeddelelser** til brugere af dashboard om nye opdateringer eller tilføjelser

Det kan være en hjælp først at blive dus 
Kuben udvides løbende med nye datakilder, i takt med, at flere temaer skal kunne besvares.




### Measures
Via measures implementeres vores standardiserede beregningsmetoder. De er grupperet temavis. Gruppen diverse indeholder beregninger på stamdata og kan være et godt sted at starte med at skabe overblik over kuben. Foruden indblik i de mere centrale tabeller vil du hér se eksempler på, hvordan viden om stamdata er essentielt for at kunne definere fx hvilken personalegruppe, der skal indgå i en beregning afhængig af temaet; hvordan dette kan bruges til at regne i årsværk; hvordan antallet af personer, der indgår i en beregning, er afgørende for, om et resultat nødvendigvis skal anonymiseres mm.

Andre—measures—lokaliseret i mapperne, ___Farver_ og ___Tekster_—bruges til kontrol af layout på dashboards. Farvetemaer, akselængder, dynamiske tekstetiketter meddelelser, kolonnebredder mm. 




# Dataadgange
Du skal have adgang til ..........
<!-- Embed iFrame. word-doc: "Guide til bestilling af adgange.docx" på OneDrive-->
{::options parse_block_html="true" /}
<details><summary markdown="span">Guide til bestilling af dataadgange</summary>
<center>
<iframe src="https://regionh-my.sharepoint.com/personal/stefan_sajin-henningsen_regionh_dk/_layouts/15/Doc.aspx?sourcedoc={c652f92d-8025-4f11-9b4c-3e0f0e0dadba}&amp;action=embedview&amp;wdEmbedCode=0&amp;wdPrint=0&wdToolbar=FALSE" width="100%" height="700" frameborder="0" seamless="yes"></iframe>
</center>
<br>
</details>
{::options parse_block_html="false" /}



<!-- Embed iFrame. PowerPoint: "One pager - Design guidelines til rapporter i FLIS" på OneDrive-->
{::options parse_block_html="true" /}
<details><summary markdown="span">One pager — Design guidelines til rapporter i FLIS</summary>
<center>
<iframe src="https://regionh-my.sharepoint.com/personal/stefan_sajin-henningsen_regionh_dk/_layouts/15/Doc.aspx?sourcedoc={89bc13a3-2f00-4dec-a49c-49033d194d1a}&amp;action=embedview&amp;wdAr=1.7777777&showNavigation=FALSE&wdStart=18&wdEnd=21" width="1000" height="588" frameborder="0" seamless="TRUE" start="18" end="21"></iframe>
</center>  
<br>
</details>
{::options parse_block_html="false" /}





