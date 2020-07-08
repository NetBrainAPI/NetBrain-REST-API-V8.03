
# Get Product Version API Design

GET /V1/System/nodeinfo
-----------------------

Call this API to get the overall system version information.

Detail Information
------------------

> Title: Get Product Version API

> Version: 10/09/2019

> API Server URL: http(s)://IP Address of NetBrain Web API
Server/ServicesAPI/API/V1/System/ProductVersion

> Authentication:

| **Type**              | **In**  | **Name**             |
|-----------------------|---------|----------------------|
| Bearer Authentication | Headers | Authentication token |

Request body (\*required)
-------------------------

> No parameters required.

Query Parameters (\*required)
-----------------------------

> No parameters required.

Headers
-------

> Data Format Headers

| **Name**     | **Type** | **Description**            |
|--------------|----------|----------------------------|
| Content-Type | String   | Support “application/json” |
| Accept       | String   | Support “application/json” |

> Authorization Headers

| **Name** | **Type** | **Description**                           |
|----------|----------|-------------------------------------------|
| Token    | String   | Authentication token, get from login API. |

Response
--------

| **Name**                           | **Type** | **Description**                               |
|------------------------------------|----------|-----------------------------------------------|
| productVersion                   | String   | current NetBrain product version                                     |
| softwareVersion                | String   | current NetBrain spftware version                                   |
| date                | String   | data retrieved date                           |
| gitVersion          | String   | version information of git                                     |
| statusCode                         | Integer  | The returned status code of executing the API |
| statusDescription                  | String   | The explanation of the status code            |

***Example***


```python
{
    "productVersion":"8.01",
    "softwareVersion":"8.0.01",
    "date":"Mon Oct 14 15:34:19 2019 +0800",
    "gitVersion":"48fb82734f4",
    "statusCode": 790200,
    "statusDescription": "Success."
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

token = "87ccc8aa-6550-41cd-a54b-ae11b521a837" 
 
headers = {'Content-Type': 'application/json', 'Accept': 'application/json'}  
headers['token'] = token
full_url = "http://192.168.28.139/ServicesAPI/API/V1/System/ProductVersion"

try:
    # Do the HTTP request
    response = requests.get(full_url, headers=headers, verify=False)
    # Check for HTTP codes other than 200
    if response.status_code == 200:
        # Decode the JSON response into a dictionary and use the data
        js = response.json()
        print (js)
    else:
        print ("Get product version failed! - " + str(response.text))
except Exception as e:
    print (str(e))
```

    {'productVersion': '8.02', 'softwareVersion': '8.0.2', 'date': 'Thu Jan 9 17:58:47 2020 -0500', 'gitVersion': '0f17faf8614', 'statusCode': 790200, 'statusDescription': 'Success.'}
    

# cURL Code from Postman


```python
curl --location --request GET 'http://192.168.28.139/ServicesAPI/API/V1/System/ProductVersion' \
--header 'token: 87ccc8aa-6550-41cd-a54b-ae11b521a837'
```
