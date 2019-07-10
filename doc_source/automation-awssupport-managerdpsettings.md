# AWSSupport\-ManageRDPSettings<a name="automation-awssupport-managerdpsettings"></a>

 **Description** 

The AWSSupport\-ManageRDPSettings automation document allows the user to manage common Remote Desktop Protocol \(RDP\) settings, such as the RDP port and Network Layer Authentication \(NLA\)\. By default, the document reads and outputs the values of the settings\.

**Important**  
Changes to the RDP settings should be carefully reviewed before running this document\.

 **Document Type** 

Automation

 **Owner** 

Amazon

 **Platforms** 

Windows

 **Parameters** 
+ InstanceId

  Type: String

  Description: \(Required\) The ID of the managed instance to manage the RDP settings of\.
+ RDPPortAction

  Type: String

  Allowed values: Check,Modify

  Default: Check

  Description: \(Required\) An action to apply to the RDP port: Check, Modify\.
+ RDPPort

  Type: String

  Default: 3389

  Description: \(Optional\) Specify the new RDP port\. Used only when the action is set to Modify\. The port number must be between 1025\-65535\. Note: After the port is changed, the RDP service is restarted\.
+ NLASettingAction

  Type: String

  Allowed values: Check,Enable,Disable

  Default: Check

  Description: \(Required\) An action to perform on the NLA setting: Check, Enable, Disable\.
+ RemoteConnections

  Type: String

  Allowed values: Check,Enable,Disable

  Default: Check

  Description: \(Required\) An action to perform on the fDenyTSConnections setting: Check, Enable, Disable\.
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The IAM role for this execution\. If no role is specified, AWS Systems Manager Automation will use the permissions of the user that runs this document\.

 **Examples** 

Check RDP Settings

```
aws ssm start-automation-execution --document-name "AWSSupport-ManageRDPSettings" --parameters "InstanceId=INSTANCEID"
```

Restore the default RDP port \(3389\), disable NLA, enable remote connections

```
aws ssm start-automation-execution --document-name "AWSSupport-ManageRDPSettings" --parameters "InstanceId=INSTANCEID,RDPPortAction=Modify, RDPPort=3389, NLASettingAction=Disable,RemoteConnections=Enable"
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

 **Required IAM Permissions** 

The EC2 instance receiving the command must have an IAM role with the **AmazonEC2RoleforSSM** Amazon managed policy attached\. The user must have at least **ssm:SendCommand** to send the command to the instance, plus **ssm:GetCommandInvocation** to be able to read the command output\.

 **Document Steps** 

aws:runCommand \- Run the PowerShell script to change or check the RDP settings on the target instance\.

 **Outputs** 

manageRDPSettings\.Output