## Dashboard

<center><img src="Images/dashboard_personalesammensætning_info.png" height="95%" width="95%" alt="Lederdashboard, info om personalesammensætning" class="center"/></center>



### Lederdashboard
I dashboardet udstilles data på enkeltmedarbejderniveau tiltænkt ledere. 
Bruger (ledere) får her overblik over udvalgte parametre om de medarbejdere, denne i forvejen har adgang til via PersonaleWeb. Se >>Brugerstyring<<. Som udgangspunkt vises alle lederens medarbejdere med en aktuel ansættelse pågældende dag. Har bruger adgang til medarbejdere på tværs af organisationsstruktur, kan der filtreres herpå, ligesom der kan filtreres på tværs af stillingshieraki—med undtagelse af laveste niveau, stillingskode/stillingskodetekst.
Foruden brug af grunddata er oprettet views til brug for beregninger i dette tema. Se >>TABELLER<<. 



### Beregninger

Measuret, [Antal medarbejdere], summerer antallet af rækker i v_DimAnsættelse. Denne sum anvendes i flere andre measures i tilpassede filterkontekster. Antallet af henholdsvis månedslønnede, fuldtidsansatte, deltidsansatte og timelønnede fremgår af visninger (cards) til venstre, og er alle baseret på [Antal medarbejdere] evalueret i forskellige filterkontekster bestemt ved de dikotomiserede J/N-kolonner i v_DimAnsættelser.
Antal [Årsværk], vist sammen med de fire føromtalte, beregner summen af beskæftigelsesdecimaler, v_DimAnsættelse[Beskdec], som i definitionen er andelen af en 37 timers arbejdsuge en person er ansat til, og her i praksis oversættes til årsværk.
Specifikt på visningen af årsværk filtreres v_DimAnsættelse[Månedslønnede]=’J’.
     [Ansættelseslængde] er et andet measure, der anvendes af flere andre. Det beregner antallet år fra en persons ansættelsesdato i kommunen (hvis en sådan findes) til i dag. 
I figuren Ansættelseslængde anvendes dette measure til at beregne medarbejdernes ansættelseslængde i nuværende stilling, [AnsatteAnsættelseslængdeInterval]. Dette measure beregner [Ansættelseslængde] evalueret i en filterkontekst af tjenestenummer (og AnsættelsesID for at sikre optælling på unikke individer) og returnerer [Antal medarbejdere] med en given ansættelseslængde. Brugt i en visning sammen med v_TallyAnsættelseslængde opgøres antallet af medarbejdere med an ansættelseslængde i hvert af disse intervaller. 
Ved mouse-over på Ansættelseslængde vises en oversigt over de enkelte medarbejdere inden for hvert ansættelseslængdeinterval. Dette ved at bruge measuret [Ansættelseslængde] i en tabel med navn og PersonID, hvor filterkontekst da er det unikke PersonID.
     Figuren Aldersfordeling viser antallet af medarbejdere, [AnsatteAldersinterval], fordelt på aldersintervaller. Metodisk er beregning og opbygning af measure identisk med førnævnte, [AnsatteAnsættelseslængdeInterval]. I measuret, [AnsatteAldersinterval], indgår measuret [Alder] dog i stedet for [Ansættelseslængde] ligesom der nu optælles [Antal medarbejdere] pr. ’Aldersinterval’ i stedet for pr. ’Ansættelseslængde’. Measuret, [Alder], beregner blot en persons alder som differencen mellem dags dato og dennes fødselsdag angivet i v_DimPerson. 
Specifikt på denne figur er nedre og øvre aldersintervaller filtreret fra via v_TallyAlder[AldersInterval_5år], så kun intervaller mellem 15 og 79 år vises.
Ved mouse-over på Aldersfordeling vises en oversigt over de enkelte medarbejdere inden for hvert aldersinterval. Dette ved at bruge measuret [Alder] i en tabel med navn og PersonID, hvor filterkontekst da er det unikke PersonID.
     Tabellen Hændelser 14 dg. frem viser en kronologisk oversigt over udvalgte personrelaterede hændelser i de kommende 14 dage, fx fødselsdage, jubilæer, orlov, tiltrædelse, fratrædelse m.m. ____ 
     Tabellen Hændelser 12 mdr. frem er identisk med førnævnte, men med filteret, 0≤ v_DimTidDato[OffsetDato] ≤365.
     Tabellen Informationstabel viser en generel oversigt over udvalgte medarbejderdata i forbindelse med den aktuelle ansættelse, herunder afdeling, afsnit, tjenestenummer, stilling, alder, ansættelseslængde, anciennitet m.m. Ancienniteten af baseret på de ansattes nuværende anciennitetsdato, som både kan ligge forud for ansættelse i Region H ifm. tilskrivning af relevant anciennitet, eller som kan være ændret ifm. oprykning eller overflytning til anden overenskomst. Visse ansættelser med fastlåst anciennitetsforløb kan være uden anciennitetsdato. Measuret [Anciennitet] beregner, i antal hele år, differencen mellem v_DimAnsættelse[Anciennitetsdato] og dags dato. For fremtidige anciennitetsdatoer anvendes dags dato.
[Ansat til] angiver en eventuel slutdato på den aktuelle ansættelse. [Alder], [Ansættelseslængde] og [Årsværk] beregner, i kontekst af unikke personer (her i kontekst af kombinationen v_DimPerson[Navn] og v_DimAnsættelse[Tjnr]) pågældende persons alder og ansættelseslænge på dagen i hele antal år samt beskæftigelsesdecimalen/årsværk på pågældende ansættelse.
