
# Change Management Scheduled Task REST API Design

POST /V1/CMDB/CM/Tasks/ScheduledTask
------------------------------------

Call this API to create a Scheduled Task (ST) for an APPROVED Network Change
(NC).

**Note:**

-   Only 1 ST can be created on each NC. If there is an existing ST, create
    request is not allowed.

-   ST can only be added to APPROVED NC, so that ST is restricted to be created
    on unapproved NC.

-   APPROVED NC with executed ST doesn’t allow ST creation request.

-   Follow NetBrain system user privileges

Detail Information
------------------

>Title: Change Management Scheduled Task API

>Version: 09/30/2019

>API Server URL: http(s)://IP Address of NetBrain Web API
Server/ServicesAPI/API/V1/CMDB/CM/Tasks/ScheduledTask

>Authentication:

| **Type**              | **In**  | **Name**             |
|-----------------------|---------|----------------------|
| Bearer Authentication | Headers | Authentication token |
| Token    | String   | Authentication token, get from login API. |


Request body (\*required)
-------------------------

| **Name**               | **Type** | **Description**                                                                                 |
|------------------------|----------|-------------------------------------------------------------------------------------------------|
| Runbook_id\*           | string   | NC runbook ID                                                                                   |
| ticket_id\*            | string   | 3rd party ITSM ticket ID.                                                                       |
|                        |          | Note: use either runbook_id or ticket_id. If both are provided, runbook_id has higher priority. |
| execution_time\*       | date     | ST start time. (input time format must follow the UTC structure: 2020-01-17T20:06:00.000Z)      |
| do_not_execute_after\* | date     | ST end time. (input time format must follow the UTC structure: 2020-01-17T20:06:00.000Z)        |

Query Parameters (\*required)
-----------------------------

>No parameters required.

Headers
-------

>Data Format Headers

| **Name**     | **Type** | **Description**            |
|--------------|----------|----------------------------|
| Content-Type | String   | Support “application/json” |
| Accept       | String   | Support “application/json” |

>Authorization Headers

| **Name** | **Type** | **Description**                           |
|----------|----------|-------------------------------------------|
| Token    | String   | Authentication token, get from login API. |

Response
--------

| **Name**          | **Type** | **Description**                                |
|-------------------|----------|------------------------------------------------|
| scheduled_task_id | String   | ID of the created scheduled task               |
| statusCode        | Integer  | The returned status code of executing the API. |
| statusDescription | String   | The explanation of the status code.            |

***Note: Error Code clarification***

| **Code** | **Message** |
|------------------------------------|----------|
| 790200 | Success |
| 795012 | License is expired. |
| 793001 | System framework level error. |
| 791000 | The runbook_id or ticket_id is required. |
| 791006 | The Change Management runbook is not found. |
| 798808 | Schedule can only be applied to approved Change Management runbook. |
| 794011 | Scheduled task of this Change Management runbook is existed. |
| 794011 | OperationFailed. |
| 793001 | InternalError. |


>***Example:***


```python
{
    "scheduled_task_id" : "d5c5bb3c-1aad-ae2c-a591-347ede7d71a9",
    "statusCode" : 790200,
    "statusDescription" : "Success"
}
```

# Full Example


```python
# import python modules 
import requests
import time
import urllib3
import pprint
#urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

token = "87ccc8aa-6550-41cd-a54b-ae11b521a837" 
full_url = "http://192.168.28.139/ServicesAPI/API/V1/CM/Tasks/ScheduleTask" 
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}  
headers['token'] = token

body = {
    "Runbook_id":"453ac967-12ad-f8f1-5158-648500fa67fb",
    "ticket_id":"562d351f1b5e44502fd68774cc4bcb51",
    "execution_time":"2020-01-17T20:06:00.000Z",
    "do_not_execute_after":"2020-01-17T21:06:00.000Z"
}

try:
    # Do the HTTP request
    response = requests.post(full_url, headers=headers, data = json.dumps(body), verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        js = response.json()
        print (js)
    else:
        print ("Create CM task failed! - " + str(response.text))
except Exception as e:
    print (str(e))
    
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X POST \
  http://192.168.28.143/ServicesAPI/API/V1/CMDB/CM/Tasks/ScheduledTask \
  -H 'Accept: */*' \
  -H 'Accept-Encoding: gzip, deflate' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Length: 194' \
  -H 'Content-Type: application/json' \
  -H 'Host: 192.168.28.143' \
  -H 'Postman-Token: 1501a820-2ec7-4971-a668-9ee3d742da52,e47745d9-d9f3-408a-9c93-7d0429c7d293' \
  -H 'User-Agent: PostmanRuntime/7.15.2' \
  -H 'cache-control: no-cache' \
  -H 'token: ce4a589c-99d9-4a0e-abb6-5a91d424fad6' \
  -d '{
    "Runbook_id":"d5c5bb3c-1aad-ae2c-a591-347ede7d71a9",
    "ticket_id":"1d9e4d500fe32b4046ddc5ece1050e7e",
    "execution_time":"2020/01/15",
    "do_not_execute_after":"2020/01/16"
}
'
```
