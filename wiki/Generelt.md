# Generelt

##	Nomenklatur
Om konventioner for navngivning i hhv. measures og tabeller. 


### Tabeller
- **Views** navngives fx v_DimAnsættelse
  - 'v_' — fordi det er et view. Alt *efter* skrives i camelCase
  - Dim, Fact, Security, Info, Slicer, Tally afhængig af **formål**
  - Alt efter er entydig(e) og letforståelig(e) *substantiv(er)*. (Gerne noget der minder om rådatatabellens oprindelige navn hvis muligt). Sammensatte ord skrives i camesCase
  - **Kolonnenavne** er entydige og letforståelige *substantiver*
  - æ, ø, å tilladt

- **Tabeller** navngives som views. 'v_' udelades 
- **Stored procedures** navngive kort og præcist beskrivende fx 'DanskeHelligdage'
  - Brug så vidt muligt verber
  - camelCase


### Measures
- Measures grupperes temaspecifikke mapperne
- Enkelte basis-measures placeres i diverse, hvis de bruges på tværs af temaer. Fx [Antal fuldtidsansatte] og [FilterSlicer]
- Navngives [tema] - [beskrivende og letforståelig tekst]
-   Kombinationer af substantiver og verber er 






- Hvilket ord bruger vi; kolonner, variable, attributes, parametre osv. ? 
- Rækker, records, tuples? 
- SammansatteOrd eller sammensatte_ord. 
- Engelsk/dansk? Å eller aa. 
- Find afvigelser og list dem mhp. ensartning. 

## Dokumentation
Tjekliste til opdatering af dokumentation:
- Tilføj ny(e) tabel(ler) til afsnittet <a href="https://dataogdigitalisering.github.io/dokumentation/chru_cube" target="_blank">chru_cube</a>
- Tilføj side til hjemmeside hér. Se <a href="https://dataogdigitalisering.github.io/dokumentation/GuideTilHjemmeside" target="_blank">Guide til hjemmeside</a>
- Importer measures
- andet ???


##	Aftaler for ændringer i CHRU_CUBE

###	Fra Udvikling til produktion
- Beskriv arbejdsgang for migrering af chru_cube fra UDV til P02. 
(noter 2022-10-27<< dependencies, aktører, kongruens i views og tabeller (scripts, opdateringsdato, opdateringsjob)

###	Oprydning
Skal diskuteres og vedtages
- Kontrol-scripts. Til hvad? Afdæk behov.
- Roller. Hvem gør/har ansvar for hvad?
  - SQL, Git, dashboards, dokumentation
- Intervaller

<!--
##	Om CHRU_
OM KUBER. FORDELE VED AT ARBEJDE MED KUBER<<
Kuben består hovedsageligt af bearbejdet data fra SD. For at blive bekendt med kuben gennemgås her HR Ledelses-dashboardet’s afsnit ét for ét. Du anbefales selv at genskabe modellen i Power BI. 
Du vil i det følgende blive præsenteret for overvejelser ifm. databearbejdning, ETL og measures. 
•	Tabeller, views, measures mm. 
-->
