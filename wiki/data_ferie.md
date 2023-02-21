# Ferieafholdelse

<!-- ERD, datamodel -->
<details><summary markdown="span">Datamodel</summary>
   <center>
      **Udskiftes med ERD fra SSMS eller TE3**
      <img src="Images/erd/erd_pbi_ferieafholdelse.png" height="95%" width="95%" class="center"/>
   </center>
</details>  
<br>


## Resume af tabeller

### v_DimFerieTyper

| **View** | **Opdateres** |
| - | - |
| [chru_cube].[v_DimFerieTyper] | Dagligt |




### v_DimFerieÅr

| **View** | **Opdateres** |
| - | - |
| [chru_cube].[v_DimFerieÅr] | Dagligt |



### v_FactFerie
| **View** | **Opdateres** | **Baseret på** |
| - | - | - |
| [chru_cube].[v_FactFerie] |  | [DM_FL_HR].[DactFerieOrdinær] |
| [chru_cube].[v_FactFerie6Ferieuge] |  | [DM_FL_HR].[FactFerie6uge] |
| [chru_cube].[v_FactFerieOverført] |  | [DM_FL_HR].[FactFerie6Overfoert] |
| [SD].[V_SD_FERIE_FERIE] +  + ??? |  | [DM_FL_HR].[FactFerie6uge] |
| [SD].[V_SD_FERIE_6UGE] |  | [DM_FL_HR].[FactFerie6uge] |
| [DM_FL_HR].[SD_OPT_HOURSASSIGNMENT] |  | [DM_FL_HR].[FactFerie6uge] |
| [DM_FL_HR].[FactTjenestetid] |  |  |



### v_FactFerieOrdinær




### v_FactFerie6Ferieuge




### v_FactFerieOverført




v_TallyRestferieIntervaller



