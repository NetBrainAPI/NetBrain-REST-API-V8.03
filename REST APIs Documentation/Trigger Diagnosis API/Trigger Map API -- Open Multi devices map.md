
# Trigger Map API Design -- Multi devices map

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
| map_setting.map_devices_para | object | parameters of drawing multi devices in map. |
| map_setting.map_devices_para.devices* | array | An array of devices name. |
| map_setting.map_devices_para.auto_link | bool | Whether to use the build-in auto-link method. <br>True: auto lick <br>False: not auto link |
| map_setting.map_devices_para.auto_link_type | string | Auto link type. <br>"L2_Topo_Type" — Layer 2 topology.<br>"L3_Topo_Type" — IPv4 Layer 3 topology.|
| map_setting.include_neighbor | bool | Whether include the connected neighbor devices in triggered map. <br>True: include device’s neighbors. <br>False: don't include device’s neighbors. |
| map_setting.neighbor_type | string | Neighbor device topology type.<br> "L3_Topo_Type" — IPv4 Layer 3 topology.<br> "Ipv6_L3_Topo_Type" — IPv6 Layer 3 topology.<br>"L2_Topo_Type" — Layer 2 topology.|


```python
body = {
    "domain_setting": {
        "tenant_id": "", # can not be null.
        "domain_id": ""  # can not be null.
    },
    'basic_setting': {
        'user': 'admin', # can not be null.
        'device': 'BJ*POP',
        'stub_name': 'qapp-create-map-stub', # can not be null.
        'triggered_by': user # can not be null.
    },
    'map_setting': {
        'map_create_mode': 9,
        'map_devices_para': {
            "devices" : [    # can not be null.
                "deviceHostname1",
                "deviceHostname1",
                "deviceHostname1",
                .
                .
                .
            ],
        'auto_link': True,
        'auto_link_type': 'L2_Topo_Type',
        'include_neighbor': False,
        'neighbor_type': ""
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

token = "fc13f0e2-88ca-4a28-89f1-9698f99e3b07"
host_url = "https://integrationlab.netbraintech.com/"
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}
headers["Token"] = token

API_BODY = {
    "domain_setting": {
        "tenant_id": "40e0032e-14e7-4fea-7d00-8fe8bd65efae",
        "domain_id": "b924c2f0-7210-43ba-9cdd-d1757ae23742"
    },
    'basic_setting': {
        'user': 'admin', # can not be null.
        'stub_name': 'APITest', # can not be null.
        'triggered_by': 'gongdai.liu' # can not be null.
    },
    'map_setting': {
        'map_create_mode': 9,
        'map_devices_para': {
            "devices" : [    # can not be null.
                "US-BOS-R1",
                "US-BOS-R2",
                "US-BOS-SW5",
                "US-BOS-R3"
            ]
        }
    }
}
    

def TriggerTask(API_BODY):

    # Trigger  API url
    API_URL = r"/ServicesAPI/API/V1/Triggers/Run"
    # Trigger API payload
    print(headers)
    api_full_url = host_url + API_URL
    api_result = requests.post(api_full_url, data=json.dumps(API_BODY), headers=headers, verify=False)
    if api_result.status_code == 200:
        return api_result.json()
    else:
        return api_result.json()
    
trigger_res = TriggerTask(API_BODY)
trigger_res
```

    {'Content-Type': 'application/json', 'Accept': 'application/json', 'Token': 'fc13f0e2-88ca-4a28-89f1-9698f99e3b07'}
    




    {'error': 'Incorrect map creation mode (9) provided.'}



# cURL Code from Postman:


```python
curl --location --request GET 'https://integrationlab.netbraintech.com/ServicesAPI/API/V1/Triggers/Run' \
--header 'token: fc13f0e2-88ca-4a28-89f1-9698f99e3b07' \
--header 'Content-Type: application/json' \
--data-raw '{
        "tenant_id": "40e0032e-14e7-4fea-7d00-8fe8bd65efae",
        "domain_id": "b924c2f0-7210-43ba-9cdd-d1757ae23742"
    },
    "basic_setting": {
        "user": "admin", 
        "stub_name": "APITest", 
        "triggered_by": "gongdai.liu" 
    },
    "map_setting": {
        "map_create_mode": 9,
        "map_devices_para": {
            "devices" : [   
                "US-BOS-R1",
                "US-BOS-R2",
                "US-BOS-SW5",
                "US-BOS-R3"
            ]
        }
    }
}'
```
