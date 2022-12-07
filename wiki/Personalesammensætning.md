# Personalesammensætning

## Datamodel
![Power BI-model af personalesammensætning](https://raw.githubusercontent.com/DataOgDigitalisering/dokumentation/master/Images/cube_model_personalesammensætning.png)


## Relationer

|     FRA                         |                                  |      CF      |     TIL                             |               |     KARDINALITET    |     AKTIV    |
|---------------------------------|----------------------------------|--------------|-------------------------------------|---------------|---------------------|--------------|
|     v_SecurityRettigheder       |     [OrganisationID]             |     ↔        |     v_SecurityOrganisationBridge    |     [ID]      |     *:1             |     J        |
|     v_DimAnsættelse             |     [PersonID]                   |     ↔        |     v_DimPerson                     |     [ID]      |     *:1             |     J        |
|     v_DimAnsættelse             |     [OrganisationsID]            |     →        |     v_DimOrganisation               |     [ID]      |     *:1             |     J        |
|     v_DimAnsættelse             |     [NuværendeOrganisationID]    |     →        |     v_DimOrganisation               |     [ID]      |     *:1             |     N        |
|     v_DimAnsættelse             |     [StillingsID]                |     →        |     v_DimStilling                   |     [ID]      |     *:1             |     J        |
|     v_DimAnsættelse             |     [NuværendeOrganisationID]    |     →        |     v_SecurityOrganisationBridge    |     [ID]      |     *:1             |     J        |
|     v_FactHændelser             |     [Hændelsesdato]              |     →        |     v_DimTidDato                    |     [Dato]    |     *:1             |     J        |
|     v_FactHændelser             |     [HændelsesID]                |     →        |     v_DimHændelser                  |     [ID]      |     *:1             |     J        |
|     v_FactHændelser             |     [StillingsID]                |     →        |     v_DimStilling                   |     [ID]      |     *:1             |     J        |
|     v_FactHændelser             |     [HændelseOrganisationsID]    |     →        |     v_DimOrganisation               |     [ID]      |     *:1             |     J        |
|     v_FactHændelser             |     [HændelseOrganisationsID]    |     →        |     v_SecurityOrganisationBridge    |     [ID]      |     *:1             |     J        |


## Resume af tabeller
## Dashboard
## Measures
