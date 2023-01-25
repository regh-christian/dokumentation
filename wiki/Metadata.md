# Metadata

## Tabeller

```SQL
<details>
   <summary><b>Metadata om tabeller i DAX Studio</b></summary>
   <center>

       USE [Flis2_LønHR_v2];

       SELECT
        col.TABLE_SCHEMA AS 'Skema'
        ,col.TABLE_NAME AS 'Tabel'
        ,col.ORDINAL_POSITION as ' '
        ,COALESCE(LEFT(keys.CONSTRAINT_NAME,1), NULL) AS '_Key'
        ,col.COLUMN_NAME AS 'Kolonne'
        ,DATA_TYPE AS 'Type'
        --,CHARACTER_MAXIMUM_LENGTH AS 'CharMaxLength'
        --,NUMERIC_PRECISION AS 'NumPrec'
        --,DATETIME_PRECISION AS 'dtPrec'
        ,COALESCE(DATETIME_PRECISION, NUMERIC_PRECISION, CHARACTER_MAXIMUM_LENGTH, NULL ) AS 'Len/Prec'
        ,CASE WHEN IS_NULLABLE = 'YES' THEN 'Y' ELSE 'N' END AS 'NULLs'
        ,COALESCE(colDesc.columnDescription, NULL) AS '_Beskrivelse'
       FROM INFORMATION_SCHEMA.COLUMNS col
       INNER JOIN information_schema.TABLES tbl 
           ON col.table_name = tbl.table_name
       LEFT JOIN INFORMATION_SCHEMA.KEY_COLUMN_USAGE keys ON 1=1
          AND keys.TABLE_SCHEMA = col.TABLE_SCHEMA
          AND keys.TABLE_NAME = col.TABLE_NAME
          AND keys.COLUMN_NAME = col.COLUMN_NAME			
       LEFT JOIN (
           SELECT 
         sc.object_id
         ,sc.column_id
         ,sc.name
         ,colProp.[value] AS 'ColumnDescription'
           FROM sys.columns sc
           INNER JOIN sys.extended_properties colProp
               ON colProp.major_id = sc.object_id
                   AND colProp.minor_id = sc.column_id
                   AND colProp.name = 'MS_Description' 
        ) colDesc
        ON colDesc.object_id = object_id(tbl.table_schema + '.' + tbl.table_name)
        AND colDesc.name = col.COLUMN_NAME
       WHERE 1=1
        and col.TABLE_SCHEMA in ('chru_cube','DM_FL_HR')--(@sch)
       ORDER BY Skema asc, Tabel ASC, ' ' ASC       
      </center>
</details>
<br>
```


<center>
<iframe width="100%" height="700" frameborder="0" scrolling="no" src="https://regionh-my.sharepoint.com/personal/nicolai_schmidt_01_regionh_dk1/_layouts/15/Doc.aspx?sourcedoc={c7c4140c-dc3a-4d83-955c-b6ae4c7ba5db}&action=embedview&wdAllowInteractivity=FALSE&Item=tbl_tabeller&&wdHideGridlines=TRUE&wdHideHeaders=TRUE&wdInConfigurator=TRUE&wdInConfigurator=TRUE&edesNext=TRUE&edrtees6=FALSE&resen=FALSE&ed1JS=FALSE&wdHideSheetTabs=TRUE&ActiveCell=A1000"></iframe>
</center>
 

## Measures
<center>
<iframe width="100%" height="700" frameborder="0" scrolling="no" src="https://regionh-my.sharepoint.com/personal/nicolai_schmidt_01_regionh_dk1/_layouts/15/Doc.aspx?sourcedoc={c7c4140c-dc3a-4d83-955c-b6ae4c7ba5db}&action=embedview&wdAllowInteractivity=FALSE&wdHideGridlines=TRUE&wdHideHeaders=TRUE&wdInConfigurator=TRUE&wdInConfigurator=TRUE&edesNext=TRUE&edrtees6=FALSE&resen=FALSE&ed1JS=FALSE&wdHideSheetTabs=TRUE&Item=tbl_measures&ActiveCell=A1000"></iframe>
</center>



<!--
&action=embedview
&wdAllowInteractivity=FALSE
&Item=measures
&wdHideGridlines=TRUE
&wdHideHeaders=TRUE
&wdInConfigurator=TRUE
&wdInConfigurator=TRUE
&edesNext=TRUE
&edrtees6=FALSE
&resen=FALSE
&ed1JS=FALSE
&wdHideSheetTabs=TRUE
&ActiveCell=A1000
-->

