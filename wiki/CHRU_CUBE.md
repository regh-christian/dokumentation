# CHRU_CUBE


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

## Basis
![Icons](https://github.com/DataOgDigitalisering/dokumentation/blob/master/Images/cube_model_basis.png)


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


### v_DimPerson
|     KolNum    |     KolNavn        |     Datatype    |     Beskrivelse    |
|---------------|--------------------|-----------------|--------------------|
|     1         |     ID             |     int         |                    |
|     2         |     Navn           |     varchar     |                    |
|     3         |     Fødselsdato    |     date        |                    |
