# Introduktion


## Data og rapportering

**Data og Rapportering** er en sektion under enheden **Data og Digitalisering** i **Center for HR og Uddannelse (CHRU)**. Sektionen udstiller data og leverer ledelsesinformationer til regionens personaleledere og topledelser i form af Power BI Dashboards. Tilmed understøtter sektionen andre dele af CHRU med dataanvendelse (intern styring, indsigter, erkendelser, mv.). Sektionen varetager også *postkassehenvendelser* internt fra hele regionen, men også fra eksterne parter (spørgsmål og aktindsigter fra politikere, og journalister). 
Sektionen har hovedsageligt tekniske medarbejdere med kompetencer i dataudtræk og datafremstilling (7) og økonomi medarbejdere (2), samt ledelsesmedarbejdere (2) til at understøtte disse.

<center>
<iframe src="https://regionh-my.sharepoint.com/personal/stefan_sajin-henningsen_regionh_dk/_layouts/15/Doc.aspx?sourcedoc={9eae6cfa-732f-48a1-81f3-246e3b6a2e86}&amp;action=embedview&amp;wdAr=1.7777777&showNavigation=FALSE" width="800" height="480" frameborder="0" seamless="TRUE"></iframe>
</center>
<br>

Tjek [øvelse 1][#ex_dataadgange]

### Formål
Denne wiki er tiltænkt som introduktionsmateriale til nye medarbejdere og som et opslagværk til erfarne medarbejdere hos Data og Rapportering. 
Temaer er underbygget af øvelser, som kan findes i dette private *<a href="https://github.com/DataOgDigitalisering/FortroligInformation" target="_blank">repository</a>*.

### Software og opsætning
Følgende software er en forudsætning og kan findes i Softwareshoppen. Versioner pr. 2022-09-08.

* Microsoft SQL Server Management Studio
* Power BI desktop
* SQL Server Data Tools Bundle
  * Dax Studio
  * Tabular Editor
  
### Kilder og adgange
- L:\, ad-hoc
- [Link til Christians dokument]()

Hvor specifikke emner med fordel kan gennemgås via andre kilder fx ifm. løsning af opgaver eller kan bidrage med forståelse i en given kontekst, vil link til supplerende materiale være indsat.

<!--
![Icons](https://raw.githubusercontent.com/ElvinIruthayam/elviniruthayam.github.io/master/Images/IconArrayWBR.png) 

| Server        | Second Header | Beskrivelse |
| ------------- | ------------- |-------------|
| Content Cell  | Content Cell  |             |
| Content Cell  | Content Cell  |             |

-->


> <a name="ex_dataadgange">ØVELSE - KONTRAL AF DATAADGANGE</a> 
> Denne øvelse går ud på at teste adgange. Følg nedenstående link til et SQL-script. Åbn og eksekver dette script i SQL Server Management Studio for at undersøge om du har alle nødvendige adgange. Kørslen bør tage omkring 10-15 minutter og returnere en tabel, der beskriver dine adgange.
>*[Link til SQL-SCRIPT](https://github.com/DataOgDigitalisering/FortroligInformation/blob/main/%C3%98velse1/ex_dataadgange.sql)*.

> **ØVELSE - BRUG AF ADGANGE**
> Hospital XXX ønsker optælling af alle pr. dd. månedslønnede hhv. fuld- og deltidsansatte sygeplejersker 
> udspecificeret for alle sygeplejegrupper under fællesbetegnelsen STILKO2TXT = ’Sygepl. Pers’
> Hvor mange af disse er autoriserede med speciale indenfor anæstesi?
