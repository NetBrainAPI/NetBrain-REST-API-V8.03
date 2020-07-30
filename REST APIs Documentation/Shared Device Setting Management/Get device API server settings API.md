Shared Device Setting REST API Design 
==========================

## GET /ServicesAPI/API/V1/CMDB/SharedDeviceSettings/APIServerSetting

This API is used to get device API server settings in current domain. The response of this API will return a list in JSON format.<br>

## Detail Information

>**Title:** Get device API server settings API

>**Version:** 05/27/2020

>**API Server URL:** http(s)://IP Address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/SharedDeviceSettings/APIServerSetting

>**Authentication:**

|**Type**|**In**|**Name**|
|------|------|------|
|Bearer Authentication|Headers|Authentication token|

## Request body (*required)

>No request body.

## Query Parameters (*required)

|**Name**|**Type**|**Description**|
|------|------|------|
|hostname|string OR list of string|A list of device hostnames|
|ip|string OR list of string|A list of device management IPs|
|||If provided both of hostname and ip, hostname has higher priority. If any of the devices are not found from the provided query parameter, return the found devices as a list in response and add another json key "deviceNotFound", the value is a mixed list of hostnames and IPs that are not found. If both of hostname and ip are as empty, response would depends on the "skip" and "limit" values customer insert. |
|skip|integer|The amount of records to be skipped. The value must not be negative.  If the value is negative, API throws exception {"statusCode":791001,"statusDescription":"Parameter 'skip' cannot be negative"}. No upper bound for this parameter.|
|limit|integer|The up limit amount of device records to return per API call. The value must not be negative.  If the value is negative, API throws exception {"statusCode":791001,"statusDescription":"Parameter 'limit' cannot be negative"}. No upper bound for this parameter. If the parameter is not specified in API call, it means there is not limitation setting on the call.|
|||If only provide skip value, return the device list with 50 devices information start from the skip number. If only provide limit value, return from the first device in DB. If provided both skip and limit, return as required. Error exceptions follow each parameter's description.<br>Skip and limit parameters are based on the search result from DB. The "limit" value valid range is 10 - 100, if the assigned value exceeds the range, the server will respond error message: "Parameter 'limit' must be greater than or equal to 10 and less than or equal to 100."  |


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

|**Name**|**Type**|**Description**|
|------|------|------|
|statusCode|integer|Code issued by NetBrain server indicating the execution result.|
|statusDescription|string|The explanation of the status code.|
|shareDeviceSettings|object list| A list of device setting object.|
|shareDeviceSettings.HostName| string | Device hostname. |
|shareDeviceSettings.ManageIp | string | Device management IP address. |
|shareDeviceSettings.ApplianceId | string | Name of front server. |
|shareDeviceSettings.Locked| bool | Whether the device setting has been locked. |
|shareDeviceSettings.LiveStatus| integer | live status of current device. |
|shareDeviceSettings.API_setting| object list | API servers applied to current device. |
|shareDeviceSettings.API_setting.API_plugin| string | name of applied API plugin. |
|shareDeviceSettings.API_setting.API_server| object | applied API server. |
|shareDeviceSettings.API_setting.API_server.name| string | applied API servers name. |
|shareDeviceSettings.API_setting.API_server.front_server| string | front server connect with applied API server. |


***Example***


```python
{  
    "Shared device setting" : {
        "Locked" : False,
        "LiveStatus" : 1,
        "HostName" : "CP-SW1",
        "ApplianceId" : "FS1",
        "ManageIp" : "192.168.0.58",
        "API_setting" : {[
                {
                    "API_plugin" : "string",
                    "API_server" : {
                        "name" : "string"/null,
                        "front_server" : "string"/null
                    }     
                },
                {
                    "API_plugin" : "string",
                    "API_server" : {
                        "name" : "string"/null,
                        "front_server" : "string"/null
                    }  
                },
                {
                    "API_plugin" : "string",
                    "API_server" : {
                        "name" : "string"/null,
                        "front_server" : "string"/null
                    }     
                },
                .
                .
                .
            ]
        }
    }
}
```

***Response Code***

| Code   | Message             | Description                                                                                                                                                                        |
|--------|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 790200 | OK                  |                                                                                                                                                                                    |
| 791001 | InvalidParameter    | Parameter 'skip' must be a positive numeric value.<br>Parameter 'limit' must be greater than or equal to 10 and less than or equal to 100.<br>Invalid IP address provided: {0}.|
| 793001 | InternalServerError | System framework level error                                                                                                                                                       |
