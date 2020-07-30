
NetBrain Integration Deployment Guide
=====================================

Single Pane of Glass – View Splunk search result in NetBrain dynamic map
------------------------------------------------------------

Use Case
========

Description
-----------
One of the most advantage feature from Netbrain to help Network Engineer trouble shoot their network issues is the NetBrain dynamic map. And the most powerful capability of Netbrain dynamic map is the map can be considered as a container for tons of network data. To enhance the network information on dynamic map and help NetBrain users to get the benefit from Splunk database, we provide a integrate solution between NetBrain and Splunk to enable the feature which can visualizes Splunk search result base on different search query to NetBrain dynamic maps per device. NetBrain users can view all available data which can be queried in Splunk Enterprise system base on different search query. For example, server IIS logs, device system logs and all those data sources from third party systems or Splunk forwarder.



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
