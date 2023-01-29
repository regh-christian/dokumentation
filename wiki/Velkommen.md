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
I kuben processeres data fra mange forskellige kilder til en række forskellige formål. Kuben er datamodel i bl.a. 'HR Lederdashboard' og 'Strategisk Dahboard'. I disse implementeres vores standardiserede beregningsmetoder indenfor temaer som sygefravær, ferieafholdelse, personalesammensætning, løn, vagtplan m.fl.
På produktions- og udviklingsserverne findes data under skemaet, **chru_cube**. I Tabular Editor findes kuben under navnet CHRU_HRKube.




### Tabeller
|-------------------|-------------------|-------------------|-------------------|--------------------|
|     Grunddata     |       Temaer      |   Brugerstyring   |     Gruppering    | Serviceinformation |



Grunddata, temaer, brugerstyring, gruppering og serviceinformation.

Al data i kuben tilhører én af følgende typer:
- **Dimensionstabeller**. Tabeller hvis navn starter v_Dim... Nogle er næsten identisk kildedata såsom organisations- og stillingshieraki. Andre er mere komplekse og lavet specifikt til enkelte temaer. 
- **Fact-tabeller**
- **Tally-tabeller** er støttetabeller, som anvendes til sortering og inddeling af fx alder i ønskede intervaller. Bruges på figurakser og i diagrammer til at kontrollere grupperinger.
- **Slicer tabeller** anvendes som bridge-tabeller. 
- **Brugerstyring**. Tabeller med navnet v_Security... indgår i måden hvorpå, data filtreres afhængig af hvilken bruger, der tilgår kuben. Det er vigtigt at kende til disse tabeller, og hvordan de anvendes, da 
- **Info**



### Measures



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





