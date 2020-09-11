# AWSSupport\-TroubleshootRDP<a name="automation-awssupport-troubleshootrdp"></a>

 **Description** 

The AWSSupport\-TroubleshootRDP automation document allows the user to check or modify common settings on the target instance which may impact Remote Desktop Protocol \(RDP\) connections, such as the RDP port, Network Layer Authentication \(NLA\) and Windows Firewall profiles\. Optionally, changes can be applied offline by stopping and starting the instance, if the user explicitly allows for offline remediation\. By default, the document reads and outputs the values of the settings\.

**Important**  
Changes to the RDP settings, RDP service and Windows Firewall profiles should be carefully reviewed before running this document\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-TroubleshootRDP)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows

**Parameters**
+ Action

  Type: String

  Valid values: CheckAll \| FixAll \| Custom

  Default: Custom

  Description: \(Optional\) \[Custom\] Use the values from Firewall, RDPServiceStartupType, RDPServiceAction, RDPPortAction, NLASettingAction and RemoteConnections to manage the settings\. \[CheckAll\] Read the values of the settings without changing them\. \[FixAll\] Restore RDP default settings, and disable the Windows Firewall\.
+ AllowOffline

  Type: String

  Valid values: True \| False

  Default: False

  Description: \(Optional\) Fix only \- Set it to true if you allow an offline RDP remediation in case the online troubleshooting fails, or the provided instance is not a managed instance\. Note: For the offline remediation, SSM Automation stops the instance, and creates an AMI before attempting any operations\.
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ Firewall

  Type: String

  Valid values: Check \| Disable

  Default: Check

  Description: \(Optional\) Check or disable the Windows firewall \(all profiles\)\.
+ InstanceId

  Type: String

  Description: \(Required\) The ID of the instance to troubleshoot the RDP settings of\.
+ NLASettingAction

  Type: String

  Valid values: Check \| Disable

  Default: Check

  Description: \(Optional\) Check or disable Network Layer Authentication \(NLA\)\.
+ RDPPortAction

  Type: String

  Valid values: Check \| Modify

  Default: Check

  Description: \(Optional\) Check the current port used for RDP connections, or modify the RDP port back to 3389 and restart the service\.
+ RDPServiceAction

  Type: String

  Valid values: Check \| Start \| Restart \| Force\-Restart

  Default: Check

  Description: \(Optional\) Check, start, restart, or force\-restart the RDP service \(TermService\)\.
+ RDPServiceStartupType

  Type: String

  Valid values: Check \| Auto

  Default: Check

  Description: \(Optional\) Check or set the RDP service to automatically start when Windows boots\.
+ RemoteConnections

  Type: String

  Valid values: Check \| Enable

  Default: Check

  Description: \(Optional\) An action to perform on the fDenyTSConnections setting: Check, Enable\.
+ S3BucketName

  Type: String

  Description: \(Optional\) Offline only \- S3 bucket name in your account where you want to upload the troubleshooting logs\. Make sure the bucket policy does not grant unnecessary read/write permissions to parties that do not need access to the collected logs\.
+ SubnetId

  Type: String

  Default: SelectedInstanceSubnet

  Description: \(Optional\) Offline only \- The subnet ID for the EC2Rescue instance used to perform the offline troubleshooting\. If no subnet ID is specified, AWS Systems Manager Automation will create a new VPC\. IMPORTANT: The subnet must be in the same Availability Zone as InstanceId, and it must allow access to the SSM endpoints\.

**Required IAM Permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.

It is recommended that the EC2 instance receiving the command has an IAM role with the **AmazonSSMManagedInstanceCore** Amazon managed policy attached\. For the online remediation, the user must have at least **ssm:DescribeInstanceInformation**, **ssm:ExecuteAutomation** and **ssm:SendCommand** to run the automation and send the command to the instance, plus **ssm:GetAutomationExecution** to be able to read the automation output\. For the offline remediation, the user must have at least **ssm:DescribeInstanceInformation**, **ssm:ExecuteAutomation**, **ec2:DescribeInstances**, plus **ssm:GetAutomationExecution** to be able to read the automation output\. AWSSupport\-TroubleshootRDP calls AWSSupport\-ExecuteEC2Rescue to perform the offline remediation \- please review the permissions for AWSSupport\-ExecuteEC2Rescue to ensure you can run the automation successfully\.

 **Document Steps** 

1. aws:assertAwsResourceProperty \- Check if the instance is a Windows Server instance

1. aws:assertAwsResourceProperty \- Check if the instance is a managed instance

1. \(Online troubleshooting\) If the instance is a managed instance, then:

   1. aws:assertAwsResourceProperty \- Check the provided Action value

   1. \(Online check\) If the **Action = CheckAll**, then:

      aws:runPowerShellScript \- Runs the PowerShell script to get the Windows Firewall profiles status\.

      aws:executeAutomation \- Calls AWSSupport\-ManageWindowsService to get the RDP service status\.

      aws:executeAutomation \- Calls AWSSupport\-ManageRDPSettings to get the RDP settings\.

   1. \(Online fix\) If the **Action = FixAll**, then:

      aws:runPowerShellScript \- Runs the PowerShell script to disable all Windows Firewall profiles\.

      aws:executeAutomation \- Calls AWSSupport\-ManageWindowsService to start the RDP service\.

      aws:executeAutomation \- Calls AWSSupport\-ManageRDPSettings to enable remote connections and disable NLA\.

   1. \(Online management\) If the **Action = Custom**, then:

      aws:runPowerShellScript \- Runs the PowerShell script to manage the Windows Firewall profiles\.

      aws:executeAutomation \- Calls AWSSupport\-ManageWindowsService to manage the RDP service\.

      aws:executeAutomation \- Calls AWSSupport\-ManageRDPSettings to manage the RDP settings\.

1. \(Offline remediation\) If the instance is not a managed instance then:

   1. aws:assertAwsResourceProperty \- Assert **AllowOffline = True**

   1. aws:assertAwsResourceProperty \- Assert **Action = FixAll**

   1. aws:assertAwsResourceProperty \- Assert the value of SubnetId

      \(Use the provided instance's subnet\) If SubnetId is SELECTED\_INSTANCE\_SUBNET

      aws:executeAwsApi \- Retrieve the current instance's subnet\.

      aws:executeAutomation \- Run AWSSupport\-ExecuteEC2Rescue with provided instance's subnet\.

   1. \(Use the provided custom subnet\) If SubnetId is not SELECTED\_INSTANCE\_SUBNET

      aws:executeAutomation \- Run AWSSupport\-ExecuteEC2Rescue with provided SubnetId value\.

 **Outputs** 

manageFirewallProfiles\.Output

manageRDPServiceSettings\.Output

manageRDPSettings\.Output

checkFirewallProfiles\.Output

checkRDPServiceSettings\.Output

checkRDPSettings\.Output

disableFirewallProfiles\.Output

restoreDefaultRDPServiceSettings\.Output

restoreDefaultRDPSettings\.Output

troubleshootRDPOffline\.Output

troubleshootRDPOfflineWithSubnetId\.Output