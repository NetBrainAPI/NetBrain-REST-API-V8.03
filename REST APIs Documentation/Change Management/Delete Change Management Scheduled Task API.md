
# Change Management Scheduled Task REST API Design

DELETE /V1/CMDB/CM/Tasks/ScheduledTask
--------------------------------------

Call this API to delete a “Waiting” status Scheduled Task (ST) of an APPROVED
Network Change (NC).

Note:

-   ST on “Running” or “Executed” status cannot be deleted.

-   Follow NetBrain system user privileges

-   Stop a “Running” task?

Detail Information
------------------

> Title: Change Management Scheduled Task API

> Version: 09/30/2019

> API Server URL: http(s)://IP Address of NetBrain Web API
Server/ServicesAPI/API/V1/CM/Tasks/ScheduleTask

> Authentication:

| **Type**              | **In**  | **Name**             |
|-----------------------|---------|----------------------|
| Bearer Authentication | Headers | Authentication token |

Request body (\*required)
-------------------------

> No body required.

Query Parameters (\*required)
-----------------------------

| **Name**               | **Type** | **Description**             |
|------------------------|----------|-----------------------------|
| Runbook_id\*           | string   | NC runbook ID               |
| ticket_id\*            | string   | 3rd party ITSM ticket ID.   |

***Note:*** Runbook_id and ticket_id only one should be provided by customer.

Headers
-------

> Data Format Headers

| **Name**     | **Type** | **Description**            |
|--------------|----------|----------------------------|
| Content-Type | String   | Support “application/json” |
| Accept       | String   | Support “application/json” |

> Authorization Headers

| **Name** | **Type** | **Description**                           |
|----------|----------|-------------------------------------------|
| Token    | String   | Authentication token, get from login API. |

Response
--------

| **Name**          | **Type** | **Description**                                |
|-------------------|----------|------------------------------------------------|
| statusCode        | Integer  | The returned status code of executing the API. |
| statusDescription | String   | The explanation of the status code.            |


***Note: Error Code clarification***

| **Code**| **Message**                                |
|------------------------------------|----------|
| 790200 |  Call running successfully.|
| 791000 | runbook id or ticket id required. |
| 791006 | The Change Management runbook is not found.|
| 798808 | Schedule can only be applied to approved Change Management runbook.|
| 798808 | Task Schedule is running now.|
| 794011 | Failed to delete. |
| 794011 | The scheduled task is running.|
| 793001 | System Exception Message.|


> ***Example***



```python
{  
    "statusCode" : "790200",
    "statusDescription" : "Success"
}
```

# Full Example:


```python
# import python modules 
import requests
import time
import urllib3
import pprint
#urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
import json

token = "87ccc8aa-6550-41cd-a54b-ae11b521a837" 
 
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}  
headers['token'] = token
runbook_id = "453ac967-12ad-f8f1-5158-648500fa67fb"
full_url = "http://192.168.28.139/ServicesAPI/API/V1/CM/Tasks/ScheduleTask/"
param = {
    "runbook_Id":runbook_id
}

try:
    # Do the HTTP request
    response = requests.delete(full_url, headers=headers, params = param, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        js = response.json()
        print (js)
    else:
        print ("Delete CM task failed! - " + str(response.text))
except Exception as e:
    print (str(e))
```

    {'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code Postman


```python
curl -X DELETE \
  'http://192.168.28.139/ServicesAPI/API/V1/CM/Tasks/ScheduleTask?runbook_Id=453ac967-12ad-f8f1-5158-648500fa67fb' \
  -H 'Accept: */*' \
  -H 'Accept-Encoding: gzip, deflate' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Length: ' \
  -H 'Content-Type: application/json' \
  -H 'Host: 192.168.28.139' \
  -H 'Postman-Token: 97f4fb3c-aeda-4a85-b8ea-5fe90b5efea8,1c24702f-17a2-4b07-a46c-9fc999ff536a' \
  -H 'User-Agent: PostmanRuntime/7.15.2' \
  -H 'cache-control: no-cache' \
  -H 'token: 87ccc8aa-6550-41cd-a54b-ae11b521a837'
```
