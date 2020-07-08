Shared Device Setting REST API Design 
==========================

## PUT /ServicesAPI/API/V1/CMDB/SharedDeviceSettings/BasicSetting

This API is used to update device basic settings in current domain. The response of this API will return a list in JSON format.<br>

**Note:** the privilege requirement is following UI setting: except Guest role, all other roles can access these three PUT APIs.

## Detail Information

>**Title:** Update device basic settings API

>**Version:** 06/18/2020

>**API Server URL:** http(s)://IP Address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/SharedDeviceSettings/BasicSetting

>**Authentication:**

|**Type**|**In**|**Name**|
|------|------|------|
|Bearer Authentication|Headers|Authentication token|

## Request body (*required)

|**Name**|**Type**|**Description**|
|------|------|------|
|shareDeviceSettings.HostName*| string | Device hostname. |
|shareDeviceSettings.ManageIp* | string | Device management IP address. |
|shareDeviceSettings.ApplianceId | string | Name of front server. |
|shareDeviceSettings.Locked| bool | Whether the device setting has been locked. |
|shareDeviceSettings.LiveStatus| integer | live status of current device. |

***Example***


```python
API Body = {  
        "Locked" : False,
        "LiveStatus" : 1,
        "HostName" : "CP-SW1",
        "ApplianceId" : "FS1",
        "ManageIp" : "192.168.0.58
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

|**Name**|**Type**|**Description**|
|------|------|------|
|statusCode|integer|Code issued by NetBrain server indicating the execution result.|
|statusDescription|string|The explanation of the status code.|


```python
# success
{
    "statusCode": 790200,
    "statusDescription": "Device <Hostname> <CLI attributes> have been updated successfully."
}

# failed
# wrong device hostname or IP
{
    "statusCode": 790XXX,
    "statusDescription": "Device <Hostname> dosen't exsit in current domain."
}
# no privilege
{
    "statusCode": 790XXX,
    "statusDescription": "Your don't have enough privilege to change <Hostname> CLI settings."
}
# attributes not support to be changed
{
    "statusCode": 790XXX,
    "statusDescription": "<CLI attributes> cannot be changed by API call."
}
```