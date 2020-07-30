Shared Device Setting REST API Design 
==========================

## PUT /ServicesAPI/API/V1/CMDB/SharedDeviceSettings/APIServerSetting

This API is used to update device API server settings in current domain. The response of this API will return a list in JSON format.<br>

## Detail Information

>**Title:** Update device API server settings API

>**Version:** 05/27/2020

>**API Server URL:** http(s)://IP Address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/SharedDeviceSettings/APIServerSetting

>**Authentication:**

|**Type**|**In**|**Name**|
|------|------|------|
|Bearer Authentication|Headers|Authentication token|

## Request body (*required)

|**Name**|**Type**|**Description**|
|------|------|------|
|shareDeviceSettings.HostName*| string | Device hostname. |
|shareDeviceSettings.ManageIp* | string | Device management IP address. |
|shareDeviceSettings.API_setting| object list | API servers applied to current device. |
|shareDeviceSettings.API_setting.API_plugin| string | name of applied API plugin. |
|shareDeviceSettings.API_setting.API_server| object | applied API server. |
|shareDeviceSettings.API_setting.API_server.name| string | applied API servers name. |


```python
API Body = {  
        "HostName" : "CP-SW1",
        "ManageIp" : "192.168.0.58",
        "API_setting" : [
                {
                    "API_plugin" : "string",
                    "API_server" : {
                        "name" : "string"/null
                    }     
                },
                {
                    "API_plugin" : "string",
                    "API_server" : {
                        "name" : "string"/null
                    }  
                },
                {
                    "API_plugin" : "string",
                    "API_server" : {
                        "name" : "string"/null
                    }     
                },
                .
                .
                .
          ]
}
```

## Headers

>**Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|Content-Type|string|support "application/json"|  
|Accept|string|support "application/json"|

>**Authorization Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|token|string|Authentication token, get from login API.|

## Response

| Code | Message | Description |
|--------|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 790200 | OK ||
| 794011 | OperationFailed | There is no match hostname or managementip founded.<br>This device is locked, can not be updated.<br>Invalid IP. |
| 791000 | ParameterNull | API Setting is required |
| 793001 | InternalServerError | System framework level error |
