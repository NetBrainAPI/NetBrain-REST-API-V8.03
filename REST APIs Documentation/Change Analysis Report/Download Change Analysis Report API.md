
# Change Analysis Report API Design -- export change analysis report API

## ***GET*** V1/ChangeAnalysis/Export/Tasks/{taskId}/Download
This API is used to download the change analysis report with a given taskID. 
> **Note:** : 8.03 latest patch is required.
## Detail Information

> **Title** : Download Change Analysis Report API   <br>

> **Version** : 10/14/2020.

> **API Server URL** : http(s)://IP Address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB /ChangeAnalysis/Export/Tasks/{taskId}/Download  

> **Authentication** : 

| Type | In | Name |
|---|---|---|
|Bearer Authentication| Headers | Authentication token | 

## Path Parameters(****required***)

|**Name**|**Type**|**Description**|
|---|---|---|
|taskID* | string  | The unique ID of the export task  |


## Headers

> **Data Format Headers**

|**Name**|**Type**|**Description**|
|---|---|---|
| Content-Type | string  | support "application/json" |
| Accept | string  | support "application/json" |

> **Authorization Headers**

|**Name**|**Type**|**Description**|
|---|---|---|
| token | string  | Authentication token, get from login API. |

## Response

|**Name**|**Type**|**Description**|
|---|---|---|
|fileStream| "Content-Type", "application/octet-stream"<br>"Content-Length"<br>"Content-Disposition", "attachment;filename=" + filename<br> "X-Download-Options", "noopen"| The downloaded file stream of the change analysis report  |
|statusCode| string | Code issued by NetBrain server indicating the execution result. |
|statusDescription| string | The explanation of the status code.  |

## Status Code

|**Code**|**Message**|**Description**|
|---|---|---|
|790200| OK | Success  |
|793001| Internal Server Error | System framework level error  |






