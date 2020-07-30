
NetBrain Integration Deployment Guide
=====================================

Single Pane of Glass – Splunk Any Search Base on Custom Search Query
------------------------------------------------------------

Use Case
========

Description
-----------

This use case visualizes Splunk search reault from custom Splunk search query to NetBrain dynamic maps based on device. Network engineers can check all available data which can be query by Splunk search query in Splunk Enterprise system. For example, server IIS logs, device system logs and all those data sources from third party systems or Splunk forwarder which can be checked in Splunk.



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
| NetBrain Front Server | Splunk Enterprise core server | HTTPS |8089|

User Account and Privileges
---------------------------

| Application | User Account | Role                                                       |
|-------------|--------------|------------------------------------------------------------|
| NetBrain    | Required     | System Admin                                               |
| Splunk  | Required     | Any role that can query Splunk search job rest API.  |



```python

```
