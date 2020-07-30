
NetBrain Integration Deployment Guide
=====================================

Single Pane of Glass – View Splunk search result in NetBrain dynamic map
------------------------------------------------------------

Use Case
========

Description
-----------
One of the most advantage feature from Netbrain to help Network Engineer to trouble shoot their network issues is the NetBrain dynamic map. And the most powerful capability of Netbrain dynamic map is the map can be seen as a container for tons of network data. To enhance the network information on dynamic map and help NetBrain users to get the benefit from Splunk database, we provide this integration between NetBrain and Splunk to enable the feature to visualizes Splunk search reault base on search query to NetBrain dynamic maps per each device in dynamic map. NetBrain users can check all available data which can be queried in Splunk Enterprise system per different search query. For example, server IIS logs, device system logs and all those data sources from third party systems or Splunk forwarder which can be checked in Splunk.



Pre-requisites
==============

Application Version
-------------------

| Application | Version          |
|-------------|------------------|
| NetBrain    | IEv8.0 and above |
| Splunk  | V7.3.1 and above |

Network Connectivity
--------------------

| Source                | Destination         | Protocol   |Port|
|-----------------------|---------------------|------------|----|
| NetBrain Front Server | Splunk Enterprise server | HTTPS |8089|

User Account and Privileges
---------------------------

| Application | User Account | Role                                                       |
|-------------|--------------|------------------------------------------------------------|
| NetBrain    | Required     | System Admin                                               |
| Splunk  | Required     | Account have access to the desired data source.  |



```python

```
