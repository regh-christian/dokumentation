# GitHub versionsstyring
I .....
## Best practice

### Code test
{% prism DAX %}

EVALUATE
ADDCOLUMNS (
    FILTER (
        CROSSJOIN (
            SUMMARIZE (
                Customer,
                Customer[Customer],
                Customer[Customer.Key0]
            ),
            VALUES ( 'Date'[Date.Key0] )
        ),
        NOT ISBLANK ( [Internet Sales Amount] )
    ),
    "Sales", [Internet Sales Amount]
)
ORDER BY
    'Date'[Date.Key0] DESC,
    Customer[Customer] ASC
    
{% endprism %}

### Second Code test
```java
EVALUATE
ADDCOLUMNS (
    FILTER (
        CROSSJOIN (
            SUMMARIZE (
                Customer,
                Customer[Customer],
                Customer[Customer.Key0]
            ),
            VALUES ( 'Date'[Date.Key0] )
        ),
        NOT ISBLANK ( [Internet Sales Amount] )
    ),
    "Sales", [Internet Sales Amount]
)
ORDER BY
    'Date'[Date.Key0] DESC,
    Customer[Customer] ASC
```
