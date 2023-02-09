# Sygefravær — KLADDE



## Datamodel



## Resume af tabeller

### v_DimLønartFravær

| **View** | **Baseret på** | 
| - | - |
| [chru_cube].[v_DimLønartFravær] | [DM_FL_HR].[DimLønart] |


### v_FactFravær

| **View** | **Baseret på** | 
| - | - |
| [chru_cube].[v_FActFravær] | [DM_FL_HR].[FactFravær] |


### v_FactAggFravær

| **View** | **Baseret på** | 
| - | - |
| [chru_cube].[v_FactAggFravær] | [DM_FL_HR].[FactFraværAfsnit] |
| | [chru_cube].[v_DimOrganisation] 


### v_TallyFraværsintervaller

| **View** |
| - | 
| [chru_cube].[v_TallyFraværsintervaller] |

### v_TallyFraværMaaltal

| **View** |
| - |
| [chru_cube].[v_TallyFraværMaaltal] |

### v_SlicerNedslagsdatoer

| **View** | **Baseret på** | 
| - | - |
| [chru_cube].[v_SlicerNedslagsdatoer] | [DM_FL_HR].[DimDato] |



## Dashboard
