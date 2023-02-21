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

| **View** | **Opdateres** | **Baseret på** | 
| - | - | - |
| [chru_cube].[v_DimFerieTyper] | Dagligt | [DM_FL_HR].[DimLønart] |



### v_DimFerieÅr




### v_FactFerie




### v_FactFerieOrdinær




### v_FactFerie6Ferieuge




### v_FactFerieOverført




v_TallyRestferieIntervaller
