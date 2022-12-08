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

### Test
<iframe frameborder="0" seamless="seamless" class="viewer pbi-frame" title="Power BI Report Viewer" src="https://flis.regionh.top.local:444/powerbi/?id=f5e6db50-2411-49a4-898f-c1a860696cc9&amp;formatLocale=da&amp;hostdata={&quot;Build&quot;:&quot;15.0.1110.120&quot;,&quot;ExternalUser&quot;:&quot;True&quot;,&quot;IsPublicBuild&quot;:true,&quot;Host&quot;:&quot;Microsoft.ReportingServices.Portal.Services&quot;,&quot;HashedUserId&quot;:&quot;28EBAF86DDE503F2826084457F5C1E9F4E01C6AB35587512C5461FD2D7D2D46D&quot;,&quot;InstallationId&quot;:&quot;5f8db492-adc0-4135-af78-f41a058c31a3&quot;,&quot;IsEnabled&quot;:true,&quot;Edition&quot;:&quot;PBIRS SQL EESA&quot;,&quot;AuthenticationTypes&quot;:&quot;RSWindowsNegotiate, RSWindowsKerberos, RSWindowsNTLM&quot;,&quot;NumberOfProcessors&quot;:4,&quot;NumberOfCores&quot;:4,&quot;IsVirtualMachine&quot;:true,&quot;MachineId&quot;:&quot;06D0A27262936F38E598DFB55E0D0AE1&quot;,&quot;CountInstances&quot;:1,&quot;Count14xInstances&quot;:0,&quot;Count13xInstances&quot;:0,&quot;Count12xInstances&quot;:0,&quot;Count11xInstances&quot;:0,&quot;ProductSku&quot;:&quot;SSRSPBI&quot;,&quot;PortalVersion&quot;:&quot;2&quot;}"></iframe>
