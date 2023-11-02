
# Port Access Control(PAC)

[TOC]

## Test Plan Revision History

| Rev  | Date       | Author                                | Change Description           |
| ---- | ---------- | --------------------------------------| ---------------------------- |
| 1    | 02-11-2023 | Purnima Yeggina                       | Initial Version of test plan |
# About this Manual
This Test plan describes Local Authentication feature for Port Access Control. This test plan acts as an extension to Broadcom test plan #8346

## Definition/Abbreviation

| **Term**   | **Meaning**                              |
| ---------- | ---------------------------------------- |
| VLAN       | Virtual Local Area Network               |
| PAC        | Port Access Control                      |
| MAC        | Media Access Control                     |
| MAB        | Mac Authentication Bypass                |
| NAS        | Network Access Switch                    |
| Supplicant | A client that attempts to access services offered by the Authenticator |

## Introduction

### Objective

The main objective of this test plan is to cover the test cases that are executed for local Authentication feature using 802.1x and MAB. Test cases for testing the feature using CLI commands and functionality will be discussed as part of this Test plan.

### Scope

- The scope of this test plan is to validate Port Access Control feature using Local Authentication techniques within authenticator/SONiC switch.

## Feature Overview
PAC uses 802.1x and MAB for client authentication. These methods rely on Local Authentication to verify client credentials and provide authorization.

802.1x Client Authentication: PAC uses IEEE 802.1X-2004 for client authentication. Whenever local authentication is configured in system, Supplicants can be authenticated using this technique.

MAC Authentication Bypass (MAB): For devices like cameras or printers that do not support 802.1x, PAC uses MAB. When Local Authentication is enabled, the device's MAC address is checked against Redis DB. If a match is found, the client is authorized and placed in the specified VLAN.

## 1 Test Focus Areas

### 1.1 CLI Testing

	- Verify PAC Authentication Type Configuration
 	- Test MAB User Configuration Management 
	- Validate 802.1x (HOSTAPD) User Configuration Management 
	- Check PAC_CONFIG Details in configdb 
	- Inspect MAB_USER_CONFIG Details in configdb 
	- Examine HOSTAPD_USER_CONFIG Details in configdb 
	

### 1.2 Functional Testing
          
	- Verification of local authentication of PAC
	- Confirm MAC Address Addition to MAB Table with Access Type "Allow"
 	- Ensure MAC Address Deletion from MAB Table
  	- Rejection of Invalid MAC Address Format
	- Rejection of Duplicate MAC Address
	- Validate MAC Address Update in MAB Table
	- Check User Details Addition to hostapd Table
	- Verify User Details Deletion from hostapd Table
	- Confirm User Details Update in hostapd Table
 	- Invalid Authentication Type Handling
	- Duplicate Username Handling
 ### 1.3 Reboot Testing
          
	- Verify the user details exists after reboot

## 2 Topology
<img width="540" alt="image" src="https://github.com/Purnimayeggina/sonic-mgmt/assets/131232786/f41d357b-337e-459f-a3d1-5af96009c232">

## 3 Test Case and objectives

### **3.1 CLI Test Cases**

### 3.1.1 Verify PAC Authentication Type Configuration

| **Test ID**    | **PAC_CONFIG_CLI**                          |
| -------------- | :--------------------------------------- |
| **Test Name**  | **To configure the PAC Authentication Type using config CLI command.** |                          |
| **Type**       | **CLI**                                  |
| **Steps**      | 1. Log in to the switch.<br/>2. Configure the PAC Globally to assign an authentication type for PAC by using the command “config pac-config < global> < auth-order-list> < local>”.|

### 3.1.2 Test MAB User Configuration Management 

| **Test ID**    | **MAB_CONFIG_CLI**                          |
| -------------- | :--------------------------------------- |
| **Test Name**  | **Manage MAB User config Data** | 
| **Type**       | **CLI**                                  |
| **Steps**      | 1. Log in to the switch.<br/>2 .Configure the MAB USER CONFIG based on options:<br/>2.1. Add the MAB USER CONFIG details to configdb by using the command “config mab-user-config add – <access-type, vlan-id, session-timeout>”, the configured access-type should be allow or deny, the vlan-id, session-timeout which should be within the range value of 0..4096.<br/>2.2. Delete the MAB USER CONFIG details to configdb by using the command “config mab-user-config delete”.<br/>2.3. Update the MAB USER details to configdb by using the command “config mab-user-config update – <access-type, vlan-id, session-timeout>”.|

### 3.1.3 Validate 802.1x(HOSTAPD) User Configuration Management

| **Test ID**    | **HOSTAPD_CONFIG_CLI**                          |
| -------------- | :--------------------------------------- |
| **Test Name**  | **Manage 802.1x(HOSTAPD) User config Data** | 
| **Type**       | **CLI**                                  |
| **Steps**      | 1. Log in to the switch.<br/>2 .Configure the 802.1x(HOSTAPD) USER CONFIG based on options:<br/>2.1. Add the HOSTAPD USER CONFIG details to configdb by using the command “config hostapd-user-config add - - <password, auth-type, vlan-id, session-timeout>”, here the auth-type must be configured with eap-md5 or pap or chap<br/>2.2.  Delete the HOSTAPD USER CONFIG details to configdb by using the command “config hostapd-user-config delete”.<br/>2.3. Update the HOSTAPD USER details to configdb by using the command "config hostapd-user-config update - - <password, auth-type, vlan-id, session-timeout>“. |

### 3.1.4 Check PAC_CONFIG Details in configdb 

| **Test ID**    | **PAC_SHOW_CLI**                          |
| -------------- | :--------------------------------------- |
| **Test Name**  | **To Verify configuration of pac_config for configdb** |                          
| **Type**       | **CLI**                                  |
| **Steps**      | 1. Log in to the SONiC switch.<br/>2. Check the type of authentication for PAC globally using the command “show pac-config global”. |

### 3.1.5 Inspect MAB_USER_CONFIG Details in configdb 

| **Test ID**    | **MAB_SHOW_CLI**                          |
| -------------- | :--------------------------------------- |
| **Test Name**  | **To Verify configuration of mab_user_config for configdb** |                          
| **Type**       | **CLI**                                  |
| **Steps**      | 1. Log in to the SONiC switch.<br/>2. Ensure by checking the configuration details of MAB USER CONFIG using the command “show mab-user-config”. |

### 3.1.6 Examine 802.1x(HOSTAPD) HOSTAPD_USER_CONFIG Details in configdb 

| **Test ID**    | **HOSTAPD_SHOW_CLI**                          |
| -------------- | :--------------------------------------- |
| **Test Name**  | **To Verify configuration of hostapd_user_config for configdb.** |                          
| **Type**       | **CLI**                                  |
| **Steps**      | 1. Log in to the SONiC switch.<br/>2. Ensure by checking the configuration details of HOSTAPD USER CONFIG using the command “show hostapd-user-config”. |


### **3.2 Functional Test Cases: Functionality testing of Local Authentication for MAB and 802.1x**

### 3.2.1 Verification of local authentication of PAC

| **Test ID**    | **PAc_FUNC**                         |
| -------------- | :--------------------------------------- |
| **Test Name**  | **This test case verifies the functionality of configuring PAC for supplicant authentication using the "local" method.** |
| **Type**       | **Functional**                           |
| **Steps**      | 1. Configure the PAC device to use the local authentication method using CLI command "config pac-config global auth-order-list local", by doing this local authentication will be enabled for Port Access Control(PAC).<br/>2. check the PAC table through Redis DB and verify the PAC table configured with "local" authentication type.<br/>3. Retrieve the type of authentication from the PAC Table using the CLI command "show pac-config global". | 

### 3.2.2 Confirm MAC Address Addition to MAB Table with Access Type "Allow"

| **Test ID**    | **MAB_FUNC_001**                         |
| -------------- | :--------------------------------------- |
| **Test Name**  | **To verify that when a MAC address is added to the MAB table, its access type is correctly set to “Allow”.** |
| **Type**       | **Functional**                           |
| **Steps**      | 1. Enable the Local Authentication (MAC address-based authentication) feature on an interface in the network device.<br/>2. check the MAB table and verify that the MAC address, access type, VLAN ID, and session timeout are added correctly through Redis DB.<br/>3. Retrieve the access type for the added MAC address from the MAB table using CLI command "show mab-user-config".<br/>4. check the mac address is authorized or unauthorized by verifying the access type value as Allow or Deny.<br/>5. Confirm that the retrieved access type matches the expected "Allow" which means the mac address is authorized. |
                    
### 3.2.3 Ensure MAC Address Deletion from MAB Table

| **Test ID**    | **MAB_FUNC_002**                         |
| -------------- | :--------------------------------------- |
| **Test Name**  | **To ensure that a MAC address is successfully deleted from the MAB table.** |
| **Type**       | **Functional**                           |
| **Steps**      |1. Enable the Local Authentication feature on an interface in the network device.<br/>2. Delete the MAC address and its associated details from the MAB table using CLI command "config mab-user-config delete".<br/>3. Confirm that the MAC address is no longer in the MAB table by attempting to retrieve its information  by checking the MAB table contents through Redis DB or through CLI command "show mab-user-config". |

### 3.2.4 Validate MAC Address Update in MAB Table

| **Test ID**    | **MAB_FUNC_003**                         |
| -------------- | :--------------------------------------- |
| **Test Name**  | **To verify that MAC address details are successfully updated in the MAB table.** |
| **Type**       | **Functional**                           |
| **Steps**      | 1. Enable the Local Authentication feature on an interface in the network device.<br/>2. Update MAC address details (access type, VLAN ID, or session timeout) for the specified MAC address using the CLI command mentioned in section4.1<br/>3. Check the updated MAC address details from the MAB table by checking the Redis DB.<br/>4. Confirm that the MAC address details match the expected values (access type, VLAN ID, and session timeout) using the command "show mab-user-config" for the  mac address. |

### 3.2.5 Rejection of Invalid MAC Address Format

| **Test ID**               | **MAB_FUNC_004**                         |
| --------------------------| :--------------------------------------- |
| **Test Name**             | **To Reject the addition of invalid format MAC address in the MAB table.** |
| **Type**                  | **Functional**                           |
| **Steps**                 | 1. Login into SONiC switch.<br/>2. Attempt to configure a MAB (MAC Authentication Bypass) user with an invalid format MAC address, Ex: Invalid Format MAC Address as A1:32234:B021:1033:453.<br/>3. Observe the CLI output for any error messages or warnings.<br/>4. Verify that the system fails to validate the configuration due to the invalid MAC address format and displays an error message. |
| **Expected Results:**     | 1. The system should reject the configuration with an invalid MAC address format and display an error message.<br/>2. The error message should indicate "Failed to validate configuration: Data Loading Failed Value "A1:32234:B021:1033:453" does not satisfy the constraint "[0-9a-fA-F]{2}(:[0-9a-fA-F]{2}){5}" (range, length, or pattern).<br/>Aborted!". |

### 3.2.6 Rejection of Duplicate MAC Address

| **Test ID**               | **MAB_FUNC_005**                         |
| --------------------------| :--------------------------------------- |
| **Test Name**             | **To reject the addition of duplicate MAC address in the MAB table.** |
| **Type**                  | **Functional**                           |
| **Steps**                 | 1. Login into SONiC switch.<br/>2. Attempt to add a MAC address that already exists in the configuration using the following command.<br/>3.Verify that the system detects the duplicate MAC address and returns an error message<br/>4. Confirm that the system correctly rejected the attempt to add a duplicate MAC address through error message. |
| **Expected Results:**     | The system should reject the attempt to add a MAC address that already exists in the configuration and return error message "Error: 01:80:C2:00:00:00 already exists"<br/>Aborted!". |

### 3.2.7 Check User Details Addition to 802.1x(HOSTAPD) Table

| **Test ID**    | **HOSTAPD_FUNC_001**                     |
| -------------- | :--------------------------------------- |
| **Test Name**  | **To verify that user details are successfully added to the hostapd table, when authentication type is set to “allow”.** |
| **Type**       | **Functional**                           |
| **Steps**      | 1. Enable Local Authentication on a network device interface.<br/>2. Add user details with password, authentication type, VLAN ID, and session timeout by using the CLI command mentioned in section4.1 <br/>3. Confirm the user details in the hostapd table with correct values by verifying through the CLI command "show-hostapd-user-config" or check through Redis DB.|

                   
### 3.2.8 Verify User Details Deletion from hostapd Table

| **Test ID**    | **HOSTAPD_FUNC_002**                     |
| -------------- | :--------------------------------------- |
| **Test Name**  | **To verify that user details are successfully deleted from the hostapd table.** |
| **Type**       | **Functional**                           |
| **Steps**      | 1. Enable the Local Authentication feature on an interface in the network device. <br/>2. Delete the user details by using the CLI command mentioned in section 4.1. <br/>3. Check the hostapd table to confirm that the user details have been successfully removed by verifying through the CLI command "show-hostapd-user-config" or check through Redis DB.|

### 3.2.9 Confirm User Details Update in hostapd Table

| **Test ID**    | **HOSTAPD_FUNC_003**                         |
| -------------- | :--------------------------------------- |
| **Test Name**  | **To verify that user details are successfully updated in the hostapd table.** |
| **Type**       | **Functional**                           |
| **Steps**      | 1. Enable the Local Authentication feature on an interface in the network device.<br/>2. Update the hostapd table user details with new values by using the CLI command mentioned in section 4.1<br/>3. Check hostapd table to confirm that the user details have been successfully updated with new values for the specified username  by verifying through this CLI command "show-hostapd-user-config" or check through Redis DB.|

### 3.2.10 Invalid Authentication Type Handling

| **Test ID**    | **HOSTAPD_FUNC_004**                     |
| ---------------| :--------------------------------------- |
| **Test Name**  | **To reject the addition of user details when unsupported auth-type given during configuration.** |
| **Type**       | **Functional**                           |
| **Steps**      | 1. Login into SONiC switch.<br/>2.Attempt to add a new hostapd user configuration with unsupported auth-type where the supported authentication types of hostapd are  eap-md5, chap, pap. <br/>3.Verify that the system rejects the unsupported auth-type and returns an error message, Suppose the auth-type is given like "new" which  is not a supported auth-type by hostapd then it should be rejected.<br/>4. Confirm that the system correctly rejected the attempt to add an unsupported auth-type through error message. |
| **Expected Results:**     | The system should generate an error message indicating that the provided authentication type "new" does not satisfy the constraint "eap-md5|chap|pap".<br/>2. The configuration should fail to be added, and no changes should be done. |

### 3.2.11 Duplicate Username Handling 

| **Test ID**    | **HOSTAPD_FUNC_005**                         |
| ---------------| :--------------------------------------- |
| **Test Name**  | **To reject the Username when attempting to add a username that already exists in the configuration.** |
| **Type**       | **Functional**                           |
| **Steps**      | 1. Login into SONiC switch.<br/>2. Attempt to add a username that already exists in the configuration using the CLI command.<br/>3.Verify that the system detects the duplicate username, suppose the duplicate username is "sonic_user" so for this case it should return an error message<br/>4. Confirm that the system correctly rejected the attempt to add a duplicate username through error message. |
| **Expected Results:**     | The system should reject the attempt to add a username that already exists in the configuration and return error message "Error: sonic_user already exists<br/>Aborted! |

### **3.3 Reboot and Trigger Testcase**

### 3.3.1 Verify the user details exists after reboot

| **Test ID**    | **PAC_FUNC_TRIGGER**                         |
| -------------- | :--------------------------------------- |
| **Test Name**  | **To verify that user details exist after reboot.** |
| **Type**       | **Functional**                           |
| **Steps**      | 1.Login into SONiC switch. <br/>2. Verify the user configured details for PAC, 802.1x(hostapd) and MAB exists.<br/>3. Verify the details by using the CLI Show commands "show pac-config global", "show-hostapd-user-config", "show mab-user-config".|

 
## **4 Sample Outputs**

### 4.1 Sample configuration commands
```
config pac-config <global\> <auth-order-list\> <local\>
config mab-user-config <add\> <access-type\> <vlan-id\> <session-timeout\>
config mab-user-config <delete\>
config mab-user-config <update\> <access-type\> <vlan-id\> <session-timeout\>
config hostapd-user-config <add\> <password\> \[ auth-type <pap \| eap-md5 \| chap \>\] <vlan-id\> <session-timeout\>
config hostapd-user-config <delete\>
config hostapd-user-config <update\> <password\> \[ auth-type <pap \| eap-md5 \| chap \>\] <vlan-id\> <session-timeout\>

```

### 4.2 Sample show outputs
```
admin@sonic:~$ show pac-config global

-----------------
AUTH ORDER LIST
-----------------
local

admin@sonic:~$ show hostapd-user-config 

-------------------------------------------------------------------------------
USERNAME          PASSWORD       AUTH TYPE      VLAN ID    SESSION TIMEOUT
-------------------------------------------------------------------------------
username1         password1       eap-md5             40          30    


admin@sonic:~$ show mab-user-config 

-------------------------------------------------------------------------------
MAC                  ACCESS TYPE      VLAN ID          SESSION TIMEOUT
-------------------------------------------------------------------------------
45:E7:1A:84:21:34      allow           50                60
```

### 4.3 Functionality show outputs
```
4.3.1 When authentication of PAC is added local.

root@sonic:~# config pac-config global auth-order-list local
root@sonic:~# show pac-config global
---------------------
AUTH ORDER LIST
---------------------
Local

4.3.2 MAB User details need to be  added when configured to add. 

root@sonic:~# config mab-user-config add 45:E7:1A:84:21:34 –access-type allow –session-timeout 60 –vlan-id 50
root@sonic:~# show mab-user-config

------------------------------------------------------------------------
MAC                 ACCESS TYPE        VLAN ID         SESSION TIMEOUT
------------------------------------------------------------------------
45:E7:1A:84:21:34     allow              50              60

4.3.3 MAB User details need to be deleted when configured to delete.

root@sonic:~# config mab-user-config del 45:E7:1A:84:21:34
root@sonic:~# show mab-user-config

------------------------------------------------------------
MAC      ACCESS TYPE        VLAN ID          SESSION TIMEOUT
------------------------------------------------------------

-------------------------------------------------------------

4.3.4 MAB User details need to be updated when configured to update.

root@sonic:~# config mab-user-config update 45:E7:1A:84:21:34 - access-type allow –session-timeout 70 –vlan-id 10
root@sonic:~# show mab-user-config

-----------------------------------------------------------------------------
MAC                  ACCESS TYPE          VLAN ID         SESSION TIMEOUT
-----------------------------------------------------------------------------
45:E7:1A:84:21:34       allow               10                 70

4.3.5 Reject Invalid format MAC Address when attempted to add in MAB table. 

root@sonic:~# config mab-user-config add A1:32234:B021:1033:453 --access-type allow --vlan-id 30 --session-timeout 10

libyang[0]: Value "A1:32234:B021:1033:453" does not satisfy the constraint "[0-9a-fA-F]{2}(:[0-9a-fA-F]{2}){5}" (range, length, or pattern). (path: /sonic-pac:sonic-pac/MAB_USER_CONFIG/MAB_USER_CONFIG_LIST[mac='A1:32234:B021:1033:453']/mac)
sonic_yang(3):Data Loading Failed:Value "A1:32234:B021:1033:453" does not satisfy the constraint "[0-9a-fA-F]{2}(:[0-9a-fA-F]{2}){5}" (range, length, or pattern).
Error: Failed to validate configuration: Data Loading Failed
Value "A1:32234:B021:1033:453" does not satisfy the constraint "[0-9a-fA-F]{2}(:[0-9a-fA-F]{2}){5}" (range, length, or pattern).
Aborted!

4.3.6 Reject the duplicate MAC Address when attempted to add in MAB table. 

root@sonic:~# show mab-user-config

MAC                ACCESS TYPE      VLAN ID    SESSION TIMEOUT
-----------------  -------------  ---------  -----------------
01:80:C2:00:00:00  allow                 40                 20
A1:40:32:B0:10:45  deny                  70                100

root@sonic:~# config mab-user-config add 01:80:C2:00:00:00 --access-type allow --vlan-id 40 --session-timeout 90

Error: 01:80:C2:00:00:00 already exists
Aborted!

4.3.7 Hostapd User details need to be added when configured to add.

root@sonic:~# config hostapd-user-config add username1 –auth-type eap-md5 –password password1 –session-timeout 30 –vlan-id 6
root@sonic:~# show hostapd-user-config

----------------------------------------------------------------------------
USERNAME    PASSWORD       AUTH TYPE       VLAN ID        SESSION TIMEOUT
----------------------------------------------------------------------------
username1   password1       eap-md5          6                   30

4.3.8 Hostapd User details need to be deleted when configured to delete.

root@sonic:~# config hostapd-user-config del username1
root@sonic:~# show hostapd-user-config

---------------------------------------------------------------------------------
USERNAME     PASSWORD      AUTH TYPE         VLAN ID          SESSION TIMEOUT
---------------------------------------------------------------------------------

---------------------------------------------------------------------------------

4.3.9 Hostapd User details need to be updated when configured to update.

root@sonic:~# config hostapd-user-config update username1 –auth-type eap-md5 –password password2 –session-timeout 40 –vlan-id 20
root@sonic:~# show hostapd-user-config

----------------------------------------------------------------------------------
USERNAME     PASSWORD      AUTH TYPE        VLAN ID            SESSION TIMEOUT
----------------------------------------------------------------------------------
username1    password2      eap-md5           20                  40

4.3.10 Reject the addition of unsupported auth-type in Hostapd Table

root@sonic:~# config hostapd-user-config add sonic_user3 --password admin3 --auth-type new --session-timeout 40 --vlan-id 50

libyang[0]: Value "new" does not satisfy the constraint "eap-md5|chap|pap" (range, length, or pattern). (path: /sonic-hostapd:sonic hostapd/HOSTAPD_USER_CONFIG/HOSTAPD_USER_CONFIG_LIST[username='sonic_user3']/auth_type)
sonic_yang(3):Data Loading Failed:Value "new" does not satisfy the constraint "eap-md5|chap|pap" (range, length, or pattern).
Error: Failed to validate configuration: Data Loading Failed
Value "new" does not satisfy the constraint "eap-md5|chap|pap" (range, length, or pattern).
Aborted!

4.3.11 Reject the addition of duplicate username in hostapd table.

root@sonic:~# show hostapd-user-config

USERNAME     PASSWORD    AUTH TYPE      VLAN ID    SESSION TIMEOUT
-----------  ----------  -----------  ---------  -----------------
sonic_user   admin       pap                 60                 30
sonic_user1  admin1      chap                50                 60
sonic_user2  admin2      eap-md5             50                 40

root@sonic:~# config hostapd-user-config add sonic_user --password admin3 --auth-type chap --session-timeout 40 --vlan-id 50

Error: sonic_user already exists
Aborted!
```

## **Reference Links**

- https://github.com/sonic-net/SONiC/pull/1315
- https://github.com/sonic-net/sonic-mgmt/pull/8346/files
- https://github.com/sonic-net/SONiC/pull/1469/files

