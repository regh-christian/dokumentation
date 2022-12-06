# Introduktion
**Data og Rapportering** er en sektion under enheden **Data og Digitalisering** i **Center for HR og Uddannelse (CHRU)**. Sektionen udstiller data og leverer ledelsesinformationer til regionens personaleledere og topledelser i form af Power BI Dashboards. Tilmed understøtter sektionen andre dele af CHRU med dataanvendelse (intern styring, indsigter, erkendelser, mv.). Sektionen varetager også *postkassehenvendelser* internt fra hele regionen, men også fra eksterne parter (spørgsmål og aktindsigter fra politikere, og journalister). 
Sektionen har hovedsageligt tekniske medarbejdere med kompetencer i dataudtræk og datafremstilling (7) og økonomi medarbejdere (2), samt ledelsesmedarbejdere (2) til at understøtte disse.

### Formål
Denne wiki er tiltænkt som introduktionsmateriale til nye medarbejdere og som et opslagværk til erfarne medarbejdere hos Data og Rapportering. De forskellige sektioner og sider er underbygget øvelser som kan findes i dette private *[repository](https://github.com/DataOgDigitalisering/FortroligInformation)*.

### SOFTWARE OG OPSÆTNING
Følgende software er en forudsætning og kan findes i Softwareshoppen. Versioner pr. 2022-09-08.

* Microsoft SQL Server Management Studio v16.3 eller v18
* Power BI september 2021
* SQL Server Data Tools 15.9.3 Bundle
  * Dax Studio v2.14.0
  * Tabular Editor v2.13

Hvor specifikke emner med fordel kan gennemgås ifm. løsning af opgaver er relevant for forståelse i en given kontekst kan anbefales, vil link være indsat.

<!-- ![Icons](https://raw.githubusercontent.com/ElvinIruthayam/elviniruthayam.github.io/master/Images/IconArrayWBR.png) -->

| Server        | Second Header | Beskrivelse |
| ------------- | ------------- |-------------|
| Content Cell  | Content Cell  |             |
| Content Cell  | Content Cell  |             |


> ### **ØVELSE 1 - TJEK AF ADGANGE**
> Denne øvelse går ud på at teste adgange. Følg nedenstående link til et SQL-script. Åbn og eksekver dette script i SQL Server Management Studio for at undersøge om du har alle nødvendige adgange. Kørslen bør tage omkring 10-15 minutter og returnere en tabel, der beskriver dine adgange.
>*[Link til SQL-SCRIPT](https://github.com/DataOgDigitalisering/FortroligInformation/blob/main/%C3%98velse1/ex_dataadgange.sql)*.

> ### **ØVELSE 2 - BRUG AF ADGANGE**
> Hospital XXX ønsker optælling af alle pr. dd. månedslønnede hhv. fuld- og deltidsansatte sygeplejersker udspecificeret for alle sygeplejegrupper under fællesbetegnelsen STILKO2TXT = ’Sygepl. Pers’
Hvor mange af disse er autoriserede med speciale indenfor anæstesi?

