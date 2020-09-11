# AWSSupport\-ManageWindowsService<a name="automation-awssupport-managewindowsservice"></a>

 **Description** 

The AWSSupport\-ManageWindowsService automation document enables a user to stop, start, restart, pause, or disable any Windows service on the target instance\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-ManageWindowsService)

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
+ ServiceAction

  Type: String

  Valid values: Check \| Restart \| Force\-Restart \| Start \| Stop \| Force\-Stop \| Pause

  Default: Check

  Description: \(Required\) An action to apply to the Windows service\. Note that `Force-Restart` and `Force-Stop` can be used to restart and to stop a service that has dependent services\. 
+ StartupType

  Type: String

  Valid values: Check \| Auto \| Demand \| Disabled \| DelayedAutoStart

  Default: Check

  Description: \(Required\) A startup type to apply to the Windows service\.
+ WindowsServiceName

  Type: String

  Description: \(Required\) A valid Windows service name\.

**Required IAM Permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.

It is recommended that the EC2 instance receiving the command has an IAM role with the **AmazonSSMManagedInstanceCore** Amazon managed policy attached\. The user must have at least **ssm:ExecuteAutomation** and **ssm:SendCommand** to run the automation and send the command to the instance, plus **ssm:GetAutomationExecution** to be able to read the automation output\.

 **Document Steps** 

aws:runCommand \- Run the PowerShell script to apply the desired configuration to the Windows service on the target instance\.

 **Outputs** 

manageWindowsService\.Output