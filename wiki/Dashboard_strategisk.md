# Strategisk dashboard

## Sygefravær

<iframe src="https://flis.regionh.top.local:444/PBIReports/powerbi/L%C3%B8n%20og%20HR/HR%20Strategisk%20Dashboard/HR%20Strategisk%20Dashboard?RC:Toolbar=False" style="border:1px #000000 solid;" frameborder="1" height="565" width="100%"></iframe>

I dashboard udstilles aggregeret data om fravær på organisations- og stillingsniveau. 
Bruger får her overblik over organisationer, som denne i forvejen har adgang til via PersonaleWeb. Se >>Brugerstyring<<. I udgangspunktet beregnes på alle månedslønnede medarbejdere. 
     Fælles for alle beregninger er implementering af anonymitetsgrænsen. På intet tidspunkt må bruger kunne se aggregeret data, hvor mindre end dette antal personer udgør grundlag. Afhængig af beregningsmetode sikres dette på forskellig vis; Hvor beregninger baseres på løbende nedslagsdatoer (fig. 2, 3, 4, 7. [Fravær – vægtede fuldtidsfraværsdage gnsnit 12 mdr]) kontrolleres, at antal inkluderede individer på nedslagsdatoer er tilstrækkeligt. Hvis ikke udelades resultat på nedslagsdato. Hvor kun aktuelt ansatte dags dato indgår i beregning sikres, at antal aktuelt ansatte dags dato er tilstrækkeligt. 

>> Anonymitetsgrænsen bruges i alle measures til beregning af sygefravær til at forhindre identifikation af enkeltindivider.

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
