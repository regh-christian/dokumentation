# Generelt

##	Nomenklatur
Om konventioner for navngivning tabeller og measures. 


### Tabeller
- Navngives som fx v_DimAnsættelse
- Skabelon: [tabeltype]+[\_]+[Formål]+[BeskrivendeNavn]
- **Views** indledes med __v\___ 
  - Alt *efterfølgende* skrives i camelCase (**S**tort**B**egyndelsesbogstav)
  - **Formål**: Dim, Fact, Security, Info, Slicer, Tally.
  - **Beskrivende navn** er entydig(e) og letforståelig(e) *substantiv(er)*. (Gerne noget der minder om rådatatabellens oprindelige navn hvis muligt). Sammensatte ord skrives i camesCase

- **Tabeller** navngives som views. [Formål]+[**B**eskrivende**N**avn]. Fx DimLønart
  - __v\___ udelades

- **Kolonnenavne** er entydige og letforståelige *substantiver*
  - camelCase

- **Stored procedures** navngives kort og præcist beskrivende fx 'DanskeHelligdage'
  - Brug så vidt muligt verber
  - camelCase

- æ, ø, å tilladt


### Measures
- Navngives fx [Fravær - vægtede fuldtidsfraværsdage]. 
- Skabelon: [tema]+[-]+[beskrivende og letforståelig tekst]
  - Kombinationer af substantiver og verber er
  - (ikke nødvendigvis camelCase)
- Measures grupperes i temaspecifikke mapper
  - Enkelte basis-measures placeres i mappen _Diverse_, hvis de bruges på tværs af temaer. Fx [Antal fuldtidsansatte] og [FilterSlicer]
- æ ø å tilladt
- Ikke alle measures er navngivet efter denne konvention. Ved tvivl se mappen _Sygefravær_



##	Opgaver og ansvar

- Aftaler for ændringer i CHRU_HRKube
  - Roller. Hvem gør/har ansvar for hvad?
  - SQL, Dax, Git, dok
  - Intervaller

- Kontrol-scripts
  - Til hvad? Afdæk behov

- Oprydning
  - Hvordan implementerer vi vedtaget nomenklatur?
  - Prioriter. Hvad kan vi leve med?
    - slette ubrugte measures vs ændre æ ø å 

