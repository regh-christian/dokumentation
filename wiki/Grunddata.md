# Grunddata

## Datamodel
![Power BI-model, grunddata](https://raw.githubusercontent.com/DataOgDigitalisering/dokumentation/master/Images/cube_model_basis.png)

## Relationer

|     FRA                             |                                  |      CF      |     TIL                             |                                  |     KARDINALITET    |     AKTIV    |
|-------------------------------------|----------------------------------|--------------|-------------------------------------|----------------------------------|---------------------|--------------|
|     v_SecurityRettigheder           |     [OrganisationID]             |     ↔        |     v_SecurityOrganisationBridge    |     [ID]                         |     *:1             |     J        |
|     v_SecurityOrganisationBridge    |     [ID]                         |     →        |     v_DimAnsættelse                 |     [NuværendeOrganisationID]    |     1:*             |     J        |
|     v_DimAnsættelse                 |     [PersonID]                   |     ↔        |     v_DimPerson                     |     [ID]                         |     *:1             |     J        |
|     v_DimAnsættelse                 |     [OrganisationsID]            |     →        |     v_DimOrganisation               |     [ID]                         |     *:1             |     J        |
|     v_DimAnsættelse                 |     [NuværendeOrganisationID]    |     →        |     v_DimOrganisation               |     [ID]                         |     *:1             |     N        |
|     v_DimAnsættelse                 |     [StillingsID]                |     →        |     v_DimStilling                   |     [ID]                         |     *:1             |     J        |



## Resume af tabeller

### v_DimTidDato
Baseret på tidstabellen, [DM_FL_HR].[DimDato] med enkelte modifikationer. Å erstattes med aa i hht. konvention om navngivning af kolonner, formater såsom Y2016-M06 erstattes af 2016-06 og enkelte nye variable indføres pba. eksisterende. Fx er >>DagKortmaanedAar<< (10. jan 2016) sammensat af DagMåned, MånedNavn og År. 
Enkelte ny kolonner beregnes. Kolonnen Arbejdsdag er tilføjet og udregner, om dato er arbejds-, helligdag eller i en weekend. Til beregning heraf anvendes tre stored procedures; Én matcher dato mod kendte og faste helligdage; Én implementerer Gauss algoritme til bestemmelse af påskedag det givne år, hvormed ikke-faste helligdage da kan bestemmes (antal dage efter påske); I den sidste bestemmes, om dato er i en weekend. I praksis kaldes første stored procedure med dato som argument. Hvis dato er en kendt helligdag, er output ’Helligdag’. I modsat fald kaldes næste procedure til bestemmelse af påskedag, hvorefter dato igen matches mod de ikke-faste helligdage. Ved fortsat manglende match kaldes sidste procedure til beregning af, om datoen er en lørdag eller søndag.
Kolonnen Filterdatoer markerer udvalgte nedslagsdatoer såsom ’Den 1. i denne måned’, ’Den 1. i tidligere måneder’, ’I dag’, ’ For 12 måneder siden’ m.fl.  

### v_DimAnsættelse
Tabellen er baseret på SD-tabellen, SD_Person. ID er unik nøgle for det enkelte ansættelsesforhold, på samme måde som en sammensat nøgle dannet pba. cpr+institution+tjnr er det i SD_Person (ansaettelsesID). PersonID er pseudoanonymiseret nøgle henvisende til den enkelte person. En person kan have flere ansættelser på forskellige institutioner/afdelinger overlappende i tid—dog aldrig overlappende i tid på samme afdeling. I fald en person har flere samtidige ansættelser på samme afdeling, vil der være knyttet et særskilt tjenestenummer til hver af disse.
På den måde er tabellen både fact og dimension afhængig af kontekst; om vi henviser til ansættelser, der kan variere over tid eller data konkret om den enkelte person som fx alder, køn osv. Vi anvender fx denne sondring mellem (ansættelses-)ID og PersonID til at sikre, at en leder kun kan se data relevant for den aktuelle og aktive ansættelse, en person har, på den afdeling leder har beføjelser til at se data fra. Har en person fx flere samtidige ansættelser på forskellige institutioner, vil respektive leder€ i udgangspunktet kun kunne se data for ansættelsesforholdet relevant for dem. Se desuden afsnit 3.  Brugerstyring.
Baseret på den i SD_Person-tabellen datostyrede del, indfører vi nedenstående dikotomiserede variable mhp. lettere til- og fravalg i population for både os selv i opbygning af scripts og measures og for brugere af dashboards i form af slicere.
Fuldtid og Månedslønnet: Kolonnerne DEL og BESKDEC angiver, om et ansættelsesforhold er fuld- eller deltid hhv. måneds- eller timelønnet. Dette er opsummeret i kolonnerne Fuldtid og Månedslønnet hvor Fuldtid=J hvis BESKDEC>=0 og Månedslønnet=N hvis DEL=2eller6 OG BESKDEC=0.
BESKRIV AFVIGELSE MED LÆGEGRUPPE

**Ansat**: J hvis statuskode er enten 0, 1 eller 3.

**AktuelRække**: J hvis dags dato ligger indenfor ansættelsesforholdets start- og slutdato.

**AnsatDagsDato**: J hvis Ansat=J og AktuelRække=J. Dvs. J hvis statuskode er 0, 1 eller 3 og dags dato ligger indenfor ansættelsesforholdets start- og slutdato.

**AnsatSeneste14Mdr**: J hvis Ansat=J indenfor seneste 14 mdr. Dette for altid at kunne se ét helt år tilbage uanset tilfælde, hvor ansættelsesstart var fx midt- eller sidst i en måned.

**AktuelHovedansættelse**: Hvor flere ansættelsesforhold er sammenfaldende i tid, er ét og kun et en hovedansættelse.
Samtlige ansættelser arrangeres ud fra parametrene AktuelRække, Ansat, MånedsLønnet, Fuldtid og ansættelsesstartdato. Alle i faldende orden. Dette sikrer, at der i ethvert tidsrum hvor en person har et eller flere ansættelsesforhold uagtet beskæftigelsesgrad og aflønningsmetode, kan peges på ét og kun ét ansættelsesforhold som værende en hovedansættelse—nemlig ansættelsen jf. nævnte sortering, der desuden opfylder kriterierne Ansat=J OG AktuelRække=J.
En person kan i sin ansættelseshistorik have flere ansættelser, hvor AktuelHovedansættelse=J. I så fald vil de aldrig overlappe i tid.

**EksterntFinansieret**: J hvis afdelingen på ansættelsesstarttidspunktet var eksternt finansieret.  

**StandardPopulation**: Ansatte dags dato, der er månedslønnede, fuldtidsansatte og ikke eksternt finansierede.
 
**NuværendeOrganisationID**: Afdelingen, hvor ansættelsesforhold er gældende dags dato. Har en person ansættelsesforhold med fremtidig startdato på en anden afdeling, sættes afdelingen for det fremtidige ansættelsesforhold midlertidigt lig afdelingen på nuværende ansættelse.
Dermed sikrer vi, at det altid kun er den ledergruppe, der aktuelt har en person ansat, som kan se data om denne, når brugerstyringen er knyttet til denne kolonne.

**Hændelse**: Angiver om ansættelsesforholdet er en til- eller fratrædelse. Har ansættelsesforholdet værdien Ansat=J og denne er forskellig fra et evt. tidligere ansættelsesforhold, er dette en tiltrædelse. I modsat fald en fratrædelse.

**HændelseMellem**: Angiver om en hændelse er sket mellem afdelinger (samme institution) eller mellem to institutioner. I sidste tilfælde er modtagende institution anført i TilInst. Værdien ’Udenfor Region Hovedstaden’ hvis der på ansættelsesforholdet er enten til- eller fratrædelse og ingen af kriterierne ’mellem afdelinger’ eller ’mellem institutioner’ er opfyldt.
- BESKRIV Inst=2P (tjenestemandsfunktion)<<
I kuben er desuden indført unikke nøgler for organisation og stillingsbenævnelser, her hhv. OrganisationsID og StillingsID. 


### v_DimPerson
Dimensionstabel med ID som nøgle. Derudover navn og fødselsdato.

### v_DimStilling
Stillingshieraki i 4 niveauer (L1-L4) med ID som nøgle. Hoved-, fag- og stillingsgruppe samt stilling.
Desuden er stillingskoderne 1133-1139 og 1161 defineret (hard-codet i viewet) som uddannelseslæger. (Se UDV for øvrige definitioner af ledere og AtypiskeStillinger).
Current_row=1 sikrer opdateret stillingshieraki men ignorerer evt. historiske ændringer.

### v_DimOrganisation
Organisationshieraki i 6 niveauer (L1-L6) med ID som nøgle.



> #### ØVELSE – Tæl årsværk
> Udregn vha. data i databasen, LON_HR, hvor mange årsværk, der arbejdes i din sektion baseret på aktuelt ansatte i dag. 
> Gruppér dit resultat på time- hhv. månedslønnede, del- og fuldtidsansatte.
> Lav samme beregning og gruppering baseret på data fra kuben.
> (Har vi tilladelse til at bruge en specifik persons ansættelseshistorik?)	 


