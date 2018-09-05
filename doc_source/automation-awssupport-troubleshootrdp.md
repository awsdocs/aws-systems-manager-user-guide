# AWSSupport-TroubleshootRDP

## Description

The AWSSupport-TroubleshootRDP automation document allows the user to check or modify common settings on the target instance which may impact Remote Desktop Protocol (RDP) connections, such as the RDP port, Network Layer Authentication (NLA) and Windows Firewall profiles. Optionally, changes can be applied offline by stopping and starting the instance, if the user explicitly allows for offline remediation. By default, the document reads and outputs the values of the settings. IMPORTANT: Changes to the RDP settings, RDP service and Windows Firewall profiles should be carefully reviewed before running this document.

## Document Type

Automation

## Owner

Amazon

## Platform(s)

Windows

## Parameters

### InstanceId

* **Type**: String
* **Allowed pattern**: ^[m]{0,1}i-[a-z0-9]{8,17}$
* **Description**: (Required) The ID of the Windows managed instance to troubleshoot.

### Action

* **Type**: String
* **Allowed values**: CheckAll, FixAll, Custom
* **Default**: Custom
* **Description**: (Optional) [Custom] Use the values from Firewall, RDPServiceStartupType, RDPServiceAction, RDPPortAction, NLASettingAction and RemoteConnections to manage the settings. [CheckAll] Read the values of the settings without changing them. [FixAll] Restore RDP default settings, and disable the Windows Firewall.

### AllowOffline

* **Type**: String
* **Allowed values**: True, False
* **Default**: False
* **Description**: (Optional) Fix only - Set it to true if you allow an offline RDP remediation in case the online troubleshooting fails, or the provided instance is not a managed instance. Note: For the offline remediation, SSM Automation stops the instance, and creates an AMI before attempting any operations.

### Firewall

* **Type**: String
* **Allowed values**: Check, Disable
* **Default**: Check
* **Description**: (Optional) Check or disable the Windows firewall (all profiles).

### RDPServiceAction

* **Type**: String
* **Allowed values**: Check, Start, Restart, Force-Restart
* **Default**: Check
* **Description**: (Optional) Check, start, restart, or force-restart the RDP service (TermService).

### RDPServiceStartupType

* **Type**: String
* **Allowed values**: Check, Auto
* **Default**: Check
* **Description**: (Optional) Check or set the RDP service to automatically start when Windows boots.

### RDPPortAction

* **Type**: String
* **Allowed values**: Check, Modify
* **Default**: Check
* **Description**: (Optional) Check the current port used for RDP connections, or modify the RDP port back to 3389 and restart the service.

### NLASettingAction

* **Type**: String
* **Allowed values**: Check, Disable
* **Default**: Check
* **Description**: (Optional) Check or disable Network Layer Authentication (NLA).

### RemoteConnections

* **Type**: String
* **Allowed values**: Check, Allow
* **Default**: Check
* **Description**: (Optional) Check or allow remote connections.

### SubnetId

* **Type**: String
* **Allowed values**: ^$|^subnet-[a-z0-9]{8,17}|SELECTED_INSTANCE_SUBNET$
* **Default**: SELECTED_INSTANCE_SUBNET
* **Description**: (Optional) Offline only - The subnet ID for the EC2Rescue instance used to perform the offline troubleshooting. If no subnet ID is specified, AWS Systems Manager Automation will create a new VPC. IMPORTANT: The subnet must be in the same Availability Zone as InstanceId, and it must allow access to the SSM endpoints.

### S3BucketName

* **Type**: String
* **Allowed pattern**: ^$|^[_a-zA-Z0-9][-._a-zA-Z0-9]{2,62}$
* **Description**: (Optional) Offline only - S3 bucket name in your account where you want to upload the troubleshooting logs. Make sure the bucket policy does not grant unnecessary read/write permissions to parties that do not need access to the collected logs.

### AutomationAssumeRole

* **Type**: String
* **Description**: (Optional) The ARN of the role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses the permissions of the user that executes this document.

## Examples

### Check the current RDP status

```json
aws ssm start-automation-execution --document-name "AWSSupport-TroubleshootRDP" --parameters "InstanceId=INSTANCEID"
```

### Disable the Windows firewall

```json
aws ssm start-automation-execution --document-name "AWSSupport-TroubleshootRDP" --parameters "InstanceId=INSTANCEID,Action=Custom,Firewall=Disable"
```

### Restore the default RDP port

```json
aws ssm start-automation-execution --document-name "AWSSupport-TroubleshootRDP" --parameters "InstanceId=INSTANCEID, RDPPortAction=Modify"
```

### Disable NLA

```json
aws ssm start-automation-execution --document-name "AWSSupport-TroubleshootRDP" --parameters "InstanceId=INSTANCEID, NLASettingAction=Disable"
```

### Allow remote connections

```json
aws ssm start-automation-execution --document-name "AWSSupport-TroubleshootRDP" --parameters "InstanceId=INSTANCEID, RemoteConnections=Allow"
```

### Restore RDP default settings and disable all Windows Firewall profiles

```json
aws ssm start-automation-execution --document-name "AWSSupport-TroubleshootRDP" --parameters "InstanceId=INSTANCEID, Action=FixAll"
```

### Restore RDP default settings and disable all Windows Firewall profiles, with offline remediation if needed

```json
aws ssm start-automation-execution --document-name "AWSSupport-TroubleshootRDP" --parameters "InstanceId=INSTANCEID, Action=FixAll, AllowOffline=True"
```

### Retrieve the execution output

```json
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

## Required IAM Permissions

It is recommended that the EC2 instance receiving the command has an IAM role with the **AmazonEC2RoleforSSM** Amazon managed policy attached.
For the online remediation, the user must have at least **ssm:DescribeInstanceInformation**, **ssm:ExecuteAutomation** and **ssm:SendCommand** to execute the automation and send the command to the instance, plus **ssm:GetAutomationExecution** to be able to read the automation output.
For the offline remediation, the user must have at least **ssm:DescribeInstanceInformation**, **ssm:ExecuteAutomation**, **ec2:DescribeInstances**, plus **ssm:GetAutomationExecution** to be able to read the automation output. AWSSupport-TroubleshootRDP calls AWSSupport-ExecuteEC2Rescue to perform the offline remediation - please review the permissions for AWSSupport-ExecuteEC2Rescue to ensure you can run the automation successfully.

## Document Version

Document Steps:

1. **aws:assertAwsResourceProperty** - Check if the instance is a Windows instance
1. **aws:assertAwsResourceProperty** - Check if the instance is a managed instance
1. (Online troubleshooting) If the instance is a managed instance, then:
    1. **aws:assertAwsResourceProperty** - Check the provided Action value
    1. (Online check) If the *Action = Check*, then:
        1. **aws:runPowerShellScript** - Executes the PowerShell script to get the Windows Firewall profiles status.
        1. **aws:executeAutomation** - Calls AWSSupport-ManageWindowsService to get the RDP service status.
        1. **aws:executeAutomation** - Calls AWSSupport-ManageRDPSettings to get the RDP settings.
    1. (Online fix) If the *Action = Fix*, then:
        1. **aws:runPowerShellScript** - Executes the PowerShell script to disable all Windows Firewall profiles.
        1. **aws:executeAutomation** - Calls AWSSupport-ManageWindowsService to start the RDP service.
        1. **aws:executeAutomation** - Calls AWSSupport-ManageRDPSettings to enable remote connections and disable NLA.
    1. (Online management) If the *Action = Custom*, then:
        1. **aws:runPowerShellScript** - Executes the PowerShell script to manage the Windows Firewall profiles.
        1. **aws:executeAutomation** - Calls AWSSupport-ManageWindowsService to manage the RDP service.
        1. **aws:executeAutomation** - Calls AWSSupport-ManageRDPSettings to manage the RDP settings.
1. (Offline remediation) If the instance is not a managed instance then:
    1. **aws:assertAwsResourceProperty** - Assert *AllowOffline = True*
    1. **aws:assertAwsResourceProperty** - Assert *Action = FixAll*
    1. **aws:assertAwsResourceProperty** - Assert the value of SubnetId
        1. (Use the provided instance's subnet) If *SubnetId* is *SELECTED_INSTANCE_SUBNET*
            1. **aws:executeAwsApi** - Retrieve the current instance's subnet.
            1. **aws:executeAutomation** - Execute *AWSSupport-ExecuteEC2Rescue* with provided instance's subnet.
        1. (Use the provided custom subnet) If *SubnetId* is not *SELECTED_INSTANCE_SUBNET*
            1. **aws:executeAutomation** - Execute *AWSSupport-ExecuteEC2Rescue* with provided *SubnetId* value.
