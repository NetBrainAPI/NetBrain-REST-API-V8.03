
# Trigger Map API Design -- Path Map

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
|basic_setting.stub_name* | string  | Stub Name  |
|basic_setting.stub_setting | object  | Stub Setting Information  |
|basic_setting.stub_setting.mode | int  | Triggered Type.<br> 0: Real-Time,<br> 1: On-Demand  |
|map_setting | object  | Map Setting Information  |
|map_setting.map_create_mode* | int  | Create Map Mode.<br>0: Map Device and Its Neighbors.<br>1: Open Site Map of the Device.<br>2: Open Existing Map.<br>3: Map a Path.<br>4: Create an Empty Map.<br>5: Context Map Of Legacy Device<br>6: Context Map Of Cisco ACI Device<br>7: Use Qapp to Create a Map<br>9: Multi devices Create a Map   |
|###|###|  ***Note:*** the map_create_mode input value must corresponding to the predefined map stub trigger type in NetBrain UI system or "Some of the critical MapPath parameters are missing" response would be occured.|
| map_setting.map_path_para | object | parameters of creating map by path |
| map_setting.map_path_para.source* | string | source device name |
| map_setting.map_path_para.source_gateway | string | source gateway |
| map_setting.map_path_para.source_gateway_dev | string | source gateway device name |
| map_setting.map_path_para.source_gateway_intf | string | source gateway interface name |
| map_setting.map_path_para.source_port | string | source port |
| map_setting.map_path_para.destination* | string | destination device name |
| map_setting.map_path_para.destination_gateway | string | destination gateway | 
| map_setting.map_path_para.destination_gateway_dev | string | destination gateway device name |
| map_setting.map_path_para.destination_gateway_intf | string | destination gateway interface name |
| map_setting.map_path_para.destination_port | string | destination port |
| map_setting.map_path_para.direction | int | path direction<br> 1: one-way<br> 2: round-trip | 
| map_setting.map_path_para.protocol | int | protocol id |
| map_setting.map_path_para.protocol_name | string | protocol name |
| ### | ### | ***Note:*** How to find protocol id and name, please check the end of this page.|
| map_setting.map_path_para.path_analysis_set | string | path analysis set id |
| map_setting.map_path_para.path_analysis_set_name | string | path analysis set name | 
| map_setting.map_path_para.dataSource | object | path run data source |
| map_setting.map_path_para.dataSource.type | int | Run Type<br>1: Live<br>2: Baseline<br>3: Range<br>4: Around |
| map_setting.map_path_para.dataSource.recent | object | null |
| map_setting.map_path_para.dataSource.recent.duration | float | Run duration | 
| map_setting.map_path_para.dataSource.recent.unit | int | Time Unit<br> 1:Hour; 2:Minutes; 3:Second; 4:Day. |
| map_setting.map_path_para.dataSource.range | object | range information |
| map_setting.map_path_para.dataSource.range.start | datetime | start time of range |
| map_setting.map_path_para.dataSource.range.end | datetime | end time of range |
| map_setting.map_path_para.dataSource.around | object | around information |
| map_setting.map_path_para.dataSource.around.time | datetime | time |
| map_setting.map_path_para.dataSource.around.radius | object | radius information |
| map_setting.map_path_para.dataSource.around.radius.duration | float | Run duration |
| map_setting.map_path_para.dataSource.around.radius.unit | int | Time Unit<br> 1:Hour; 2:Minutes; 3:Second; 4:Day. |


```python
body = {
    "domain_setting": {
        "tenant_id": "", # can not be null.
        "domain_id": ""  # can not be null.
    },
    "basic_setting": {
        "triggered_by": "", # can not be null.
        "user_id": "",
        "user": "", # can not be null.
        "device": "", # can not be null.
        "interface": "",
        "stub_name": "", # can not be null.
        "stub_setting": {
            "mode": 0,
            "max_waiting_hours": 1
        }
    },
    "map_setting": {
        "map_create_mode": 3,
        "map_path_para": {
            "source": '',# can not be null.
            "source_gateway": '',
            "source_port": 80,
            "destination": '',# can not be null.
            "destination_gateway": '',
            "destination_port": 80,
            "direction": 1,
            "protocol": 4,
            "isLiveUseBaseLineConfig": True,
            "advancedOption": {
                "debugMode": False,
                "calcL3ActivePath": False,
                "useCommandsWithArguments": False,
                "calcWhenDeniedByACL": False,
                "calcWhenDeniedByPolicy": False,
                "enablePathFixup": True
                 },
            "dataSource": {# can not be null.
                "type": 1,
                "recent": {
                    "unit": 2,
                    "duration": 5
                },
                "range": {
                    "start": "",
                    "end": ""
                },
                "around": {
                    "time": ""
                }
            }
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

token = "d8fb1904-16f6-47d2-91bc-f1e767e6cb04"
host_url = "https://integrationlab.netbraintech.com/"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

API_Body = {
    "domain_setting": {
        "tenant_id": "40e0032e-14e7-4fea-7d00-8fe8bd65efae", # can not be null.
        "domain_id": "b924c2f0-7210-43ba-9cdd-d1757ae23742" # can not be null.
    },
    "basic_setting": {
        "user": "gongdai.liu", # can not be null.
        "stub_name": "APITest", # can not be null.
    },
    "map_setting": {
        "map_create_mode": 3, # can not be null.
        "map_path_para": {
            "source": '10.8.1.52', # can not be null.
            "destination": '172.16.11.254', # can not be null.
#             "direction": 1, # can not be null.
#             "protocol": 4 # can not be null.
        }
    }
}
    
    
    
# Trigger API function
def TriggerTask(API_Body):
 
    # Trigger  API url
    API_URL = r"/ServicesAPI/API/V1/Triggers/Run"
    # Trigger API payload
 
    api_full_url = host_url + API_URL
    api_result = requests.post(api_full_url, data=json.dumps(
        API_Body), headers=headers, verify=False)
    if api_result.status_code == 200:
        return api_result.json()
    else:
        return api_result.json()
    
result = TriggerTask(API_Body)
result
```




    {'mapId': '4dab7245-87d2-4392-bbcd-839c986ba0ce',
     'mapName': 'APITest-20200224162858',
     'mapType': 1,
     'mapUrl': 'map.html?t=40e0032e-14e7-4fea-7d00-8fe8bd65efae&d=b924c2f0-7210-43ba-9cdd-d1757ae23742&id=4dab7245-87d2-4392-bbcd-839c986ba0ce&maptype=1',
     'taskId': '290ac4b1-6085-4407-8e8d-900e98775fc8',
     'stubName': 'APITest'}



# cURL Code from Postman:


```python
curl --location --request POST 'https://integrationlab.netbraintech.com/ServicesAPI/API/V1/Triggers/Run' \
--header 'token: b63f5927-9aa7-442e-9fb6-c8abcc1b02c4' \
--header 'Content-Type: application/json' \
--data-raw '{
    "domain_setting": {
        "tenant_id": "40e0032e-14e7-4fea-7d00-8fe8bd65efae", 
        "domain_id": "b924c2f0-7210-43ba-9cdd-d1757ae23742" 
    },
    "basic_setting": {
        "user": "gongdai.liu", 
        "stub_name": "APITest"
    },
    "map_setting": {
        "map_create_mode": 3, 
        "map_path_para": {
            "source": "10.8.1.52", 
            "destination": "72.16.11.254", 
            "direction": 1,
            "protocol": 4 
        }
    }
}'
```

# How to find Protocol ID and Name:
<img src="protocol path.png" /><br>
<img src="Protocol info.JPG" />
