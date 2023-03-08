# Exitsurvey

Exitsurvey-dashboardet er under udvikling og udkommer i starten af marts. Surveyet tager afsæt i et spørgeskema, som automatisk udsendes til alle personer der frivilligt fratræder. 

## Resume af tabeller

### v_DimExitsurvey

| **View** | **Baseret på** | 
| - | - |
| [chru_cube].[v_DimExitSurveyRespondent] | [DM_FL_HR].[SD_EXITSURVEY_RESPONSES] |



### v_FactExitsurvey

| **View** | **Baseret på** | 
| - | - |
| [chru_cube].[v_FactExitSurvey] | [DM_FL_HR].[SD_EXITSURVEY_RESPONSES] |



### (v_DrillOrg)

| **View** | **Baseret på** | 
| - | - |
| [chru_cube].[DimOrgDrill] | [chru_cube].[v_DimOrganisation] |



## HR Strategisk dahboard
Exitsurvey-undersøgelsen vil indgå som en selvstændig fane/side i HR Strategisk Dashboard.


**Beregninger**


Kommer snart og tester

Tester lige lidt...
