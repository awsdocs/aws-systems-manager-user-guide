# AWSSupport\-ResetAccess<a name="automation-awssupport-resetaccess"></a>

 **Description** 

This document will use the EC2Rescue tool on the specified EC2 instance to re\-enable password decryption via the EC2 Console \(Windows\), or to generate and add a new SSH key pair \(Linux\)\. If you lost your key pair, this automation will create a password\-enabled AMI that you can use to launch a new EC2 instance with a key pair you own \(Windows\)\.

 **Document Type** 

Automation

 **Owner** 

Amazon

 **Platforms** 

Windows, Linux

 **Parameters** 
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
+ EC2RescueInstanceType

  Type: String

  Allowed values: t2\.small,t2\.medium,t2\.large

  Default: t2\.small

  Description: \(Required\) The EC2 instance type for the EC2Rescue instance\. Recommended size: t2\.small\.
+ AssumeRole

  Type: String

  Description: \(Optional\) The IAM role for this execution\. If no role is specified, AWS Systems Manager Automation will use the permissions of the user that executes this document\.

 **Examples** 

Enable EC2 password generation for Windows

```
aws ssm start-automation-execution --document-name AWSSupport-ResetAccess --parameters 'InstanceId=WINDOWSINSTANCEID'
```

Enable EC2 password generation for Windows and use the provided instance's subnet for the EC2Rescue instance

```
aws ssm start-automation-execution --document-name AWSSupport-ResetAccess --parameters 'InstanceId=WINDOWSINSTANCEID,SubnetId=SelectedInstanceSubnet'
```

Generate a new SSH key for Linux

```
aws ssm start-automation-execution --document-name AWSSupport-ResetAccess --parameters 'InstanceId=LINUXINSTANCEID'
```

Generate a new SSH key for Linux and use the provided instance's subnet for the EC2Rescue instance

```
aws ssm start-automation-execution --document-name AWSSupport-ResetAccess --parameters 'InstanceId=LINUXINSTANCEID,SubnetId=SelectedInstanceSubnet'
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

 **Required IAM Permissions** 

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