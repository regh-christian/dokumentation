# Grunddata

Med grunddata menes de tabeller, som foruden stamdata (v_DimPerson, v_DimAnsættelse, v_DimOrganisation, v_DimStilling), indgår på tværs af temaer, der behandles i chru_cube. Dvs. datotabellen, tabeller anvendt ved brugerstyring, tabel med info om seneste dataleverancer og tabel med servicemeddelelser.



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
[Flis2_LønHR_v2].[chru_cube].[v_DimTidDato] 
[Flis2_LønHR_v2].[DM_FL_HR].[DimDato]
Script fra Søren Jessen: ”07_FL_110_SD_DimAnsaettelse.sas” 
     Baseret på tidstabellen, [DM_FL_HR].[DimDato] med en række mindre modifikationer og enkelte tilføjelser. Å erstattes med aa i hht. >>konvention<< om navngivning af kolonner. Formater såsom Y2016-M06 erstattes af 2016-06 og enkelte nye variable indføres pba. eksisterende. Fx er >>’DagKortmaanedAar’<< (10. jan. 2016) sammensat af ’DagMåned’, ’MånedNavn’ og ’År’. 
     Enkelte ny kolonner beregnes; Kolonnen ’Arbejdsdag’ er tilføjet og udregner, om dato er en arbejds-, helligdag eller i en weekend (https://www.computerworld.dk/uploads/eksperten-guider/107-Beregning-af-arbejdsdage-og-skaeve-helligdage.pdf). 
Til beregning heraf anvendes tre stored procedures; funktionen chru_cube.DanskeHelligdage matcher dato mod kendte og faste helligdage og returnerer binært udfald; chru_cube.PaaskeDage implementerer Gauás algoritme til bestemmelse af påskedag det givne år, hvorefter også ikke-faste helligdage da bestemmes (antal dage efter påske); I den sidste bestemmes, om dato er i en weekend. I praksis kaldes første stored procedure med dato som argument. Hvis dato er en kendt helligdag, er output ’Helligdag’. I modsat fald kaldes næste procedure til bestemmelse af påskedag, hvorefter dato igen matches mod de ikke-faste helligdage. Ved fortsat manglende match kaldes sidste procedure til beregning af, om datoen er en lørdag eller søndag.
```sql
CASE WHEN chru_cube.DanskeHelligdage(Dato) = 1 THEN 'Helligdag'
     WHEN chru_cube.DanskeHelligdage(Dato) = 0 AND chru_cube.Arbejdsdage(Dato) = 0 THEN 'Weekend'
     ELSE 'Arbejdsdag' 
  END AS Arbejdsdag
     Kolonnen Filterdatoer markerer udvalgte nedslagsdatoer såsom ’I dag’, ’1. dag i året’ m.fl.  
```





### v_DimAnsættelse




### v_DimPerson
Dimensionstabel med ID som nøgle. Derudover navn og fødselsdato.

### v_DimStilling



### v_DimOrganisation


