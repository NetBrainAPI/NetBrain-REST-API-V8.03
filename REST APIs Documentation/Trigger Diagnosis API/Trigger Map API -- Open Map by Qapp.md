
# Trigger Map API Design -- Qapp Map

## ***POST*** /V1/Triggers/Run
Call this API to trigger a map built by Netbrain from third part software.

## Detail Information

> **Title** : Trigger Map And Path API<br>

> **Version** : 02/08/2019.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/Triggers/Run

> **Authentication** : 

| Type | In | Name |
|---|---|---|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

|**Name**|**Type**|**Description**|
|---|---|---|
|domain_setting.tenant_id* | string  | Tenant Id  |
|domain_setting.domain_id* | string  | Domain Id  |
|basic_setting.triggered_by* | string  | Trigger user |
|basic_setting.user_id | string  | User Id，Not required |
|basic_setting.user* | string  | User Name |
|###|###| ***Note:*** If an external user account is being used, the "user" input value structure should be "autntication_id\\username".|
|basic_setting.device | string  | Device Name  |
|basic_setting.interface | string  | Interface Name，Not required  |
|basic_setting.stub_name* | string  | Stub Name  |
|basic_setting.stub_setting | object  | Stub Setting Information  |
|basic_setting.stub_setting.mode | int  | Triggered Type.<br> 0: Real-Time,<br> 1: On-Demand  |
|map_setting | object  | Map Setting Information  |
|map_setting.map_create_mode* | int  | Create Map Mode.<br>0: Map Device and Its Neighbors.<br>1: Open Site Map of the Device.<br>2: Open Existing Map.<br>3: Map a Path.<br>4: Create an Empty Map.<br>5: Context Map Of Legacy Device<br>6: Context Map Of Cisco ACI Device<br>7: Use Qapp to Create a Map<br>9: Multi devices Create a Map |
|###|###|  ***Note:*** the map_create_mode input value must corresponding to the predefined map stub trigger type in NetBrain UI system or "Some of the critical MapPath parameters are missing" response would be occured.|
| map_setting.map_qapp_para | object | map qapp parameter object. |
| map_setting.map_qapp_para.device* | string | device name. |
| map_setting.map_qapp_para.thresholdParamters | array | threshold parameters. |
| map_setting.map_qapp_para.thresholdParamters.name | string | threshold parameter name. |
| map_setting.map_qapp_para.thresholdParamters.value | string | threshold value. |
| map_setting.map_qapp_para.thresholdParamters.values | array | an array combined by threshold values. |
| map_setting.map_qapp_para.inputVariableParameters | array | qapp input variable parameters. |
| map_setting.map_qapp_para.inputVariableParameters.name | string | input variable name. |
| map_setting.map_qapp_para.inputVariableParameters.value | string | input variable value. |
| map_setting.map_qapp_para.dataSource.type | int | 1: live<br> 2: current baseline. |
| map_setting.map_qapp_para.frequency | object | run frequency setting. |
| map_setting.map_qapp_para.frequency.type | int | frequency type:<br> 1: run once.<br> 2: times. |
| map_setting.map_qapp_para.frequency.times | int | times. |
| map_setting.map_qapp_para.frequency.interval | object | cycle interval. |
| map_setting.map_qapp_para.frequency.interval.unit | int | time unit:<br> 1: hour.<br> 2:min.<br> 3: sec. |
| map_setting.map_qapp_para.frequency.interval.duration | float | duration value. |


```python
body = {
    "domain_setting": {
        "tenant_id": "", # can not be null.
        "domain_id": ""  # can not be null.
    },
    'basic_setting': {
        'user': 'admin',
        'device': 'BJ*POP',
        'stub_name': 'qapp-create-map-stub',
        'triggered_by': user
    },
    'map_setting': {
        'map_create_mode': 7,
        'map_qapp_para': {
            'thresholdParamters': [
                {
                    'name': 'Input Errors Occurred',
                    'value': '800'
                }
            ],
            'inputVariableParameters':[
                    {
                        'name':'VLAN',
                        'value':'100'
                    }
            ]         
        }
    }
}
```

## Path Parameters(****required***)

> No required parameters.

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
|mapId| string | The ID of the map which users triggered from third party sofware.  |
|mapName| string | The name of the map. |
|mapType| string | Create Map Mode.<br>0: Map Device and Its Neighbors.<br>1: Open Site Map of the Device.<br>2: Open Existing Map.<br>3: Map a Path.<br>4: Create an Empty Map.  |
|mapUrl| string | The URL link of the map triggered by users.  |
|statusCode| integer | The returned status code of executing the API.  |
|statusDescription| string | The explanation of the status code.  |

> ***Example***


```python
{
    'mapId': '5edcf486-108c-63e8-5c30-a6d32941d576',
    'mapName': 'APIExisting',
    'mapType': 1,
    'mapUrl': 'map.html?t=40e0032e-14e7-4fea-7d00-8fe8bd65efae&d=b924c2f0-7210-43ba-9cdd-d1757ae23742&id=5edcf486-108c-63e8-5c30-a6d32941d576&maptype=1',
    'stubName': 'APITest'
}
```

# Full Example:


```python
import requests
import json
import time
import requests.packages.urllib3 as urllib3
 
urllib3.disable_warnings()

token = "c814c66a-87e4-40e7-b85f-27550059b0fa"
host_url = "http://192.168.28.79"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

API_BODY = {
            'domain_setting':
            {
                'tenant_id': tenantId,
                'domain_id': domainId
            },
            'basic_setting':
            {
                'user': 'APITestGL',
                'device': 'GW2Lab',
                'stub_name': 'stub1',
                'triggered_by':"Gongdailiu",
           }, 
           'map_setting':{
               'map_create_mode':7,
                'map_qapp_para': {
                    'device':'GW2Lab',
                    'dataSource':{
                        'type':1
                    },
                    'frequency':{
                        'type':1
                    },
                    'thresholdParamters': [
                        {
                            'name': 'Alert1',
                            'value': 'test'
                        },
                    ],
                    'input_variable_parameters':[
                        {
                            'name':'interface',
                            'value':'FastEthernet1/0/2'
                        },
                    ]
                }
           }
}

def TriggerTask(API_BODY):

    # Trigger  API url
    API_URL = r"/ServicesAPI/API/V1/Triggers/Run"
    # Trigger API payload
    print(headers)
    api_full_url = nb_url + API_URL
    api_result = requests.post(api_full_url, data=json.dumps(API_BODY), headers=headers, verify=False)
    if api_result.status_code == 200:
        return api_result.json()
    else:
        return api_result.json()
    
trigger_res = TriggerTask(API_BODY)
trigger_res
```


```python
{'mapId': '4d78c778-0e96-4a6d-9bf4-1fd1533ce046',
 'mapName': 'stub1-20190716141758',
 'mapType': 1,
 'mapUrl': 'map.html?t=1ebcb6e7-b3de-0983-2048-ae12a2219b56&d=2140d612-a261-4c19-a62e-08579d5c73a3&id=4d78c778-0e96-4a6d-9bf4-1fd1533ce046&maptype=1&rba=713b2263-48a0-4948-bb94-13e2a46cebeb',
 'taskId': 'ffaef497-6f2f-434a-8270-1af08826468f'}
```

# cURL Code from Postman:


```python
curl --location --request POST 'https://integrationlab.netbraintech.com/ServicesAPI/API/V1/Triggers/Run' \
--header 'token: b63f5927-9aa7-442e-9fb6-c8abcc1b02c4' \
--header 'Content-Type: application/json' \
--data-raw '{
            "domain_setting":
            {
                "tenant_id": tenantId,
                "domain_id": domainId
            },
            "basic_setting":
            {
                "user": "APITestGL",
                "device": "GW2Lab",
                "stub_name": "stub1",
                "triggered_by":"Gongdailiu",
           }, 
           "map_setting":{
               "map_create_mode":7,
                "map_qapp_para": {
                    "device":"GW2Lab",
                    "dataSource":{
                        "type":1
                    },
                    "frequency":{
                        "type":1
                    },
                    "thresholdParamters": [
                        {
                            "name": "Alert1",
                            "value": "test"
                        },
                    ],
                    "input_variable_parameters":[
                        {
                            "name":"interface",
                            "value":"FastEthernet1/0/2"
                        },
                    ]
                }
           }
}
'
```
