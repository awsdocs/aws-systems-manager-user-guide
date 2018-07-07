# Walkthrough: Customize and Update Windows AMIs Using AWS\-UpdateWindowsAmi<a name="automation-awsdocs-win"></a>

The AWS\-UpdateWindowsAmi document enables you to automate image\-maintenance tasks on your Amazon Windows AMIs without having to author the workflow in JSON or YAML\. This document is supported for Windows Server 2008 R2 or later\. You can use the AWS\-UpdateWindowsAmi document to perform the following types of tasks\.
+ Install all Windows updates and upgrade Amazon software \(default behavior\)\.
+ Install specific Windows updates and upgrade Amazon software\.
+ Customize an AMI using your scripts\.

**Before You Begin**  
Before you begin working with Automation documents, configure roles and, optionally, CloudWatch Events for Automation\. For more information, see [Setting Up Automation](automation-setup.md)\.

**Note**  
Updates to SSM Agent are typically rolled out to different regions at different times\. When you customize or update an AMI, use only source AMIs published for the region that you are working in\. This will ensure that you are working with the latest SSM Agent released for that region and avoid compatibility issues\.

The AWS\-UpdateWindowsAmi document accepts the following input parameters\.


****  

| Parameter | Type | Description | 
| --- | --- | --- | 
|  SourceAmiId  |  String  |  \(Required\) The source AMI ID\.  | 
|  InstanceIamRole  |  String  |  \(Optional\) The name of the AWS Identity and Access Management \(IAM\) instance profile role you created in [Setting Up Automation](automation-setup.md)\. The instance profile role gives Automation permission to perform actions on your instances, such as executing commands or starting and stopping services\. The Automation document uses only the name of the instance profile role\. If you specify the Amazon Resource Name \(ARN\), the Automation execution fails\.  | 
|  AutomationAssumeRole  |  String  |  \(Optional\) The name of the IAM service role you created in [Setting Up Automation](automation-setup.md)\. The service role \(also called an assume role\) gives Automation permission to assume your IAM role and perform actions on your behalf\. For example, the service role allows Automation to create a new AMI when executing the `aws:createImage` action in an Automation document\. For this parameter, the complete ARN must be specified\.  | 
|  TargetAmiName  |  String  |  \(Optional\) The name of the new AMI after it is created\. The default name is a system\-generated string that includes the source AMI ID, and the creation time and date\.  | 
|  InstanceType  |  String  |  \(Optional\) The type of instance to launch as the workspace host\. Instance types vary by region\. The default type is t2\.medium\.  | 
|  PreUpdateScript  |  String  |  \(Optional\) A script to run before updating the AMI\. Enter a script in the Automation document or at runtime as a parameter\.  | 
|  PostUpdateScript  |  String  |  \(Optional\) A script to run after updating the AMI\. Enter a script in the Automation document or at runtime as a parameter\.  | 
|  IncludeKbs  |  String  |  \(Optional\) Specify one or more Microsoft Knowledge Base \(KB\) article IDs to include\. You can install multiple IDs using comma\-separated values\. Valid formats: KB9876543 or 9876543\.  | 
|  ExcludeKbs  |  String  |  \(Optional\) Specify one or more Microsoft Knowledge Base \(KB\) article IDs to exclude\. You can exclude multiple IDs using comma\-separated values\. Valid formats: KB9876543 or 9876543\.  | 
|  Categories  |  String  |  \(Optional\)Specify one or more update categories\. You can filter categories using comma\-separated values\. Options: Critical Update, Security Update, Definition Update, Update Rollup, Service Pack, Tool, Update, or Driver\. Valid formats include a single entry, for example: Critical Update\. Or, you can specify a comma separated list: Critical Update,Security Update,Definition Update\.  | 
|  SeverityLevels  |  String  |  \(Optional\) Specify one or more MSRC severity levels associated with an update\. You can filter severity levels using comma\-separated values\. Options: Critical, Important, Low, Moderate or Unspecified\. Valid formats include a single entry, for example: Critical\. Or, you can specify a comma separated list: Critical,Important,Low\.  | 

**Automation Steps**  
The AWS\-UpdateWindowsAmi document includes the following Automation steps, by default\.

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

You can use the AWS\-UpdateWindowsAmi document as a template to create your own document, as described in the next section\. For information about actions \(steps\) that are supported in Automation documents, see [Systems Manager Automation Document Reference](automation-actions.md)\. For information about how to use Automation documents, see [Systems Manager Automation Walkthroughs](automation-walk.md)