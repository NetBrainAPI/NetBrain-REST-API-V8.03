
# Trigger Map API Design -- Device and Neighbor Map

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
|map_setting.map_create_mode* | int  | Create Map Mode.<br>0: Map Device and Its Neighbors.<br>1: Open Site Map of the Device.<br>2: Open Existing Map.<br>3: Map a Path.<br>4: Create an Empty Map.<br>5: Context Map Of Legacy Device<br>6: Context Map Of Cisco ACI Device<br>7: Use Qapp to Create a Map<br>9: Multi devices Create a Map  |
|###|###|  ***Note:*** the map_create_mode input value must corresponding to the predefined map stub trigger type in NetBrain UI system or "Some of the critical MapPath parameters are missing" response would be occured.|
| map_setting.map_device_para | object | device parameters for creating map |
| map_setting.map_device_para.device* | string | device name |
| map_setting.map_device_para.include_neighbor | bool | include neighbor flag |
| map_setting.map_device_para.interfaces | list | interface name list |
| map_setting.map_device_para.neighbor_type | string | topo Link type id |


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
        "device": "", 
        "interface": "",
        "stub_name": "", # can not be null.
        "stub_setting": {
            "mode": 0,
            "max_waiting_hours": 1
        }
    },
    "map_setting": {
        "map_create_mode": 0,# can not be null.
        "map_device_para": {
            "device": "",# can not be null.
            "include_neighbor": False,
            "interfaces": [""],
            "neighbor_type": ""
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

token = "fa43384e-d54c-4f70-86b1-2b9db572d108"
host_url = "https://integrationlab.netbraintech.com/"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

API_Body = {
    "domain_setting": {
        "tenant_id": "40e0032e-14e7-4fea-7d00-8fe8bd65efae",
        "domain_id": "b924c2f0-7210-43ba-9cdd-d1757ae23742"
    },
    "basic_setting": {
        "triggered_by": "Netbrain",
        "user_id": "admin",
        "user": "gongdai.liu",
        "device": "172.16.9.251",
        "stub_name": "APITest",
    },
    "map_setting": {
        "map_create_mode": 0,
        "map_device_para": {
            "device": "172.16.9.251",
            "include_neighbor": True,
            "neighbor_type": "IPv4 L3 Topo"
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




    {'stubName': 'APITest',
     'error': 'Failed to find the designated device 172.16.9.251'}



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
        "triggered_by": "Netbrain",
        "user_id": "admin",
        "user": "gongdai.liu",
        "device": "US-BOS-R1",
        "stub_name": "APITest",
    },
    "map_setting": {
        "map_create_mode": 0,
        "map_device_para": {
            "device": "US-BOS-R1",
            "include_neighbor": "True",
            "neighbor_type": "IPv4 L3 Topo"
        }
    }
}'
```
