# AWSSupport\-ManageRDPSettings<a name="automation-awssupport-managerdpsettings"></a>

 **Description** 

The AWSSupport\-ManageRDPSettings automation document allows the user to manage common Remote Desktop Protocol \(RDP\) settings, such as the RDP port and Network Layer Authentication \(NLA\)\. By default, the document reads and outputs the values of the settings\.

**Important**  
Changes to the RDP settings should be carefully reviewed before running this document\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-ManageRDPSettings)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ InstanceId

  Type: String

  Description: \(Required\) The ID of the managed instance to manage the RDP settings of\.
+ NLASettingAction

  Type: String

  Valid values: Check \| Enable \| Disable

  Default: Check

  Description: \(Required\) An action to perform on the NLA setting: Check, Enable, Disable\.
+ RDPPort

  Type: String

  Default: 3389

  Description: \(Optional\) Specify the new RDP port\. Used only when the action is set to Modify\. The port number must be between 1025\-65535\. Note: After the port is changed, the RDP service is restarted\.
+ RDPPortAction

  Type: String

  Valid values: Check \| Modify

  Default: Check

  Description: \(Required\) An action to apply to the RDP port\.
+ RemoteConnections

  Type: String

  Valid values: Check \| Enable \| Disable

  Default: Check

  Description: \(Required\) An action to perform on the fDenyTSConnections setting\.

**Required IAM Permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.

The EC2 instance receiving the command must have an IAM role with the **AmazonSSMManagedInstanceCore** Amazon managed policy attached\. The user must have at least **ssm:SendCommand** to send the command to the instance, plus **ssm:GetCommandInvocation** to be able to read the command output\.

 **Document Steps** 

aws:runCommand \- Run the PowerShell script to change or check the RDP settings on the target instance\.

 **Outputs** 

manageRDPSettings\.Output