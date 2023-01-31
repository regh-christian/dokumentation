# Generelt

##	Nomenklatur
Om konventioner for navngivning tabeller og measures. 


### Tabeller
- Navngives som fx v_DimAnsættelse
- Skabelon: [tabeltype]+[\_]+[Formål]+[BeskrivendeNavn]
- **Views** indledes med _v\__' 
  - Alt *efterfølgende* skrives i camelCase (**S**tort**B**egyndelsesbogstav)
  - **Formål**: Dim, Fact, Security, Info, Slicer, Tally.
  - Alt efterfølgende er entydig(e) og letforståelig(e) *substantiv(er)*. (Gerne noget der minder om rådatatabellens oprindelige navn hvis muligt). Sammensatte ord skrives i camesCase

- **Tabeller** navngives som views. [Formål]+[BeskrivendeNavn]. Fx DimLønart
  - 'v_' udelades
- **Kolonnenavne** er entydige og letforståelige *substantiver*

- **Stored procedures** navngives kort og præcist beskrivende fx 'DanskeHelligdage'
  - Brug så vidt muligt verber
  - camelCase

- æ, ø, å tilladt

### Measures
- Navngives fx [Fravær - vægtede fuldtidsfraværsdage]. 
- Skabelon: [tema]+[-]+[beskrivende og letforståelig tekst]
  - Kombinationer af substantiver og verber er
- Measures grupperes i temaspecifikke mapper
  - Enkelte basis-measures placeres i mappen _Diverse_, hvis de bruges på tværs af temaer. Fx [Antal fuldtidsansatte] og [FilterSlicer]
- æ ø å tilladt
- Ikke alle measures er navngivet efter denne konvention. Ved tvivl se mappen _Sygefravær_



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
