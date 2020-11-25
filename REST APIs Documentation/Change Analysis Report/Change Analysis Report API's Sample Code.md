# Change Analysis Report API Design -- change analysis report API's sample code

This sample code includes create export task API, check export task status API and export change analysis report API. 
> **Note:** : 8.03 latest patch is required.
## Detail Information

> **Title** : Change Analysis Report API's Sample Code   <br>

> **Version** : 10/14/2020.

# Full Example:


```python
import requests
import json
import base64
import json
import sys
import requests
import json
import base64

import smtplib
import os
import time 
import datetime
from datetime import datetime, timedelta
    

from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from email.mime.application import MIMEApplication
from email import encoders
from email.utils import parseaddr, formataddr
from email.header import Header


def ExportCAReport(server_url, token, request):

    '''
    Description:
                This API call is used to get the corresponding devices by an IP address. 
                For duplicate IP addresses, this API returns a device list.
    Prerequisite:
                Call SetCurrentDomain() before call this function
    '''

    headers = {"Content-Type": "application/json", "Accept": "application/json"}
    headers["Token"]=token
    full_url= server_url + "API/V1/CMDB/ChangeAnalysis/ExportReport"
    return requests.post(full_url, data=json.dumps(request), headers=headers)

def StartExportCAReport(server_url, token, request):

    '''
    Description:
                This API call is used to get the corresponding devices by an IP address. 
                For duplicate IP addresses, this API returns a device list.
    Prerequisite:
                Call SetCurrentDomain() before call this function
    '''

    headers = {"Content-Type": "application/json", "Accept": "application/json"}
    headers["Token"]=token
    full_url= server_url + "API/V1/CMDB/ChangeAnalysis/Export/Tasks"
    return requests.post(full_url, data=json.dumps(request), headers=headers)

def CheckExportStatus(server_url, token, taskId):

    '''
    Description:
                This API call is used to get the corresponding devices by an IP address. 
                For duplicate IP addresses, this API returns a device list.
    Prerequisite:
                Call SetCurrentDomain() before call this function
    '''

    headers = {"Content-Type": "application/json", "Accept": "application/json"}
    headers["Token"]=token
    full_url= server_url + "API/V1/CMDB/ChangeAnalysis/Export/Tasks/"+taskId+"/Status"
    return requests.get(full_url, headers=headers,verify=False)

def DownloadCAReport(server_url, token, taskId):

    '''
    Description:
                This API call is used to get the corresponding devices by an IP address. 
                For duplicate IP addresses, this API returns a device list.
    Prerequisite:
                Call SetCurrentDomain() before call this function
    '''

    headers = {"Content-Type": "application/json", "Accept": "application/json"}
    headers["Token"]=token
    full_url= server_url + "API/V1/CMDB/ChangeAnalysis/Export/Tasks/"+taskId+"/Download"
    return requests.get(full_url, headers=headers)


user = "admin"         
pwd = "admin"         
server_url = "http://NetBrainLab.com/ServicesAPI/"
TENANT = "1414c2b2-0b3e-d03d-0030-f8c6c7702112"       
DOMAIN = "f427d281-d610-4c55-9e0f-ff518caed300"



ExportCAReportRequest = {
    "startDate": "2020-06-01T04:00:00.000Z",
    "endDate": "2020-09-19T14:15:50.360Z",
    "deviceIDList": [],
    "dataTypeList": [
        "config/config",
        "sysTable/routeTable"
    ]
}

def Login(server_url, user, pwd):
    '''
    Description: 
                Send a request to log in to NetBrain database by specifying the login credentials and generate an authentication
                token for subsequent API calls. Username and password will be post in the "Authenticateion" header.

    Note: 
               1) All API requests require that you have an authentication token, so that you need to add an authorization
                header to each request with your authentication token as the value. 
               2) After login, next you need to specify the domain that you will work on by the SetCurrentDomain API.
    '''
    full_url= server_url + "API/V1/Session"
    #get token
    basic_data = user + ":" + pwd
    basic_data = basic_data.encode("ascii")
    auth_data = base64.b64encode(basic_data)
    headers = {"Content-Type": "application/json", "Accept": "application/json"}
    headers["Authorization"] = "Basic " + auth_data.decode()
    response = requests.post(full_url,headers=headers, verify=False)
    if response.reason:
        print(response.reason)
    res_json = response.json()
    if response.content:
        print(response.content)
    if (res_json.get("token")):
        token = res_json["token"]
        return token
    return ""

def SetCurrentDomain(server_url, token, tenant, domain):
    '''
    Description: 
                Specify which domain you will work on to get or set NetBrain data.
    '''
    full_url = server_url + "API/V1/Session/CurrentDomain"
    headers = {"Content-Type": "application/json", "Accept": "application/json"}
    headers["Token"]=token
    return requests.put(full_url,data=json.dumps({"tenantId":tenant,"domainId":domain}),headers=headers, verify=False)

def EmailCAReport():
    print("Testing result for ExportCAReport()")
    token = Login(server_url,user,pwd)
    SetCurrentDomain(server_url,token,TENANT,DOMAIN)
    #response = ExportCAReport(server_url,token,ExportCAReportRequest)
    now = datetime.now()
    yesterday = now - timedelta(days=1)
    ExportCAReportRequest['endDate'] = now.strftime("%Y-%m-%dT%H:%M:%SZ")
    ExportCAReportRequest['startDate'] = yesterday.strftime("%Y-%m-%dT%H:%M:%SZ")
    response = StartExportCAReport(server_url,token,ExportCAReportRequest)
    response = json.loads(response.text)
    taskFinished = False
    taskID = response['taskID']
    if not taskID :
        return
    status = CheckExportStatus(server_url,token,taskID)
    status = json.loads(status.text)
    while status['fileProcessed']<status['fileTotal']:
            time.sleep(3)
            status = CheckExportStatus(server_url,token,taskID)
            status = json.loads(status.text)

    response = DownloadCAReport(server_url, token,response['taskID'])
    if response :
        try:
            fileStream = response.content
            #filename = response.headers['filename']
            
            send_server = "smtp.office365.com"
            message = MIMEMultipart()

            message['Subject'] = "Daily change analysis report"
            message['X-Priority'] = '1'
            message["From"] = "QA_Autotest@netbraintech.com"
            att = MIMEApplication(fileStream)
            att['Content-Disposition'] = response.headers['Content-Disposition']
            #att['Content-Disposition'] = 'attachment; filename="%s"' % filename
            message.attach(att)
            stmpObj = smtplib.SMTP(send_server, 587)
            stmpObj.starttls()
            stmpObj.set_debuglevel(1)

            stmpObj.login("QA_Autotest@netbraintech.com", "netbrain.1")
            stmpObj.sendmail("thomas.sun@netbraintech.com",
                    ["thomas.sun@netbraintech.com",], message.as_string())
 
            i=1
        except  Exception as e:
            print(e)

EmailCAReport()
```