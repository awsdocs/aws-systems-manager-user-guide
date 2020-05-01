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
+ InstanceId

  Type: String

  Description: \(Required\) The ID of the managed instance to manage the RDP settings of\.
+ WindowsServiceName

  Type: String

  Description: \(Required\) A valid Windows service name\.
+ StartupType

  Type: String

  Allowed values: Check,Auto,Demand,Disabled,DelayedAutoStart

  Default: Check

  Description: \(Required\) A startup type to apply to the Windows service: Auto, Demand \(Manual\), Disabled, DelayAutoStart, Check\. 
+ ServiceAction

  Type: String

  Allowed values: Check,Restart,Force\-Restart,Start,Stop,Force\-Stop,Pause

  Default: Check

  Description: \(Required\) An action to apply to the Windows service: Restart, Force\-Restart, Start, Stop, Force\-Stop, Pause, Check\. Note: Force\-Restart and Force\-Stop can be used to restart and to stop a service that has dependent services\. 
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The IAM role for this execution\. If no role is specified, AWS Systems Manager Automation will use the permissions of the user that runs this document\.

 **Required IAM Permissions** 

It is recommended that the EC2 instance receiving the command has an IAM role with the **AmazonSSMManagedInstanceCore** Amazon managed policy attached\. The user must have at least **ssm:ExecuteAutomation** and **ssm:SendCommand** to run the automation and send the command to the instance, plus **ssm:GetAutomationExecution** to be able to read the automation output\.

 **Document Steps** 

aws:runCommand \- Run the PowerShell script to apply the desired configuration to the Windows service on the target instance\.

 **Outputs** 

manageWindowsService\.Output