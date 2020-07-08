
# Trigger Event API Design

GET /V1/CMDB/EventDriven/Events
------------------------------------

Call this API to get an triggered event running status in NetBrain by event nbEventId.


Detail Information
------------------

>Title: Get triggered event running status API

>Version: 01/20/2020

>API Server URL: http(s)://IP Address of NetBrain Web API
Server/ServicesAPI/API/V1/CMDB/EventDriven/Events/{nbEventId}
>Authentication:

| **Type**              | **In**  | **Name**             |
|-----------------------|---------|----------------------|
| Bearer Authentication | Headers | Authentication token |
| Token    | String   | Authentication token, get from login API. |


Request body (\*required)
-------------------------

>No body required. 

Path Parameters (\*required)
-----------------------------

| **Name**              | **Type**| **Description**      |
|-----------------------|---------|----------------------|
| nbEventId | String   | ID of the triggered NetBrain event.             |

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
| task  | Object   | An triggered Netbrain Event task.            |
| task.translationStatus  | Integer   | Event status after parsed by NetBrain system.           |
| task.translationDescription | String   | Explanation for "translationStatus".        |
| task.taskStatus  | Integer   | The running status of the NetBrain task which triggered by third party system event.|
| task.taskDescription  | String   | Explanation for "taskStatus".        |
| task.mapId  | String   | Map ID of the map triggered by third party system event. |
| task.mapName  | String   | Map name of the map triggered by third party system event.            |
| task.mapType  | String   | Map type of the map triggered by third party system event.            |
| task.mapUrl  | String   | Map URL of the map triggered by third party system event.            |
| task.TaskId  | String   | Task ID of the triggered Netbrain Event task.           |
| task.pathAppId  | String   | Path application ID tirggered by third party system event.          |
| task.rbaId  | String   | Runbook ID of the runbook which tiggered by third party system event.            |
| task.error  | String   | Running error of the triggered Netbrain Event task.           |
| statusCode        | Integer  | The returned status code of executing the API. |
| statusDescription | String   | The explanation of the status code.            |

>***Example:***


```python
{
  "task": {
    "translationStatus": 2,
    "translationDescription": "Successfully translated the event.",
    "taskStatus": 1,
    "taskDescription": "Finished",
    "mapId": "",
    "mapName": "",
    "mapType": "",
    "mapUrl": "",
    "TaskId": "",
    "pathAppId": "",
    "rbaId": "",
    "error": ""
  }
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

token = "087049d6-9762-43c4-b732-a519b26d6c10" 
url = "http://192.168.28.139/ServicesAPI/API/V1/CMDB/EventDriven/Events" 
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}  
headers['token'] = token
nbEventId = "4a193712-5d57-4269-9e3f-5030f3e96616_202001"
full_url = url + "/" + nbEventId

try:
    # Do the HTTP request
    response = requests.get(full_url, headers=headers, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        js = response.json()
        print (js)
    else:
        print ("Get Triggered Event failed! - " + str(response.text))
except Exception as e:
    print (str(e))
    
```

    {'task': {'translationStatus': 21, 'translationDescription': 'No matched event template.', 'taskStatus': 0, 'taskDescription': 'Failed', 'mapType': 0, 'mapUrl': '', 'taskId': '4a193712-5d57-4269-9e3f-5030f3e96616_202001', 'rbaId': ''}, 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl --location --request GET 'http://192.168.28.139/ServicesAPI/API/V1/CMDB/EventDriven/Events/0fea4709-b096-4d14-bf4b-4a9142f0aaea_202001' \
--header 'token: 087049d6-9762-43c4-b732-a519b26d6c10'
```
