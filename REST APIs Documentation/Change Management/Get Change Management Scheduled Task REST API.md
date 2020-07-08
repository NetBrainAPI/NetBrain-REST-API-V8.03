
# Change Management Scheduled Task REST API Design

GET /V1/CMDB/CM/Tasks/ScheduledTask
-----------------------------------

Call this API to get a Scheduled Task (ST) of an APPROVED Network Change (NC).

Note:

-   ST can only be added to APPROVED NC, so that unapproved NC doesn’t have an
    ST. Return NC status and error message in response.

-   Follow NetBrain system user privileges

Detail Information
------------------

>Title: Change Management Scheduled Task API

>Version: 09/30/2019

>API Server URL: http(s)://IP Address of NetBrain Web API
Server/ServicesAPI/API/V1/CMDB/CM/Tasks/ScheduledTask/

>Authentication:

| **Type**              | **In**  | **Name**             |
|-----------------------|---------|----------------------|
| Bearer Authentication | Headers | Authentication token |

Request body (\*required)
-------------------------

>No body required.

Query Parameters (\*required)
-----------------------------

| **Name**               | **Type** | **Description**             |
|------------------------|----------|-----------------------------|
| Runbook_id\*           | string   | NC runbook ID               |
| ticket_id\*            | string   | 3rd party ITSM ticket ID.   |

***Note:*** Runbook_id and ticket_id only one should be provided by customer.


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

| **Name**              | **Type** | **Description**                                |
|-----------------------|----------|------------------------------------------------|
| runbook_id            | String   | NC runbook ID                                  |
| scheduled_task_id     | String   | ID of the created scheduled task               |
| scheduler             | String   | User name of the scheduler                     |
| scheduled_task_status | String   | Status of the created scheduled task           |
| execution_time        | date     | ST start time                                  |
| do_not_execute_after  | date     | ST end time                                    |
| status                | String   | Status of scheduled task                       |
| statusCode            | Integer  | The returned status code of executing the API. |
| statusDescription     | String   | The explanation of the status code.            |

***Note: Error Code clarification***

| **Code** | **Message**                                |
|------------------------------------|----------|
| 790200 | Success.|
| 791000 | The runbook_id or ticket_id is required. |
| 791006 | The Change Management runbook is not found. |
| 798808 | Schedule can only be applied to approved Change Management runbook.|
| 794011 | Task Schedule can not be found. |
| 793001 | System Exception Message.|
| 793001 | You're not privileged to get the Network Change. |

***Example***


```python
{
    "runbook_id" : "d5c5bb3c-1aad-ae2c-a591-347ede7d71a9",
    "scheduled_task_id" : "de7d71a9-1aad-ae2c-a591-347ed5c5bb3c",
    "scheduler" : "netbrain",
    "scheduled_task_status" : "pending",
    "execution_time" : "2020/01/15",
    "do_not_execute_after" : "2020/01/16",
    "status" : "pending"
    "statusCode" : "790200",
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
 
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}  
headers['token'] = token
runbook_id = "453ac967-12ad-f8f1-5158-648500fa67fb"
full_url = "http://192.168.28.139/ServicesAPI/API/V1/CM/Tasks/ScheduleTask/"
param = {
    "runbook_Id":runbook_id
}

try:
    # Do the HTTP request
    response = requests.get(full_url, headers=headers, params = param, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        js = response.json()
        print (js)
    else:
        print ("Get CM task failed! - " + str(response.text))
except Exception as e:
    print (str(e))
    
```

    {'runbook_id': '453ac967-12ad-f8f1-5158-648500fa67fb', 'scheduled_task_id': 'c0777853-ec19-488e-b207-43c221d1a93e', 'scheduler': 'gongdai.liu', 'execution_time': '2020-01-17T20:06:00Z', 'do_not_execute_after': '2020-01-17T21:06:00Z', 'status': 'waiting', 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl -X GET \
  http://192.168.28.139/ServicesAPI/API/V1/CMDB/CM/Tasks/ScheduledTask/453ac967-12ad-f8f1-5158-648500fa67fb \
  -H 'Accept: */*' \
  -H 'Accept-Encoding: gzip, deflate' \
  -H 'Cache-Control: no-cache' \
  -H 'Connection: keep-alive' \
  -H 'Content-Type: application/json' \
  -H 'Host: 192.168.28.139' \
  -H 'Postman-Token: ef87ecaa-beee-44f1-911f-030031433bc6,0da2ebe3-e67c-474c-8e6c-f4f49fa51678' \
  -H 'User-Agent: PostmanRuntime/7.15.2' \
  -H 'cache-control: no-cache' \
  -H 'token: 87ccc8aa-6550-41cd-a54b-ae11b521a837'
```
