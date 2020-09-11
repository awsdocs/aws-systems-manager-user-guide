# AWSSupport\-ExecuteEC2Rescue<a name="automation-awssupport-executeec2rescue"></a>

 **Description** 

This document will use the EC2Rescue tool to troubleshoot and where possible repair common connectivity issues with the specified EC2 instance for Linux or Windows Server\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-ExecuteEC2Rescue)

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
+ LogDestination

  Type: String

  Description: \(Optional\) S3 bucket name in your account where you want to upload the troubleshooting logs\. Make sure the bucket policy does not grant unnecessary read/write permissions to parties that do not need access to the collected logs\.
+ SubnetId

  Type: String

  Default: CreateNewVPC

  Description: \(Optional\) The subnet ID for the EC2Rescue instance\. By default, AWS Systems Manager Automation creates a new VPC\. Alternatively, Use SelectedInstanceSubnet to use the same subnet as your instance, or specify a custom subnet ID\. IMPORTANT: The subnet must be in the same Availability Zone as UnreachableInstanceId, and it must allow access to the SSM endpoints\.
+ UnreachableInstanceId

  Type: String

  Description: \(Required\) ID of your unreachable EC2 instance\. IMPORTANT: AWS Systems Manager Automation stops this instance, and creates an AMI before attempting any operations\. Data stored in instance store volumes will be lost\. The public IP address will change if you are not using an Elastic IP\.

**Required IAM Permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.

You must have at least **ssm:ExecuteAutomation** and **ssm:GetAutomationExecution** to be able to read the automation output\. For more information about the required permissions see **AWSSupport\-StartEC2RescueWorkflow**

 **Document Steps** 

1. aws:assertAwsResourceProperty \- Assert if the provided instance is Windows 

   1. \(EC2Rescue for Windows\) If the provided instance is a Windows Server instance: 

      1. aws:executeAutomation \- Invoke AWSSupport\-StartEC2RescueWorkflow with the EC2Rescue for Windows offline script

      1. aws:executeAwsApi \- Retrieve the backup AMI ID from the nested automation

      1. aws:executeAwsApi \- Retrieve the EC2Rescue summary from the nested automation

   1. \(EC2Rescue for Linux\) If the provided instance is a Linux instance: 

      1. aws:executeAutomation \- Invoke AWSSupport\-StartEC2RescueWorkflow with the EC2Rescue for Linux offline script

      1. aws:executeAwsApi \- Retrieve the backup AMI ID from the nested automation

      1. aws:executeAwsApi \- Retrieve the EC2Rescue summary from the nested automation

 **Outputs** 

getEC2RescueForWindowsResult\.Output

getWindowsBackupAmi\.ImageId

getEC2RescueForLinuxResult\.Output

getLinuxBackupAmi\.ImageId