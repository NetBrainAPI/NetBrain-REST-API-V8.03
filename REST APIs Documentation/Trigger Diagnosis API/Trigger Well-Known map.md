
## Trigger input parameter for verify application node

### Input parameters:

| Name | Type | Description |
|---|---|---|
|search_well_known_map_setting |object	|search Well-known map setting|
|search_well_known_map_setting.folder* |string	|Customized folder path to save the well known map. <br> Sample: Public/Known-Problems/Applications/Cisco TelePresence|
|###|###| ***Note:*** If NOT provided, exception is throw and returned in the API response as “folder path in search well known map setting not defined”|
|search_well_known_map_setting.map_count |integer	|Number of well-known maps allowed to be return. Max number can be 10 amd min can be 1.|
|search_well_known_map_setting.execute_runbook |bool	|Whether executing runbook on matched maps|
|search_well_known_map_setting.duplicate_map |bool	|Whether duplicate these maps in NetBrain.|

***Example：***


```python
task_parameter = {
    "domain_setting": {
        "tenant_id": "xxx-xxx-xxxx",
        "domain_id": "xxx-xxx-xxxx"
    },
    "basic_setting": {
        "triggered_by": "admin",
        "user": "admin",
        "device": "BJ-R1",
        "stub_name": "Default-Search Well Known Map" // this field is hard coded
    },
    "search_well_known_map_setting": {
        "folder": "Public/Well Known Map/Oracle DB",
        "map_count": 10, // allow up to ten well-known maps to be returned.
        "execute_runbook": true, // whether executing runbook on matched maps
        "duplicate_map": false
    }
}
```
