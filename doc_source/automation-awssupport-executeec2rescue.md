# AWSSupport\-ExecuteEC2Rescue<a name="automation-awssupport-executeec2rescue"></a>

 **Description** 

This document will use the EC2Rescue tool to troubleshoot and where possible repair common connectivity issues with the specified EC2 instance \(Windows or Linux\)\.

 **Document Type** 

Automation

 **Owner** 

Amazon

 **Platforms** 

Windows, Linux

 **Parameters** 
+ UnreachableInstanceId

  Type: String

  Description: \(Required\) ID of your unreachable EC2 instance\. IMPORTANT: AWS Systems Manager Automation stops this instance, and creates an AMI before attempting any operations\. Data stored in instance store volumes will be lost\. The public IP address will change if you are not using an Elastic IP\.
+ SubnetId

  Type: String

  Default: CreateNewVPC

  Description: \(Optional\) The subnet ID for the EC2Rescue instance\. By default, AWS Systems Manager Automation creates a new VPC\. Alternatively, Use SelectedInstanceSubnet to use the same subnet as your instance, or specify a custom subnet ID\. IMPORTANT: The subnet must be in the same Availability Zone as UnreachableInstanceId, and it must allow access to the SSM endpoints\.
+ EC2RescueInstanceType

  Type: String

  Allowed values: t2\.small,t2\.medium,t2\.large

  Default: t2\.small

  Description: \(Required\) The EC2 instance type for the EC2Rescue instance\. Recommended size: t2\.small\.
+ LogDestination

  Type: String

  Description: \(Optional\) S3 bucket name in your account where you want to upload the troubleshooting logs\. Make sure the bucket policy does not grant unnecessary read/write permissions to parties that do not need access to the collected logs\.
+ AssumeRole

  Type: String

  Description: \(Optional\) The IAM role for this execution\. If no role is specified, AWS Systems Manager Automation will use your IAM permissions to run this document\.

 **Examples** 

EC2Rescue an instance

```
aws ssm start-automation-execution --document-name AWSSupport-ExecuteEC2Rescue --parameters 'UnreachableInstanceId=INSTANCEID'
```

EC2Rescue an instance and use the current instance subnet for the EC2Rescue instance

```
aws ssm start-automation-execution --document-name AWSSupport-ExecuteEC2Rescue --parameters 'UnreachableInstanceId=INSTANCEID,SubnetId=SelectedInstanceSubnet'
```

EC2Rescue an instance and use a custom subnet for the EC2Rescue instance

```
aws ssm start-automation-execution --document-name AWSSupport-ExecuteEC2Rescue --parameters 'UnreachableInstanceId=INSTANCEID,SubnetId=SUBNETID'
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

 **Required IAM Permissions** 

You must have at least **ssm:ExecuteAutomation** and **ssm:GetAutomationExecution** to be able to read the automation output\. For more information about the required permissions see **AWSSupport\-StartEC2RescueWorkflow**

 **Document Steps** 

1. aws:assertAwsResourceProperty \- Assert if the provided instance is Windows 

   1. \(EC2Rescue for Windows\) If the provided instance is Windows: 

      1. aws:executeAutomation \- Invoke AWSSupport\-StartEC2RescueWorkflow with the EC2Rescue for Windows offline script

      1. aws:executeAwsApi \- Retrieve the backup AMI ID from the nested automation

      1. aws:executeAwsApi \- Retrieve the EC2Rescue summary from the nested automation

   1. \(EC2Rescue for Linux\) If the provided instance is Linux: 

      1. aws:executeAutomation \- Invoke AWSSupport\-StartEC2RescueWorkflow with the EC2Rescue for Linux offline script

      1. aws:executeAwsApi \- Retrieve the backup AMI ID from the nested automation

      1. aws:executeAwsApi \- Retrieve the EC2Rescue summary from the nested automation

 **Outputs** 

getEC2RescueForWindowsResult\.Output

getWindowsBackupAmi\.ImageId

getEC2RescueForLinuxResult\.Output

getLinuxBackupAmi\.ImageId