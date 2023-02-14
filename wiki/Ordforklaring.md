### Aktuel hovedansættelse
Den ansættelse som opfylder og sorterer højest på kriterierne, gældende dags dato; status 0, 1 eller 3; Månedslønnet; Fuldtid; Startdato. Dvs. et timelønnet ansættelsesforhold kan være en aktuel hovedansættelse, men kun hvis det ikke overlapper et månedslønnet. På samme måde kan en deltidsstilling være en aktuel hovedansættelse, men kun hvis dette ikke overlapper en anden fuldtidsansættelse.
```SQL
-- AktuelHovedansættelse, 07_FL_110_SD_DimAnsaettelse.sas
by cpr descending AktuelRække descending Ansat descending Månedslønnet descending Fuldtid descending start tjnr;
if first.cpr AND AktuelRække = 1 AND Ansat = 1 then AktuelHovedansættelse=1;
else AktuelHovedansættelse=0;
```


### Anciennitet
I SD har anciennitetsdato ikke en entydig betydning. Den kan betyde startdatoen i første ansættelse i regionen, men den kan også være resultat af omregnet anciennitet. Et bedre udtryk for en persons egentlige anciennitet i regionen er _AnsLængdeCPR_.


### Anonymitetsgrænse
I alle beregninger, hvor der er risiko for at kunne identificere enkeltindivider i små populationer—og hvor dette ikke er tilladt—implementeres en anonymitetsgrænse. I praksis maskeres et resultat, hvis dette er fundet pba. et antal af individer lavere en anonymitetsgrænsen. 

> Vær opmærksom på, at anonymitetsgrænsen kan variere med tema.


### Ansættelse
Et ansættelsesforhold med status ansat uden løn (0), ansat/genåbnet (1) eller midlertidigt ude af løn (3). Alle ansættelser er unikt kendetegnet ved en stillings-, tjeneste- og overenskomstkode, en lønklasse og afdeling. Ændring i én eller flere af disse forhold, medfører et nyt registreret ansættelsesforhold. Ansættelsens varighed opgøres fra 1. dag til den sidste dag i pågældende ansættelse begge inklusiv. 


### Ansættelse, aktuel
Med aktuel ansættelse henvises oftest til dét ansættelsesforhold, hvis start- og slutdato inkluderer dags dato og har status ansat/genåbnet (1), ansat uden løn (0) eller midlertidigt ude af løn (3).
Vi vælger denne population med [Ansat]=J og [AktuelRække]=J. 
```SQL
-- Aktuelrække, 07_FL_110_SD_DimAnsaettelse.sas
,(CASE
    WHEN today() BETWEEN START and SLUT THEN 1 ELSE 0 
  END) as AktuelRække length = 3,
```

```SQL
-- Ansat, 07_FL_110_SD_DimAnsaettelse.sas
,(CASE  
    WHEN STAT IN ('0', '1', '3') THEN 1 ELSE 0 
  END) as Ansat length = 3,
```
_Ikke at forveksle med_ [aktuel hovedansættelse](#aktuelhovedansættlse).


### Ansættelseslængde
I opgørelser om personalesammensætning henviser ansættelseslængde til perioden fra _ansættelsensdato_ i SD til dags dato.
```DAX
[Ansættelseslængde]:=
//Measure beregner ansættelseslængder
VAR __AnsaettelsesDato =
    MAX ( 'v_DimAnsættelse'[Ansættelsesdato] )
VAR Result =
    IF (
        NOT ( ISBLANK ( __AnsaettelsesDato ) ),
        INT (
            DIVIDE (
                DATEDIFF (
                    IF ( __AnsaettelsesDato > TODAY (), TODAY (), __AnsaettelsesDato ),
                    TODAY (),
                    DAY
                ),
                365.25,
                0
            )
        )
    )
RETURN
    Result
```


### Antal fuldtidsansatte
```DAX
CALCULATE (
    [Antal medarbejdere],
    'v_DimAnsættelse'[Månedslønnet] = "J",
    'v_DimAnsættelse'[Fuldtid] = "J"
)
```


### Antal månedslønnede
```DAX
CALCULATE (
    [Antal medarbejdere],
    'v_DimAnsættelse'[Månedslønnet] = "J"
)
```


### Antal timelønnede
```DAX
CALCULATE (
    [Antal medarbejdere],
    'v_DimAnsættelse'[Månedslønnet] = "N"
)
```


### Beskæftigelsessum
Sum af ansattes beskæftigelsesdecimaler. Anvendes flere steder direkte oversat i fx årsværk. Vi bruger denne i beregning af sygefravær til at eliminere evt intervariabilitet mellem fuld- og deltidsansatte


### Beskæftigelsessum, gennemsnitlig
Gennemsnitlig beskæftigelsessum på udvalgte nedslagsdatoer—d. 1. i en måned. 
Muligheden for at ansatte til- og aftræder eller skifter fra fuld- til deltid midt i en måned introducerer kompromisser, hvor vi må vælge imellem tunge og komplekse eller mere grovmaskede beregninger. Ét eksempel herpå er i beregning af det rullende gennemsnit af fuldtidsfraværsdage, hvor vi fx månedsvist d. 1. aggregerer indeværende månedens fravær relativt til beskæftigelsessum. Her antages, at beskæftigelsesdecimaler d. 1. er repræsentativ for hele perioden siden forrige nedslagsdato. 


### Deltidsansat
Alle i v_DimAnsættelse, som er månedslønnede og ikke er fuldtidsansatte.
```DAX
[Antal deltidsansatte] :=
CALCULATE (
    [Antal medarbejdere],
    'v_DimAnsættelse'[Månedslønnet] = "J",
    'v_DimAnsættelse'[Fuldtid] = "N"
)
```


### Fratrædelse
Kendetegnet ved et skift i ansættelsesstatus til enten emigreret/død (7), fratrådt (8) eller pensioneret (9) fra at have været enten ansat uden løn (0), ansat/genåbnet (1) eller midlertidigt ude af løn (3). Fratrædelsesdatoen er sidste dag i en ansættelsesperiode.


### Fravær
Fravær er defineret som registreret arbejdstid med én af lønarterne i v_DimLønartFravær[L2Code]. Vi skelner groft imellem tre kategorier af fravær: Sygefravær, barn syg og andet.


### Fraværsdag
En fraværsdag er defineret ved
antal fraværstimer = antal planlagte timer
Det er muligt at have delvise fraværsdage, hvor
(antal fraværstimer)/(antal planlagte timer )<1
En fraværsdag er ikke i sig selv et udtryk for antal timers fravær, men er relativ ift. personens beskæftigelsesdecimal.
Se også fuldtidsfraværsdag


### Fuldtidsansat
Ansættelser kendetegnet ved
```SQL
-- Fuldtid, 07_FL_110_SD_DimAnsaettelse.sas
,(CASE 
    WHEN BESKDEC >= 1 THEN 1 ELSE 0
  END) as Fuldtid length = 3,
```


### Fuldtidsfraværsdag
En fuldtidsfraværsdag er defineret ved 7,4 timers fravær svt. ét dagsværk. Enhed kan sammenlignes på tværs i organisationen uagtet forskelle i enkeltindividers beskæftigelsesdecimal.


### Hændelse
Pr. 2023-01-06 defineret som  en af følgende:

| Fødselsdag | Hvert år |
| Jubilæum | Ved år: 1, 5, 10, 15, 20, 25, 30, 35 |
| Tiltræder | Når der skiftes til statuskode 0, 1, 3 efter ikke at have eksisteret eller været fratrådt |
| Fratræder | Når der skiftes til statuskode 7, 8, 9 efter at have været ansat |
| Skifter fra anden afdeling | Når der skiftes fremkommer både skifter til og fra. Der tjekkes på AfdFmt. Denne er lidt speciel, da Skifter til-hændelsen kun kan ses så længe medarbejderen fortsat er ansat på afgivende afsnit |
| Skifter til anden afdeling | Når der skiftes fremkommer både skifter til og fra. Der tjekkes på AfdFmt. Denne er lidt speciel, da Skifter til-hændelsen kun kan ses så længe medarbejderen fortsat er ansat på afgivende afsnit |
| Terminsdato | Alle terminsdatoer |
| Går på orlov | Når der skiftes til statuskode 0 eller 1 efter at have været i 3 eller omvendt for til |
| Tilbage fra orlov | Når der skiftes til statuskode 0 eller 1 efter at have været i 3 eller omvendt for til |


### Jubilæum
Alle jubilæumsdatoer i 5-årsintervaller samt étårsjubilæum beregnes og betragtes som en hændelse.


### Måltal
Region H’s målsætning for fravær pr. 2023-01-20 er 11,7 dage/år.


### Månedslønnet
Ansættelser kendetegnet ved 
```SQL
-- Månedslønnet, 07_FL_110_SD_DimAnsaettelse.sas
,(CASE 
    WHEN DEL IN ('2', '6') OR BESKDEC = 0 THEN 0 ELSE 1
  END) as Månedslønnet length = 3,
```


### Orlov
Kendetegnet ved et skift i ansættelsesstatus fra midlertidigt ude af løn (3) til enten ansat/genåbnet (1) eller ansat uden løn (0). Eller omvendt.
 

### Standardpopulation
I bredeste forstand forstås ved v_DimAnsættelse[Standardpopulation]=J, alle [månedslønnede](#månedslønnet) og ikke-eksternt finansierede ansættelser.
```SQL
-- Standardpopulation, 07_FL_110_SD_DimAnsaettelse.sas
,(CASE  
    WHEN Månedslønnet = 0 THEN 0 
    WHEN EksterntFinansieret = 1 THEN 0 
    -- WHEN RelevantStilling = 0 THEN 0
    WHEN L1Code IN( '_S2_','0000','9007','9008') THEN 0
    ELSE 1 
  END) as Standardpopulation length = 3,
```
I praksis defineres populationer med filtre på faner i kombination med filter i beregninger og figurer. Kriterier for at indgå i populationen varierer med temaet; den tidslige dimension af en given analyse; samt hvem analysen er rettet imod.


### Tiltrædelse
Kendetegnet ved et skift i ansættelsesstatus fra ukendt, emigreret/død (7), fratrådt (8) eller pensioneret (9) til enten ansat uden løn (0), ansat/genåbnet (1) eller midlertidigt ude af løn (3). Tiltrædelsesdatoen er første dag i det nye ansættelsesforhold.


### Årsværk
I beregninger anvendes

$$ 1924 \frac{timer}{år} = 52 \frac{uger}{år} \cdot 37 \frac{timer}{uge} = 260 \frac{dag}{år} \cdot 7,4 \frac{timer}{dag} $$
