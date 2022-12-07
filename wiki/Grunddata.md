# Grunddata

## Datamodel
![Power BI-model, grunddata](https://raw.githubusercontent.com/DataOgDigitalisering/dokumentation/master/Images/cube_model_basis.png)

## Relationer

|     FRA                         |                                  |      CF      |     TIL                             |             |     KARDINALITET    |     AKTIV    |
|---------------------------------|----------------------------------|--------------|-------------------------------------|-------------|---------------------|--------------|
|     v_SecurityRettigheder       |     [OrganisationID]             |     ↔        |     v_SecurityOrganisationBridge    |     [ID]    |     *:1             |     J        |
|     v_DimAnsættelse             |     [PersonID]                   |     ↔        |     v_DimPerson                     |     [ID]    |     *:1             |     J        |
|     v_DimAnsættelse             |     [OrganisationsID]            |     →        |     v_DimOrganisation               |     [ID]    |     *:1             |     J        |
|     v_DimAnsættelse             |     [NuværendeOrganisationID]    |     →        |     v_DimOrganisation               |     [ID]    |     *:1             |     N        |
|     v_DimAnsættelse             |     [StillingsID]                |     →        |     v_DimStilling                   |     [ID]    |     *:1             |     J        |
|     v_DimAnsættelse             |     [NuværendeOrganisationID]    |     →        |     v_SecurityOrganisationBridge    |     [ID]    |     *:1             |     J        |
