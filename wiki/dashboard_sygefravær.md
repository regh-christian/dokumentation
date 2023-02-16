# Sygefravær

## Lederdashboard

I dashboardet udstilles data på enkeltmedarbejderniveau tiltænkt ledere. 
Bruger får her overblik over udvalgte parametre om de medarbejdere, denne i forvejen har adgang til via PersonaleWeb. Se [brugerstyring](./Brugerstyring).

Fravær er bredt defineret ved lønarterne i v_DimLønartFravær[L1Code].
Der filtreres globalt _på fanen_ ud fra kriterierne v_DimAnsættelse[Ansat]=J, v_DimAnsættelse[Månedslønnet]=J og v_DimLønartFravær[L1Name]='Fravær'. Dog ikke sidstnævnte filter på fanen (bogmærket), **Fravær pr. type**, hvor samtlige fraværstyper opgøres.
	Som udgangspunkt vises altså alle lederens månedslønnede medarbejdere med en aktuel—eller historisk—ansættelse med statuskode 0, 1 eller 3, _hvis_ denne i opgørelsesperioden har registreret fravær i en af førnævnte lønarter. 
Har bruger adgang til medarbejdere på tværs af organisationsstruktur, kan der filtreres herpå, ligesom der kan filtreres på tværs af stillingshieraki—med undtagelse af laveste stillingsniveau.
Foruden brug af grunddata er oprettet views og tabeller til brug i beregninger under dette tema. Se [***Resume af tabeller***](./Sygefravær#resume-af-tabeller). 

<iframe src="https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HR%20Lederdashboard/Sygefrav%C3%A6r?RC:Toolbar=False" style="border:1px #000000 solid;" frameborder="1" height="435" width="100%"></iframe>


#### Beregninger
Fravær opgøres overordnet på én af to måder. 
Measure, [Fravær – antal arbejdsdage], summerer v_FactFravær[Arbejdsdage]. Med 'Arbejdsdag' menes hér en hel fraværsdag opgjort relativt til planlagt tid, $$ \frac{\text{fraværstimer}}{\text{planlagt arbejdstid}} $$.

I modsætning hertil vises fravær også som løbende gennemsnit—over korte eller længere perioder—af antal fuldtidsfraværsdage relativt til den, på opgørelsestidspunktet, gennemsnitlige beskæftigelsesdecimal. 

>	En fuldtidsfraværsdag er andelen af fraværstimer på en dag af en standard 7,4 timers arbejdsdag. Beskæftigelsesdecimal oversættes direkte til årsværk. ”Fuldtidsfraværsdage pr. årsværk” er da en normaliseret og vægtet enhed sammenlignelig på tværs af afdelinger, uanset at disse har forskellig sammensætning af fuld- og deltidsansatte samt varierende komposition i vagtlag.


I figur **Antal sygefraværsdage de seneste 12 mdr.** udregnes på individniveau, [Fravær – antal arbejdsdage på tværs af org]. I beregningen ophæves relationen v_DimAnsættelse[OrganisationsID]➔_DimOrganisation[ID] til fordel for v_DimAnsættelse[**Nuværende**OrganisationsID]➔_DimOrganisation[ID]. 



i filterkontekst af tid (v_DimTidDato[KortMaanedNavn] og person (v_DimPerson[ID]). Measure tilsidesætter den eksisterende relation, v_DimAnsættelse[OrganisationID]v_DimOrganisation[ID], og beregner [Fravær – antal arbejdsdage] (fraværstimer relativt til planlagt tid) i relationen, v_DimAnsættelse[NuværendeOrganisationID]v_DimOrganisation[ID]. 
```SQL
,CASE
   WHEN [Start] >= CONVERT(date, GETDATE()) THEN OrganisationsID
   ELSE
      (SELECT OrganisationsID 
       FROM DM_FL_HR.DimAnsættelse sub 
       WHERE 1 = 1
       AND sub.Tjnr = src.Tjnr 
       AND CONVERT(date, GETDATE()) BETWEEN sub.[Start] AND sub.Slut
      )
END AS NuværendeOrganisationID 
```
Dette, i kombination med filtret specifikt på denne figur, [Ansat på afdeling, nuværende afd]=1, gør, at bruger også ser data fra evt. tidligere ansættelser—på samme tjenestenummer! 
Desuden filtreres på v_DimAnsættelse[AnsatDagsDato]=’J’, hvorfor kun personer med an aktiv ansættelse dd. vises—også historisk. 


```DAX
[Ansat på afdeling, nuværende afd] =
--Er afhængig af den tilsvarende relation under "Relationships".
CALCULATE (
    [Ansat på afdeling],
    USERELATIONSHIP ( 'v_DimAnsættelse'[NuværendeOrganisationID],
    v_DimOrganisation[ID]
    )
)
```
og
```DAX
[Ansat på afdeling] =
VAR RelevantPersonID =
    MAX ( 'v_DimAnsættelse'[PersonID] )
VAR RelevantePersoner =
    FILTER (
        v_DimPerson,
        [ID] = RelevantPersonID
    )
VAR AnsatPaaAfdeling =
    IF ( COUNTROWS ( RelevantePersoner ) + 0 > 0, 1, 0 )
RETURN
    AnsatPaaAfdeling
```


<table>
<tr>
<th>Json 1</th>
<th>Markdown</th>
</tr>
<tr>
<td>
  
```json
{
  "id": 1,
  "username": "joe",
  "email": "joe@example.com",
  "order_id": "3544fc0"
}
```
  
</td>
<td>

```json
{
  "id": 5,
  "username": "mary",
  "email": "mary@example.com",
  "order_id": "f7177da"
}
```

</td>
</tr>
</table>








>> Inaktiv relation mellem v_DimAnsættelse[NuværendeOrganisationID] og v_DimOrganisation[ID] anvendes her til at beregne fravær på individniveau (tjenestenummer). v_DimAnsættelse[NuværendeOrganisationID] viser, på alle tidligere ansættelser med samme tjenestenummer, hvor personen aktuelt er ansat dags dato. Har en medarbejder flere ansættelser på tværs af organisation, og har bruger også adgang til data herfra, vises medarbejderens totale fravær.

Via bogmærket, Fravær pr. type, ses samme beregning i figuren  Antal fraværsdage de seneste 12 mdr. fordelt på fravær, hvor filterkontekst på tid og fraværstype (v_DimLønartFravær[L1Name]) er ophævet. Dermed ses akkumuleret antal fraværsdage i perioden, men nu fordelt på underkategori af fraværstyper, v_DimLønartFravær[L1Name]. Bruger kan til- og fravælge underkategorier via filter med standardindstillingen {”Sygefravær”, ”$56 fravær”, ”Arbejdsskade”, ”Graviditetsgener” }. 
Desuden filtreres på v_DimAnsættelse[AnsatDagsDato]=’J’, hvorfor kun personer med an aktiv ansættelse dd. vises. Desuden ’Ansat på afdelingen, nuværende afdeling’=1.
<br>

**FIGUR: Sygefravær de seneste 12 mdr. fordelt på intervaller**
Med [Fravær – antal i fraværsintervaller Ikke anonymiseret] beregnes fraværsdage (fraværstimer relativt til planlagt tid). Measure er designet til at beregne—i kontekst af v_TallyFraværsintervaller[Interval]—antallet af personer i perioden—på tværs af organisation—med antal fraværsdage indenfor definerede intervaller. (Intervaller inkluderer øvre grænseværdier.)

> | [**Daxpatterns.com \ Dynamic segmentation**](https://www.daxpatterns.com/dynamic-segmentation/) | <img src="Images/icons_ref/icon_daxpatterns.png" height="45" width="45"> | 
     
Filtrene, [Ansat på afdeling, nuværende afd]=1 og v_DimAnsættelse[AnsatDagsDato]=’J’, anvendes igen til beregning på personniveau på tværs af organisation, men kun hvis disse er ansat dags dato. 

>>	Tally-tabeller er i vores fulde kontrol, hvorfor vi relativt let kan konfigurere i intervalgrænser
<br>


**FIGUR: Gns. løbende sygefravær opgjort over seneste 12 mdr.**
Til beregning anvendes to measures, [Fravær – vægtede fuldtidsfraværsdage gnsnit 12 mdr Ikke anonymiseret] og [Fravær – Benchmark regionen 12 mdr.].

> | [**Sqlbi.com \ Rolling 12 Months Average in DAX**](https://www.sqlbi.com/articles/rolling-12-months-average-in-dax/) | <img src="Images/icons_ref/icon_sqlbi.png" height="45" width="45"> |

Førstnævnte udregner i kontekst af v_DimTidDato[MaanedAar] og valgt(e) organisationsniveau(er) (1) for hver d. 1. i måneden i foregående 12 måneder den løbende gennemssnitssum af beskæftigelsesdecimaler på aktuelle ansættelser pågældende datoer—også selvom de ikke er ansatte længere—, (2) månedlig sum af fuldtidsfraværsdage indeværende måned, v_FactFravær[Fuldtidsdage]. Endeligt (3) andel af fuldtidsfraværsdage af den gennemsnitlige beskæftigelsessum. (4) Kurven ’Aktuel visning’ viser dermed—beregnet i en månedskontekst—for hver måned det løbende gennemsnit af antal fuldtidsfraværsdage pr. årsværk opgjort over seneste 12 måneder.

	v_FactFravær[Fuldtidsdage] er antallet af fraværstimer på en dag relativt til en 7,4 timers arbejdsdag. Dette, i kombination med en gennemsnitssum af beskæftigelsesdecimaler, bruges til at normalisere beregning af fravær i enheden ”antal fuldtidsfraværsdage pr. årsværk”. Dermed muliggøres sammenligning af fravær på tværs af afdelinger, uanset at disse har forskellig sammensætning af fuld- og deltidsansatte samt varierende komposition i vagtlag.

Med figurspecifikt filter på v_DimTidDato[OffsetMaaned]=[-12;-1] udregnes kun på fravær i dette tidsinterval . 
     Til visning af ’Alle medarbejdere jf. rolle’ indgår foregående beregning i measure [Fravær – benchmark regionen 12 mdr.], hvor filterkonteksten v_DimOrganisation ophæves. Inkluderet i denne beregning er da alle personer i v_DimAnsættelse—på tværs af organisation—som bruger har adgang til. Anonymitetsgrænsen er implementeret.
     Regionens måltal på 11,7%  fravær er defineret som en ’Y-axis Constant Line’.
Via bogmærket, Sygefravær pr. måned, kan bruger se næsten samme beregning i Sygefravær de seneste 12 mdr. fordelt på måneder. I denne figur vises [Fravær – vægtede fuldtidsfraværsdage] opgjort pr. måned—fra sidste dag i sidst måned—og ikke et løbende gennemsnit, hvormed sæsonudsving bliver tydeligere. Metodik i measure er identisk med [Fravær – vægtede fuldtidsfraværsdage gnsnit 12 mdr Ikke anonymiseret] med undtagelse af specificering af opgørelsesperiode på én måned i stedet for 12 samt implementering af anonymitetsgrænsen, da der også tælles på historisk ansatte, som ikke skal kunne identificeres. 

**FIGUR: Gns. sygefravær de seneste 12 mdr. fordelt på afdelinger**
Også her anvendes det vægtede gennemsnit. Andel af normaliseret fravær (ift. 7,4timer/dag) relativt til gennemsnitlig sum af beskæftigelsesdecimal (opgjort pr. d. 1. hver måned i seneste 12 måneder). 
Beregnes ved først summering af og dernæst ratioen mellem hhv. [MaskeretFravSum12Mdr] og [MaskeretBeskDec12Mdr] i v_FactAggFravær. 
     Har bruger adgang til medarbejdere på tværs af organisation, vises en bar pr. afdeling (v_DimOrganisation[L4Name]).
     Med figurspecifikt filter på v_DimTidDato[OffsetMaaned]=-1 regnes 12 måneder bagud startende fra sidste hele måned.
     Via fanen, Sygefravær pr. afsnit, kan samme figur ses opgjort på laveste enhedsniveau. 
     
     
     
- -- -- - 
 
## Strategisk dashboard



<iframe src="https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HR%20Strategisk%20Dashboard/HR%20Strategisk%20Dashboard?RC:Toolbar=False&filterPaneEnabled=false&navContentPaneEnabled=false&$initialTab=Sygefrav%C3%A6r" style="border:1px #000000 solid;" frameborder="1" height="435" width="100%"></iframe>

I dashboard udstilles aggregeret data om fravær på organisations- og stillingsniveau. 
Bruger får her overblik over organisationer, som denne i forvejen har adgang til via PersonaleWeb. Se >>Brugerstyring<<. I udgangspunktet beregnes på alle månedslønnede medarbejdere. 
     Fælles for alle beregninger er implementering af anonymitetsgrænsen. På intet tidspunkt må bruger kunne se aggregeret data, hvor mindre end dette antal personer udgør grundlag. Afhængig af beregningsmetode sikres dette på forskellig vis; Hvor beregninger baseres på løbende nedslagsdatoer (fig. 2, 3, 4, 7. [Fravær – vægtede fuldtidsfraværsdage gnsnit 12 mdr]) kontrolleres, at antal inkluderede individer på nedslagsdatoer er tilstrækkeligt. Hvis ikke udelades resultat på nedslagsdato. Hvor kun aktuelt ansatte dags dato indgår i beregning sikres, at antal aktuelt ansatte dags dato er tilstrækkeligt. 

	Anonymitetsgrænsen bruges i alle measures til beregning af sygefravær til at forhindre identifikation af enkeltindivider.

Foruden brug af grunddata er oprettet views til brug for beregninger specifikt for dette tema. Se >>TABELLER<<.



#### **Beregninger**
Fravær er bredt defineret ved lønarterne i v_DimLønartFravær[L1Code]. 
Hvor andet ikke er nævnt, medregnes månedslønnede (v_DimAnsættelse[Månedslønnet]=’J’) ansatte (v_DimAnsættelse[Ansat]=’J’ svt. statuskode {0,1,3} ).
Filteret, v_DimLønartFravær[L1Name]=”Sygefravær”, på fanen Sygefravær sikrer, at kun denne fraværskategori medtages i beregninger her. Via bookmarks føres bruger til faner uden dette filter, hvor også øvrige kategorier vises. 
     I beregninger opgøres fravær overordnet på én af to måder;
Measure, [Fravær – antal arbejdsdage], summerer v_FactFravær[Arbejdsdage] og indgår som byggesten i en række andre measures i dette tema. En arbejdsdag er hér fraværstimer relativt til planlagt tid.
I modsætning hertil vises fravær også som løbende gennemsnit—over korte eller længere perioder—af antal fuldtidsfraværsdage relativt til den, på opgørelsestidspunktet, gennemsnitlige beskæftigelsesdecimal. En fuldtidsfraværsdag er andelen af fraværstimer på en dag af en standard 7,4 timers arbejdsdag. Beskæftigelsesdecimal oversættes direkte til årsværk. Denne, ”fuldtidsfraværsdage pr. årsværk”, er da en normaliseret og vægtet enhed, som er sammenlignelig på tværs af afdelinger, uanset at disse har forskellig sammensætning af fuld- og deltidsansatte samt varierende komposition i vagtlag.
     Fanespecifikt filter, v_DimStilling[Hovedstillingsgruppe],  fravælger grupperne ’Ansat uden løn’, ’Budgetteknik’, ’Politikere/tjm’, ’Samling’ og ’Ukendt’. v_DimLønartFravær[L1Name]=”Sygefravær” fjerner øvrige fraværstyper.
     Til figurerne 2, 3, 4 og 7 beregnes fravær, [Fravær – vægtede fuldtidsfraværsdage gnsnit 12 mdr], som løbende 12måneders gennemsnitssum af fuldtidsfraværsdage pr. årsværk—her er antal fraværstimer normaliseret til en fuldtidsdag på 7,4timer, ligesom antal medarbejdere opgøres som gennemsnitssum af beskæftigelsesdecimal. 
I figur 2 ses seneste 3 års løbende 12 måneders gennemsnit opgjort sidste dage i respektive år via filter på v_SlicerNedslagsdatoer. Med input fra slicer kan bruger vælge nedslagsdato, og dermed ændre filterkontekst for measure, [Fravær – vægtede fuldtidsfraværsdage gnsnit 12 mdr Benchmark]. Valgt nedslagsdato er bestemmende for, fra hvilken dato dette measure regner 12 måner baglæns. Altså en dynamisk størrelse til benchmarking af aktuelt fravær med historisk.
Figur 3 og 4 beregninger det vægtede gennemsnit 12 måneder bagud relativt til månederne, det er vist for, og figur 7 regner 12 måneder bagud fra dags dato. 
I Figurerne 5 og 6 beregnes fravær som andelen af fraværstimer af planlagt tid, [Fravær – antal arbejdsdage på tværs af org]. I figur 5  bruges dette i [Fravær – antal i fraværsinterval] til beregning af antal personer i perioden—og på tværs af organisation—med antal fraværsdage indenfor definerede intervaller. (Intervaller inkluderer øvre grænseværdier). Measure er designet til at indgå i kontekst af v_TallyFraværsintervaller[Interval]. I figur 6 indgår det i [Fravær – antal under regionalt niveau] og i kontekst af v_TallyFraværMaaltal til bestemmelse af antal personer antal fraværsdage—på tværs af organisation—over og under regionens måltal.

> | [**Daxpatterns.com \ Dynamic segmentation**](https://www.daxpatterns.com/dynamic-segmentation/ target="_blank") | <img src="Images/icons_ref/icon_daxpatterns.png" height="45" width="45"> |
 
 
 
 
 
 
 
 
