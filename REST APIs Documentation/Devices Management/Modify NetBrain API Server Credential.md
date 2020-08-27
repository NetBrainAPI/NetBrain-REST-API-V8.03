API Server REST API
===================

PUT /V1/CMDB/ApiServers
-----------------------

Call this API to modify existing API server credentials.

Detail Information
------------------

>   **Title**: Modify NetBrain API Server Credential API

>   **Version** : 03/24/2020.

>   **API Server URL** : http(s)://IP address of NetBrain Web API
>   Server/ServicesAPI/API/V1/CMDB/ApiServers

>   **Authentication** :

| Type                  | In      | Name                 |
|-----------------------|---------|----------------------|
| Bearer Authentication | Headers | Authentication token |

Request body (\*required)
-------------------------

| **Name**     | **Type** | **Description**                                       |
|--------------|----------|-------------------------------------------------------|
| \*serverName | String   | The name of an existing API server in current domain. |
| \*username   | String   | New username to be updated.                           |
| \*password   | String   | New password to be updated.                           |

Query Parameters (\*required)
-----------------------------

>   No request parameter.

Headers
-------

>   Data Format Headers

| **Name**     | **Type** | **Description**            |
|--------------|----------|----------------------------|
| Content-Type | String   | Support “application/json” |
| Accept       | String   | Support “application/json” |

>   Authorization Headers

| **Name** | **Type** | **Description**                           |
|----------|----------|-------------------------------------------|
| Token    | String   | Authentication token, get from login API. |

Response
--------

| **Name**          | **Type** | **Description**                               |
|-------------------|----------|-----------------------------------------------|
| statusCode        | Integer  | The returned status code of executing the API |
| statusDescription | String   | The explanation of the status code            |

**Example**

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# Success Response
{
    "statusCode": 790200,
    "statusDescription": "Username and Password update Successfully."
}

# empty server name input
{
    "statusCode": 790...,
    "statusDescription": "Api server name must be specified."
}

# API server name input not exist
{
    "statusCode": 790...,
    "statusDescription": "No API server with name <XXXXXXXXXXXX> found in current domain."
}

# username or password is empty
{
    "statusCode": 790...,
    "statusDescription": " Username/password must be specified."
}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
