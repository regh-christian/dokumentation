# GitHub versionsstyring
I .....
## Best practice
### First Code test
```SQL
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

### Second Code test
```DAX
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

### Third Code test
```cpp
struct my_promise_type
{
  void* operator new(std::size_t size)
  {
    void* ptr = my_custom_allocate(size);
    if (!ptr) throw std::bad_alloc{};
    return ptr;
  }

  void operator delete(void* ptr, std::size_t size)
  {
    my_custom_free(ptr, size);
  }
  ...
};
```


