# AWSSupport\-ActivateWindowsWithAmazonLicense<a name="automation-awssupport-activatewindowswithamazonlicense"></a>

 **Description** 

The AWSSupport\-ActivateWindowsWithAmazonLicense automation document activates an EC2 instance for Windows Server with a license provided by Amazon\. The automation verifies and configures required key management service operating system settings and attempts activation\. This includes operating system routes to Amazon's key management servers, and key management service operating system settings\. Setting the `AllowOffline` parameter to `True` allows the automation to successfully target instances that are not managed by AWS Systems Manager, but requires a stop and start of the instance\.

**Note**  
This document cannot be used on Bring Your Own License \(BYOL\) Windows Server instances\. For information about using your own license, see [Microsoft Licensing on AWS](https://aws.amazon.com/windows/resources/licensing/)\. 

 **Document Type** 

Automation

 **Owner** 

Amazon

 **Platforms** 

Windows

 **Parameters** 
+ InstanceId

  Type: String

  Description: \(Required\) ID of your managed EC2 instance for Windows Server\.
+ ForceActivation

  Type: String

  Allowed values: True,False

  Default: False

  Description: \(Optional\) Set it to True if you want to proceed even if Windows is already activated\.
+ AllowOffline

  Type: String

  Allowed values: True,False

  Default: False

  Description: \(Optional\) Set it to True if you allow an offline Windows activation remediation in case the online troubleshooting fails, or if the provided instance is not a managed instance\.
**Important**  
The offline method requires that the provided EC2 instance be stopped and then started\. Data stored in instance store volumes will be lost\. The public IP address will change if you are not using an Elastic IP\.
+ SubnetId

  Type: String

  Default: CreateNewVPC

  Description: \(Optional\) Offline only \- The subnet ID for the EC2Rescue instance used to perform the offline troubleshooting\. Use SelectedInstanceSubnet to use the same subnet as your instance, or CreateNewVPC to create a new VPC\. IMPORTANT: The subnet must be in the same Availability Zone as InstanceId, and it must allow access to the SSM endpoints\.
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The IAM role for this execution\. If no role is specified, AWS Systems Manager Automation will use the permissions of the user that runs this document\.

 **Examples** 

Start the automation

```
aws ssm start-automation-execution --document-name AWSSupport-ActivateWindowsWithAmazonLicense --parameters "InstanceId=INSTANCEID"
```

Send the command with ForceActivation = True

```
aws ssm start-automation-execution --document-name AWSSupport-ActivateWindowsWithAmazonLicense --parameters "InstanceId=INSTANCEID,ForceActivation=True"
```

Send the command with AllowOffline = True

```
aws ssm start-automation-execution --document-name AWSSupport-ActivateWindowsWithAmazonLicense --parameters "InstanceId=INSTANCEID,AllowOffline=True"
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

 **Required IAM Permissions** 

It is recommended that the EC2 instance receiving the command has an IAM role with the **AmazonSSMManagedInstanceCore** Amazon managed policy attached\. You must have at least **ssm:ExecuteAutomation** and **ssm:SendCommand** to run the automation and send the command to the instance, plus **ssm:GetAutomationExecution** to be able to read the automation output\. For the offline remediation, see the permissions needed by **AWSSupport\-StartEC2RescueWorkflow**\.

 **Document Steps** 

1. aws:assertAwsResourceProperty \- Check the provided instance's platform is Windows\.

1. aws:assertAwsResourceProperty \- Confirm the provided instance is a managed instance

   1. \(Online activation fix\) If the input instance is a managed instance, then run aws:runCommand to run the PowerShell script to attempt to fix Windows activation\.

   1. \(Offline activation fix\) If the input instance is not a managed instance:

      1. aws:assertAwsResourceProperty \- Verifies the AllowOffline flag is set to True\. If so, the offline fix starts, otherwise the workflow ends\.

      1. aws:executeAutomation \- Invoke AWSSupport\-StartEC2RescueWorkflow with the Windows activation offline fix script\. The script leverages EC2Config or EC2Launch depending on the OS version\.

      1. aws:executeAwsApi \- Read the result from AWSSupport\-StartEC2RescueWorkflow\.

 **Outputs** 

activateWindows\.Output

getActivateWindowsOfflineResult\.Output