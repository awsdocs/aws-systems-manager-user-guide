# AWSSupport\-ResetAccess<a name="automation-awssupport-resetaccess"></a>

 **Description** 

This document will use the EC2Rescue tool on the specified EC2 instance to re\-enable password decryption via the EC2 Console \(Windows\), or to generate and add a new SSH key pair \(Linux\)\. If you lost your key pair, this automation will create a password\-enabled AMI that you can use to launch a new EC2 instance with a key pair you own \(Windows\)\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-ResetAccess)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ EC2RescueInstanceType

  Type: String

  Valid values: t2\.small \| t2\.medium \| t2\.large

  Default: t2\.small

  Description: \(Required\) The EC2 instance type for the EC2Rescue instance\. Recommended size: t2\.small\.
+ InstanceId

  Type: String

  Description: \(Required\) ID of the EC2 instance you want to reset access for\.
**Important**  
Systems Manager Automation stops this instance, and creates an AMI before attempting any operations\. Data stored in instance store volumes will be lost\. The public IP address will change if you are not using an Elastic IP\.
+ SubnetId

  Type: String

  Default: CreateNewVPC

  Description: \(Optional\) The subnet ID for the EC2Rescue instance\. By default, Systems Manager Automation creates a new VPC\. Alternatively, Use SelectedInstanceSubnet to use the same subnet as your instance, or specify a custom subnet ID\.
**Important**  
The subnet must be in the same Availability Zone as InstanceId, and it must allow access to the SSM endpoints\.

**Required IAM Permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.

You must have at least **ssm:ExecuteAutomation**, **ssm:GetParameter** \(to retrieve the SSH key parameter name\) and **ssm:GetAutomationExecution** to be able to read the automation output\. For more information about the required permissions, see [AWSSupport\-StartEC2RescueWorkflow](automation-awssupport-startec2rescueworkflow.md)\.

 **Document Steps** 

1. aws:assertAwsResourceProperty \- Assert if the provided instance is Windows\.

   1. \(EC2Rescue for Windows\) If the provided instance is Windows:

      1. aws:executeAutomation \- Invoke AWSSupport\-StartEC2RescueWorkflow with the EC2Rescue for Windows offline password reset script

      1. aws:executeAwsApi \- Retrieve the backup AMI ID from the nested automation

      1. aws:executeAwsApi \- Retrieve the password\-enabled AMI ID from the nested automation

      1. aws:executeAwsApi \- Retrieve the EC2Rescue summary from the nested automation

   1. \(EC2Rescue for Linux\) If the provided instance is Linux:

      1. aws:executeAutomation \- Invoke AWSSupport\-StartEC2RescueWorkflow with the EC2Rescue for Linux offline SSH key injection script

      1. aws:executeAwsApi \- Retrieve the backup AMI ID from the nested automation

      1. aws:executeAwsApi \- Retrieve the SSM parameter name for the injected SSH key

      1. aws:executeAwsApi \- Retrieve the EC2Rescue summary from the nested automation

 **Outputs** 

getEC2RescueForWindowsResult\.Output

getWindowsBackupAmi\.ImageId

getWindowsPasswordEnabledAmi\.ImageId

getEC2RescueForLinuxResult\.Output

getLinuxBackupAmi\.ImageId

getLinuxSSHKeyParameter\.Name