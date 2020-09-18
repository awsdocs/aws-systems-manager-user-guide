# Walkthrough: Patch a Windows Server AMI<a name="automation-walk-patch-windows-ami-cli"></a>

The `AWS-UpdateWindowsAmi` document enables you to automate image maintenance tasks on your Amazon Windows AMIs without having to author the workflow in JSON or YAML\. This document is supported for Windows Server 2008 R2 or later\. You can use the `AWS-UpdateWindowsAmi` document to perform the following types of tasks\.
+ Install all Windows updates and upgrade Amazon software \(default behavior\)\.
+ Install specific Windows updates and upgrade Amazon software\.
+ Customize an AMI using your scripts\.

**Before You Begin**  
Before you begin working with Automation documents, configure roles and, optionally, EventBridge for Automation\. For more information, see [Getting started with Automation](automation-setup.md)\. This walkthrough also requires that you specify the name of an AWS Identity and Access Management \(IAM\) instance profile\. For more information about creating an IAM instance profile, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\.

**Note**  
Updates to SSM Agent are typically rolled out to different regions at different times\. When you customize or update an AMI, use only source AMIs published for the region that you are working in\. This will ensure that you are working with the latest SSM Agent released for that region and avoid compatibility issues\.

The `AWS-UpdateWindowsAmi` document accepts the following input parameters\.


****  

| Parameter | Type | Description | 
| --- | --- | --- | 
|  SourceAmiId  |  String  |  \(Required\) The source AMI ID\. You can automatically reference the latest Windows Server AMI ID by using a Systems Manager Parameter Store *public* parameter\. For more information, see [Query for the latest Windows AMI IDs using AWS Systems Manager Parameter Store](http://aws.amazon.com/blogs/mt/query-for-the-latest-windows-ami-using-systems-manager-parameter-store/)\.  | 
|  IamInstanceProfileName  |  String  |  \(Required\) The name of the IAM instance profile role you created in [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. The instance profile role gives Automation permission to perform actions on your instances, such as running commands or starting and stopping services\. The Automation document uses only the name of the instance profile role\. If you specify the Amazon Resource Name \(ARN\), the Automation execution fails\.  | 
|  AutomationAssumeRole  |  String  |  \(Required\) The name of the IAM service role you created in [Getting started with Automation](automation-setup.md)\. The service role \(also called an assume role\) gives Automation permission to assume your IAM role and perform actions on your behalf\. For example, the service role allows Automation to create a new AMI when running the `aws:createImage` action in an Automation document\. For this parameter, the complete ARN must be specified\.  | 
|  TargetAmiName  |  String  |  \(Optional\) The name of the new AMI after it is created\. The default name is a system\-generated string that includes the source AMI ID, and the creation time and date\.  | 
|  InstanceType  |  String  |  \(Optional\) The type of instance to launch as the workspace host\. Instance types vary by region\. The default type is t2\.medium\.  | 
|  PreUpdateScript  |  String  |  \(Optional\) A script to run before updating the AMI\. Enter a script in the Automation document or at runtime as a parameter\.  | 
|  PostUpdateScript  |  String  |  \(Optional\) A script to run after updating the AMI\. Enter a script in the Automation document or at runtime as a parameter\.  | 
|  IncludeKbs  |  String  |  \(Optional\) Specify one or more Microsoft Knowledge Base \(KB\) article IDs to include\. You can install multiple IDs using comma\-separated values\. Valid formats: KB9876543 or 9876543\.  | 
|  ExcludeKbs  |  String  |  \(Optional\) Specify one or more Microsoft Knowledge Base \(KB\) article IDs to exclude\. You can exclude multiple IDs using comma\-separated values\. Valid formats: KB9876543 or 9876543\.  | 
|  Categories  |  String  |  \(Optional\)Specify one or more update categories\. You can filter categories using comma\-separated values\. Options: Critical Update, Security Update, Definition Update, Update Rollup, Service Pack, Tool, Update, or Driver\. Valid formats include a single entry, for example: Critical Update\. Or, you can specify a comma separated list: Critical Update,Security Update,Definition Update\.  | 
|  SeverityLevels  |  String  |  \(Optional\) Specify one or more MSRC severity levels associated with an update\. You can filter severity levels using comma\-separated values\. Options: Critical, Important, Low, Moderate or Unspecified\. Valid formats include a single entry, for example: Critical\. Or, you can specify a comma separated list: Critical,Important,Low\.  | 

**Automation Steps**  
The `AWS-UpdateWindowsAmi` document includes the following Automation steps, by default\.

**Step 1: launchInstance \(aws:runInstances action\)**  
This step launches an instance with an IAM instance profile role from the specified `SourceAmiID`\.

**Step 2: runPreUpdateScript \(aws:runCommand action\)**  
This step enables you to specify a script as a string that runs before updates are installed\.

**Step 3: updateEC2Config \(aws:runCommand action\)**  
This step uses the AWS\-InstallPowerShellModule public document to download an AWS public PowerShell module\. Systems Manager verifies the integrity of the module by using an SHA\-256 hash\. Systems Manager then checks the operating system to determine whether to update EC2Config or EC2Launch\. EC2Config runs on Windows Server 2008 R2 through Windows Server 2012 R2\. EC2Launch runs on Windows Server 2016\.

**Step 4: updateSSMAgent \(aws:runCommand action\)**  
This step updates SSM Agent by using the AWS\-UpdateSSMAgent public document\.

**Step 5: updateAWSPVDriver \(aws:runCommand action\)**  
This step updates AWS PV drivers by using the AWS\-ConfigureAWSPackage public document\.

**Step 6: updateAwsEnaNetworkDriver \(aws:runCommand action\)**  
This step updates AWS ENA Network drivers by using the AWS\-ConfigureAWSPackage public document\.

**Step 7: installWindowsUpdates \(aws:runCommand action\) **  
This step installs Windows updates by using the AWS\-InstallWindowsUpdates public document\. By default, Systems Manager searches for and installs all missing updates\. You can change the default behavior by specifying one of the following parameters: `IncludeKbs`, `ExcludeKbs`, `Categories`, or `SeverityLevels`\. 

**Step 8: runPostUpdateScript \(aws:runCommand action\)**  
This step enables you to specify a script as a string that runs after the updates have been installed\.

**Step 9: runSysprepGeneralize \(aws:runCommand action\) **  
This step uses the AWS\-InstallPowerShellModule public document to download an AWS public PowerShell module\. Systems Manager verifies the integrity of the module by using an SHA\-256 hash\. Systems Manager then runs sysprep using AWS\-supported methods for either EC2Launch \(Windows Server 2016\) or EC2Config \(Windows Server 2008 R2 through 2012 R2\)\.

**Step 10: stopInstance \(aws:changeInstanceState action\) **  
This step stops the updated instance\. 

**Step 11: createImage \(aws:createImage action\) **  
This step creates a new AMI with a descriptive name that links it to the source ID and creation time\. For example: “AMI Generated by EC2 Automation on \{\{global:DATE\_TIME\}\} from \{\{SourceAmiId\}\}” where DATE\_TIME and SourceID represent Automation variables\.

**Step 12: TerminateInstance \(aws:changeInstanceState action\) **  
This step cleans up the execution by terminating the running instance\. 

**Output**  
This section enables you to designate the outputs of various steps or values of any parameter as the Automation output\. By default, the output is the ID of the updated Windows AMI created by the execution\.

**Note**  
By default, when Automation runs the `AWS-UpdateWindowsAmi` document and creates a temporary instance, the system uses the default VPC \(172\.30\.0\.0/16\)\. If you deleted the default VPC, you will receive the following error:  
VPC not defined 400  
To solve this problem, you must make a copy of the `AWS-UpdateWindowsAmi` document and specify a subnet ID\. For more information, see [VPC not defined 400](automation-troubleshooting.md#automation-trbl-common-vpc)\.

**To create a patched Windows AMI by using Automation**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to run the `AWS-UpdateWindowsAmi` document\. In the parameters section, specify an AMI source ID, an EC2 instance profile role, and your Automation service role\. The example command below uses a recent Amazon EC2 AMI to minimize the number of patches that need to be applied\. If you run this command more than once, you must specify a unique value for `targetAMIname`\. AMI names must be unique\.

   ```
   aws ssm start-automation-execution --document-name="AWS-UpdateWindowsAmi" --parameters SourceAmiId='ami-0246f4914689c475f',IamInstanceProfileName='ManagedInstanceProfile',AutomationAssumeRole='arn:aws:iam::{{global:ACCOUNT_ID}}:role/AutomationServiceRole'
   ```

   The command returns an execution ID\. Copy this ID to the clipboard\. You will use this ID to view the status of the workflow\.

   ```
   {
       "AutomationExecutionId": "ID"
   }
   ```

1. To view the workflow execution using the AWS CLI, run the following command:

   ```
   aws ssm describe-automation-executions
   ```

1. To view details about the execution progress, run the following command\.

   ```
   aws ssm get-automation-execution --automation-execution-id ID
   ```

**Note**  
Depending on the number of patches applied, the Windows patching process run in this sample workflow can take 30 minutes or more to complete\.