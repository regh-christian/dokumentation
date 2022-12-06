# CHRU_CUBE {#section_chru_cube}

## Tilføj til denne side

Når en ny tabel tilføjes til kuben


```SQL
USE [Flis2_LønHR_v2];
DECLARE @tbl AS CHAR(50) = 'v_DimAnsættelse';
SELECT
	ORDINAL_POSITION AS KolNum
	,COLUMN_NAME AS KolNavn
	,DATA_TYPE AS Datatype
	,'' AS Beskrivelse
		
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = @tbl
```

[https://www.tablesgenerator.com/markdown_tables](https://www.tablesgenerator.com/markdown_tables)



## Basis
![Icons](https://github.com/DataOgDigitalisering/dokumentation/blob/master/Images/cube_model_basis.png?raw=true)


### v_DimTidDato
| KolNum | KolNavn                    | Datatype | Beskrivelse |
|--------|----------------------------|----------|-------------|
| 1      | Dato                       | date     |             |
| 2      | Aar                        | int      |             |
| 3      | AarISOUge                  | int      |             |
| 4      | Maaned                     | int      |             |
| 5      | AarMaaned                  | varchar  |             |
| 6      | MaanedAar                  | varchar  |             |
| 7      | Kvartal                    | smallint |             |
| 8      | AarKvartal                 | varchar  |             |
| 9      | UgeNr                      | smallint |             |
| 10     | AarUge                     | varchar  |             |
| 11     | Dag                        | int      |             |
| 12     | DagAar                     | int      |             |
| 13     | DagUge                     | int      |             |
| 14     | Ugedag                     | varchar  |             |
| 15     | Dagstype                   | varchar  |             |
| 16     | Arbejdsdag                 | varchar  |             |
| 17     | ArbejdsdagBinaer           | tinyint  |             |
| 18     | MaanedNavn                 | varchar  |             |
| 19     | KortMaanedNavn             | varchar  |             |
| 20     | DagKortmaanedAar           | varchar  |             |
| 21     | TalDato                    | varchar  |             |
| 22     | OffsetDato                 | int      |             |
| 23     | FortidFremtid              | varchar  |             |
| 24     | 12MdrIntervaller           | varchar  |             |
| 25     | 12MdrIntervallerAfsluttede | varchar  |             |
| 26     | RankUge                    | int      |             |
| 27     | RankMaaned                 | int      |             |
| 28     | RankAar                    | int      |             |
| 29     | OffsetUge                  | int      |             |
| 30     | OffsetMaaned               | int      |             |
| 31     | OffsetAar                  | int      |             |
| 32     | Filterdatoer               | varchar  |             |


### v_DimAnsættelse
|     KolNum    |     KolNavn                    |     Datatype    |     Beskrivelse    |
|---------------|--------------------------------|-----------------|--------------------|
|     1         |     ID                         |     int         |                    |
|     2         |     StillingsID                |     int         |                    |
|     3         |     OrganisationsID            |     int         |                    |
|     4         |     NuværendeOrganisationID    |     int         |                    |
|     5         |     AnsættelsesformID          |     int         |                    |
|     6         |     PersonID                   |     int         |                    |
|     7         |     Tjnr                       |     varchar     |                    |
|     8         |     Jubilæumsdato              |     date        |                    |
|     9         |     Ansættelsesdato            |     date        |                    |
|     10        |     Anciennitetsdato           |     date        |                    |
|     11        |     Start                      |     date        |                    |
|     12        |     Slut                       |     date        |                    |
|     13        |     OverenskomstID             |     int         |                    |
|     14        |     Stat                       |     char        |                    |
|     15        |     Tjko                       |     char        |                    |
|     16        |     Beskdec                    |     numeric     |                    |
|     17        |     Ansat                      |     varchar     |                    |
|     18        |     Fuldtid                    |     varchar     |                    |
|     19        |     AktuelRække                |     varchar     |                    |
|     20        |     Månedslønnet               |     varchar     |                    |
|     21        |     AnsatDagsDato              |     varchar     |                    |
|     22        |     AnsatSeneste14Mdr          |     varchar     |                    |
|     23        |     AktuelHovedansættelse      |     varchar     |                    |
|     24        |     EksterntFinansieret        |     varchar     |                    |
|     25        |     Standardpopulation         |     varchar     |                    |
|     26        |     Hændelse                   |     varchar     |                    |
|     27        |     Opdateringsdato            |     datetime    |                    |
|     28        |     InstTjek                   |     varchar     |                    |
|     29        |     AfdelingTjek               |     varchar     |                    |
|     30        |     AfsnitTjek                 |     varchar     |                    |
|     31        |     HændelseMellem             |     varchar     |                    |
|     32        |     Hændelsesdato              |     date        |                    |
|     33        |     TilInst                    |     varchar     |                    |
|     34        |     AnsLængdeTjnr              |     int         |                    |
|     35        |     AnsLængdeCPR               |     int         |                    |
|     36        |     AnsLængdeTjnr_Aktuel       |     int         |                    |
|     37        |     AnsLængdeCPR_Aktuel        |     int         |                    |


### v_DimPerson
|     KolNum    |     KolNavn        |     Datatype    |     Beskrivelse    |
|---------------|--------------------|-----------------|--------------------|
|     1         |     ID             |     int         |                    |
|     2         |     Navn           |     varchar     |                    |
|     3         |     Fødselsdato    |     date        |                    |


### v_DimOrganisation
|     KolNum    |     KolNavn              |     Datatype    |     Beskrivelse    |
|---------------|--------------------------|-----------------|--------------------|
|     1         |     ID                   |     int         |                    |
|     2         |     L1Code               |     varchar     |                    |
|     3         |     L1Name               |     varchar     |                    |
|     4         |     L2Code               |     varchar     |                    |
|     5         |     L2Name               |     varchar     |                    |
|     6         |     L3Code               |     varchar     |                    |
|     7         |     L3Name               |     varchar     |                    |
|     8         |     L4Code               |     varchar     |                    |
|     9         |     L4Name               |     varchar     |                    |
|     10        |     L5Code               |     varchar     |                    |
|     11        |     L5Name               |     varchar     |                    |
|     12        |     L6Code               |     varchar     |                    |
|     13        |     L6Name               |     varchar     |                    |
|     14        |     MedarbejdereAnsat    |     int         |                    |


### v_DimStilling
|     KolNum    |     KolNavn               |     Datatype    |     Beskrivelse    |
|---------------|---------------------------|-----------------|--------------------|
|     1         |     ID                    |     int         |                    |
|     2         |     L1Code                |     varchar     |                    |
|     3         |     L1Name                |     varchar     |                    |
|     4         |     L2Code                |     varchar     |                    |
|     5         |     L2Name                |     varchar     |                    |
|     6         |     L3Code                |     varchar     |                    |
|     7         |     L3Name                |     varchar     |                    |
|     8         |     L4Code                |     varchar     |                    |
|     9         |     L4Name                |     varchar     |                    |
|     10        |     Uddannelseslæge       |     varchar     |                    |
|     11        |     Leder                 |     varchar     |                    |
|     12        |     AtypiskeStillinger    |     varchar     |                    |



