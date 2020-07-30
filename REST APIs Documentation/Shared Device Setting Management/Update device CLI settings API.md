Shared Device Setting REST API Design 
==========================

## PUT /ServicesAPI/API/V1/CMDB/SharedDeviceSettings/CLISetting

This API is used to update device CLI settings in current domain. The response of this API will return a list in JSON format.<br>

**Note:** the privilege requirement is following UI setting: except Guest role, all other roles can access these three PUT APIs.

## Detail Information

>**Title:** Update device CLI settings API

>**Version:** 05/27/2020

>**API Server URL:** http(s)://IP Address of NetBrain Web API Server/ServicesAPI/API/V1/CMDB/SharedDeviceSettings/CLISetting

>**Authentication:**

|**Type**|**In**|**Name**|
|------|------|------|
|Bearer Authentication|Headers|Authentication token|

## Request body (*required)

|**Name**|**Type**|**Description**|
|------|------|------|
|shareDeviceSettings.HostName*| string | Device hostname. |
|shareDeviceSettings.ManageIp* | string | Device management IP address. |
|shareDeviceSettings.CLI_setting| object | CLI setting of current device. |
|shareDeviceSettings.CLI_setting.mode| integer | mode for cli access. <br> 0 : DirectAccess <br> 1 : ViaOtherDevice|
|shareDeviceSettings.CLI_setting.access_mode| integer | access mode. <br> 0 : Telnet <br> 1 : SSH <br> 2 : SSHPubicKey|
|shareDeviceSettings.CLI_setting.access_mode_port| integer | port number of access mode. |
|shareDeviceSettings.CLI_setting.CLI_credential_username| string | usename for CLI credential. |
|shareDeviceSettings.CLI_setting.CLI_credential_password| string | password for CLI credential. |
|shareDeviceSettings.CLI_setting.privilege_username| string | device privilege username. |
|shareDeviceSettings.CLI_setting.privilege_password| string | device privilege password. |
|shareDeviceSettings.CLI_setting.Telnet_Proxy_Id| string | Telnet_Proxy_Id. |
|shareDeviceSettings.CLI_setting.Telnet_Proxy_Id_For_Smart_CLI| string | Telnet_Proxy_Id_For_Smart_CLI. |
|shareDeviceSettings.CLI_setting.prompt_settings| object | object for CLI prompt settings. |
|shareDeviceSettings.CLI_setting.prompt_settings.non_privilege_prompt| string | non_privilege_prompt. |
|shareDeviceSettings.CLI_setting.prompt_settings.privilege_prompt| string | privilege_prompt. |
|shareDeviceSettings.CLI_setting.prompt_settings.login_prompt_username| string | login_prompt_username. |
|shareDeviceSettings.CLI_setting.prompt_settings.login_prompt_password| string | login_prompt_password. |
|shareDeviceSettings.CLI_setting.prompt_settings.privilege_login_prompt| string | privilege_login_prompt_username. |
|shareDeviceSettings.CLI_setting.prompt_settings.privilege_password_prompt| string | privilege_password_prompt. |
|shareDeviceSettings.CLI_setting.advenced_setting| object | object for CLI advanced settings. |
|shareDeviceSettings.CLI_setting.advenced_setting.ForceTimeout| integer | force time out for CLI access |
|shareDeviceSettings.CLI_setting.advenced_setting.SSH_key_setting| object | object for CLI SSH finger print settings. |
|shareDeviceSettings.CLI_setting.advenced_setting.SSH_key_setting.checkSSHFingerprint| bool | enable or not for SSH Fingerprint key. |
|shareDeviceSettings.CLI_setting.advenced_setting.SSH_key_setting.SSHFingerprintKey| string | SSH fingerprint key value. |

***Example***


```python
API Body = { 
        "HostName" : "CP-SW1",
        "ManageIp" : "192.168.0.58",
        "CLI_setting" : {
            "mode":0\1,
            "access_mode":0\1\2,
            "access_mode_port":"string"/int,
            "CLI_credential_username":"string",
            "CLI_credential_Password":"string",
            "privilege_username":"string",
            "privilege_password":"string",           
            "Telnet_Proxy_Id" : "string",
            "Telnet_Proxy_Id_For_Smart_CLI" : "string"，
            "prompt_settings":{
                "non_privilege_prompt":"string",
                "privilege_prompt":"string",
                "login_prompt_username":"string",
                "login_prompt_password":"string",
                "privilege_login_prompt":"string"
                "privilege_password_prompt":"string"
            },
            "advenced_setting":{
                "ForceTimeout" : int,
                "SSH_key_setting":{
                    "checkSSHFingerprint" : true,
                    "SSHFingerprintKey" : "xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx"
                }
            }
        }
}
```

## Headers

>**Data Format Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|Content-Type|string|support "application/json"|  
|Accept|string|support "application/json"|

>**Authorization Headers**

|**Name**|**Type**|**Description**|
|------|------|------|
|token|string|Authentication token, get from login API.|

## Response

| Code | Message | Description |
|--------|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 790200 | OK ||
| 794011 | OperationFailed | There is no match hostname or managementip founded.<br>This device is locked, can not be updated.<br>Invalid IP.<br>Please insert a correct cli access method value.<br>Please insert a correct cli setting mode value.<br>The privateKey can not null.<br>The cli setting privateKey can not null.<br>There is no match cli setting privateKey. |
| 791000 | ParameterNull | CLI Setting is required |
| 793001 | InternalServerError | System framework level error |
