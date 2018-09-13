# AWSSupport-ManageRDPSettings

## Description

The AWSSupport-ManageRDPSettings automation document allows the user to manage common Remote Desktop Protocol (RDP) settings, such as the RDP port and Network Layer Authentication (NLA). By default, the document reads and outputs the values of the settings. IMPORTANT: Changes to the RDP settings should be carefully reviewed before running this document.

## Document Type

Automation

## Owner

Amazon

## Platform(s)

Windows

## Parameters

### InstanceId

* **Type**: String
* **Allowed pattern**: ^[i|mi]-[a-z0-9]{8,17}$
* **Description**: (Required) The ID of the managed instance to manage the RDP settings of.

### RDPPortAction

* **Type**: String
* **Allowed values**: Check, Modify
* **Default**: Check
* **Description**: (Required) An action to apply to the RDP port: Check, Modify.

### RDPPort

* **Type**: String
* **Allowed pattern**: \^(102[5-9]|1[0-9][3-9][0-9]|[2-9][0-9]{3}|[1-5][0-9]{4}|6[0-5][0-5][0-3][0-5])$
* **Default**: 3389
* **Description**: (Optional) Specify the new RDP port. Used only when the action is set to Modify. The port number must be between 1025-65535. Note: After the port is changed, the RDP service is restarted.

### NLASettingAction

* **Type**: String
* **Allowed values**: Check, Enable, Disable
* **Default**: Check
* **Description**: (Required) An action to perform on the NLA setting: Check, Enable, Disable.

### Remote Connections

* **Type**: String
* **Allowed values**: Check, Enable, Disable
* **Default**: Check
* **Description**: (Required) An action to perform on the fDenyTSConnections setting: Check, Enable, Disable.

### AutomationAssumeRole

* **Type**: String
* **Description**: (Optional) The ARN of the role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses the permissions of the user that executes this document.

## Examples

### Check RDP Settings

```json
aws ssm start-automation-execution --document-name "AWSSupport-ManageRDPSettings" --parameters "InstanceId=INSTANCEID"

```

### Restore the default RDP port (3389), disable NLA, enable remote connections

```json
aws ssm start-automation-execution --document-name "ManageRDPSettings" --parameters "InstanceId=INSTANCEID,RDPPortAction=Modify, RDPPort=3389, NLASettingAction=Disable,RemoteConnections=Enable"

```

### Retrieve the execution output

```json
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

## Required IAM Permissions

The EC2 instance receiving the command must have an IAM role with the **AmazonEC2RoleforSSM** Amazon managed policy attached.
The user must have at least **ssm:SendCommand** to send the command to the instance, plus **ssm:GetCommandInvocation** to be able to read the command output.

## Document Version

Document Steps:

1. **aws:runPowerShellScript** - Executes the PowerShell script to change or check the RDP settings on the target instance.
