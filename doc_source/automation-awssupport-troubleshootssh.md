# AWSSupport\-TroubleshootSSH<a name="automation-awssupport-troubleshootssh"></a>

 **Description** 

The AWSSupport\-TroubleshootSSH automation document installs the Amazon EC2Rescue tool for Linux, and then uses the EC2Rescue tool to check or attempt to fix common issues that prevent a remote connection to the Linux machine via SSH\. Optionally, changes can be applied offline by stopping and starting the instance, if the user explicitly allows for offline remediation\. By default, the document operates in read\-only mode\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-TroubleshootSSH)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Linux

**Parameters**
+ Action

  Type: String

  Valid values: CheckAll \| FixAll

  Default: CheckAll

  Description: \(Required\) Specify whether to check for issues without fixing them or to check and automatically fix any discovered issues\.
+ AllowOffline

  Type: String

  Valid values: True \| False

  Default: False

  Description: \(Optional\) Fix only \- Set it to true if you allow an offline SSH remediation in case the online troubleshooting fails, or the provided instance is not a managed instance\. Note: For the offline remediation, SSM Automation stops the instance, and creates an AMI before attempting any operations\.
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ InstanceId

  Type: String

  Description: \(Required\) ID of your EC2 instance for Linux\.
+ S3BucketName

  Type: String

  Description: \(Optional\) Offline only \- S3 bucket name in your account where you want to upload the troubleshooting logs\. Make sure the bucket policy does not grant unnecessary read/write permissions to parties that do not need access to the collected logs\.
+ SubnetId

  Type: String

  Default: SelectedInstanceSubnet

  Description: \(Optional\) Offline only \- The subnet ID for the EC2Rescue instance used to perform the offline troubleshooting\. If no subnet ID is specified, AWS Systems Manager Automation will create a new VPC\.
**Important**  
The subnet must be in the same Availability Zone as InstanceId, and it must allow access to the SSM endpoints\.

**Required IAM Permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.

It is recommended that the EC2 instance receiving the command has an IAM role with the **AmazonSSMManagedInstanceCore** Amazon managed policy attached\. For the online remediation, the user must have at least **ssm:DescribeInstanceInformation**, **ssm:ExecuteAutomation** and **ssm:SendCommand** to run the automation and send the command to the instance, plus **ssm:GetAutomationExecution** to be able to read the automation output\. For the offline remediation, the user must have at least **ssm:DescribeInstanceInformation**, **ssm:ExecuteAutomation**, **ec2:DescribeInstances**, plus **ssm:GetAutomationExecution** to be able to read the automation output\. AWSSupport\-TroubleshootSSH calls AWSSupport\-ExecuteEC2Rescue to perform the offline remediation \- please review the permissions for AWSSupport\-ExecuteEC2Rescue to ensure you can run the automation successfully\.

 **Document Steps** 

1. aws:assertAwsResourceProperty \- Check if the instance is a managed instance 

   1. \(Online remediation\) If the instance is a managed instance, then: 

      1. aws:configurePackage \- Install EC2Rescue for Linux via AWS\-ConfigureAWSPackage\.

      1. aws:runCommand \- Run the bash script to run EC2Rescue for Linux\.

   1. \(Offline remediation\) If the instance is not a managed instance then: 

      1. aws:assertAwsResourceProperty \- Assert **AllowOffline = True**

      1. aws:assertAwsResourceProperty \- Assert **Action = FixAll**

      1. aws:assertAwsResourceProperty \- Assert the value of SubnetId

      1. \(Use the provided instance's subnet\) If SubnetId is SelectedInstanceSubnet us aws:executeAutomation to run AWSSupport\-ExecuteEC2Rescue with provided instance's subnet\.

      1. \(Use the provided custom subnet\) If SubnetId is not SelectedInstanceSubnet use aws:executeAutomation to run AWSSupport\-ExecuteEC2Rescue with provided SubnetId value\.

 **Outputs** 

troubleshootSSH\.Output

troubleshootSSHOffline\.Output

troubleshootSSHOfflineWithSubnetId\.Output