
# Change Analysis Report API Design -- create export task API

## ***POST*** /V1/ChangeAnalysis/Export/Tasks
This API is used to create a task to export the change analysis report. 
> **Note:** : 8.03 latest patch is required.
## Detail Information

> **Title** : Create Export Task For Change Analysis Report API    <br>

> **Version** : 10/14/2020.

> **API Server URL** : http(s)://IP address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/ChangeAnalysis/Export/Tasks 

> **Authentication** : 

| Type | In | Name |
|---|---|---|
|Bearer Authentication| Headers | Authentication token | 

## Request body(****required***)

|**Name**|**Type**|**Description**|
|---|---|---|
|startDate* | string  | Define the start date of the report in ISO8601 format  |
|endDate* | string  | Define the end date of the report in ISO8601 format  |
|dataTypeList* | List of objects  | The monitored data type to be included in the report. See table below for data type code details.  |
|deviceIDList | List of objects  | The monitored devices to be included in the report. By default, the scope is all devices in the current domain unless specified. |

## Data Type

|**Data Type**|**dataTypeList.dataType**|**Data Type**|**dataTypeList.dataType**|
|---|---|---|---|
|Configuration   Files | config/config | IPSec   VPN Table [Real-time] | nctTable/IPsec   VPN Table[Real-time] |
|Route Table | sysTable/routeTable |     IPv6 Route Table                     |     nctTable/IPv6 Route Table                     |
|ARP Table | sysTable/arpTable |     L2VPN Forwarding Table[Real-time]    |     nctTable/L2VPN Forwarding Table[Real-time]    |
|MAC Table | sysTable/macTable |     LDP Table                            |     nctTable/LDP Table                            |
|NDP Table | sysTable/cdpTable  |     Ltm Pool Table                       |     nctTable/Ltm Pool Table                       |
|STP Table | sysTable/stpTable |     LWAP Summary Table                   |     nctTable/LWAP Summary Table                   |
|BGP Advertised-route table | sysTable/bgpNbrTable |     Management ARP Table                 |     nctTable/Management ARP Table                 |
|ARP Switch Table | nctTable/ARP Switch Table |     Management Route Table               |     nctTable/Management Route Table               |
|ARP Table [Mac Learning Type] | nctTable/ARP Table[Mac Learning Type]    |     MPLS Aggregated Label | nctTable/MPLS Aggregated Label |
|BFD Table |nctTable/BFD Table | MPLS LFIB  | nctTable/MPLS LFIB |
|BGP All VPNv4 Advertised Route Table    |     11                                       |     MPLS TE                              |     nctTable/MPLS TE                              |
|BGP Neighbors                           |     nctTable/BGP Neighbors                   |     MPLS VPNv4 Label                     |     nctTable/MPLS VPNv4 Label                     |
|BGP Network Table                       |     nctTable/BGP Network Table               |     Multicast Route Table                |     nctTable/Multicast Route Table                |
|BGP Received Route Table                |     nctTable/BGP Received Route Table        |     NAT Table                            |     nctTable/NAT Table                            |
|BGP VPNv4 Imported Routes               |     nctTable/BGP VPNv4 Imported Routes       |     NAT Tabel[Real-time]                 |     nctTable/NAT Table[Real-time]                 |
|Bridging Group                          |     nctTable/Bridging Group                  |     NHRP Table                           |     nctTable/ NHRP Table                          |
|Client Summary Table                    |     nctTable/Client Summary Table            |     NHTP Table                           |     nctTable/ NHTP Table                          |
|Contract Table                          |     nctTable/Contract Table                  |     OSPF Neighbors                       |     nctTable/OSPF Neighbors                       |
|COOP Endpoint Table                     |     nctTable/COOP Endpoint Table             |     OTV Table[Real-time]                 |     nctTable/OTV Table[Real-time]                 |
|EPG Contract Table                      |     nctTable/EPG Contract Table              |     Policy Content                       |     nctTable/Policy Content                       |
|External EPG Mapping Table              |     nctTable/External EPG Mapping Table      |     Policy Table                         |     nctTable/Policy Table                         |
|Fabric ISL                              |     nctTable/Fabric ISL                      |     QoS Mapping Table                    |     nctTable/QoS Mapping Table                    |
|Fabric Route Topology                   |     nctTable/Fabric Route Topology           |     Segment Routing SRGB                 |     nctTable/Segment Routing SRGB                 |
|Fabric Trunk                            |     nctTable/Fabric Trunk                    |     Service Graph Mapping Table          |     nctTable/Service Graph Mapping Table          |
|Fabric Path Route Table                 |     nctTable/FabricPath Route Table          |     SPB MAC Table                        |     nctTable/SPB MAC Table                        |
|Filter Table                            |     nctTable/Filter Table                    |     Uplink Table                         |     nctTable/Uplink Table                         |
|FabricPath Route Table                  |     nctTable/ FabricPath Route Table         |     Virtual Server Table                 |     nctTable/Virtual Server Table                 |
|Filter Table                            |     nctTable/Filter Table                    |     Virtual Wire                         |     nctTable/Virtual Wire                         |
|FlexConnect Summary Table               |     nctTable/FlexConnect Summary Table       |     VM Mapping Table                     |     nctTable/VM Mapping Table                     |
|Fortigate Route Table                   |     nctTable/Fortigate Route Table           |     VXLAN Address Table                  |     nctTable/VXLAN Address Table                  |
|Global Endpoint Table                   |     nctTable/Global Endpoint Table           |     VXLAN Peer Table                     |     nctTable/VXLAN Peer Table                     |
|GRE Tunnels                             |     nctTable/GRE Tunnels                     |     Wireless Endpoint Table              |     nctTable/Wireless Endpoint Table              |
|HA State                                |     nctTable/HA State                        |     WLAN Summary Table                   |     nctTable/WLAN Summary Table                   |
|Host Guest Table                        |     nctTable/Host Guest Table                |     Zone Table                           |     nctTable/Zone Table                           |
|IPSec VPN Table                         |     nctTable/IPsec VPN Table                 |     Zoning Rule Priority Table           |     nctTable/Zoning Rule Priority Table           |
|                                             |                                              |     Zoning Rule Table                    |     nctTable/Zoning Rule Table                    |


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
|taskID| string  | The unique ID of the created task  |
|statusCode| string  | Code issued by NetBrain server indicating the execution result |
|statusDescription| string | The explanation of the status code.  |

> ***Example***

# Example:


```python
API Body = {
	“StartDate” : “2020-06-01T04:00:00.000Z”,
	“EndDate” : “2020-06-02T04:00:00.000Z”,
	“DataTypeList”: [“config/config”],
	“DeviceIDList”: []
	}
```
    
## Status Code

|**Code**|**Message**|**Description**|
|---|---|---|
|790200| OK | Success  |
|791000| ParameterNULL | The required parameters in request body cannot be empty |
|790210| Data Not Enabled | Change analysis is not enabled or requested data type is not enabled in change analysis setting   |
|790210| No Access to device | User does not have privilege to access the requested devices  |
|793001| Internal Server Error | System Failure  |






