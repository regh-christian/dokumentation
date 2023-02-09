# Sygefravær — KLADDE



## Datamodel



## Resume af tabeller

### v_DimLønartFravær

| **View** | **Baseret på** | 
| - | - |
| [chru_cube].[v_DimLønartFravær] | [DM_FL_HR].[DimLønart] |

Subset af v_DimLønart med samme primærnøgle, ID. Indeholder kun lønarter relateret til fravær.
Tabellen er i to niveuer. ’L1Code’ og ’L1Name’ er hard-codet kategorisering af visse lønarter i 3 forskellige fraværstyper; ’Sygefravær’, ’Barn syg’ eller ’Andet’.
’L2Code’ er 3-cifret lønartkode—svt. ’L1Code’ i v_DimLønart—og ’L2Name’ er hard-codet undergruppering af fraværstyper. 

´´´sql
,case 
   when l1code in(510, 771, 772, 775,760) then 'Øvrigt fravær'
   when l1code in(520) then 'Omsorgsdag'
   when l1code in(522,525) then '§56 fravær'
   when l1code in(524,590) then 'Arbejdsskade'
   when l1code in(790,526) then 'Sygefravær'
   when L1Code in(780,527) then 'Graviditetsgener'
   when l1code in(570,575,576,770) then 'Barsel og adoption'
   when l1code in(680,950) then 'Kursus'
   when l1code in(774) then 'Seniortimer'
   when l1code in(910,929) then 'Barn syg'
end as l2Name
´´´
>> Vær opmærksom på, at gruppering på L2-niveau ikke er identisk er med SD's *LOENARTTXT*


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
