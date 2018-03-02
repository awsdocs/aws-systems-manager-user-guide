# Working with Predefined Automation Documents<a name="automation-awsdocs"></a>

To help you get started quickly, Systems Manager provides the following pre\-defined Automation documents\. These documents are maintained by Amazon Web Services\. 

**Note**  
Any document name that includes *WithApproval*, means the document includes the [aws:approve](automation-actions.md#automation-action-approve) action\. This action temporarily pauses an Automation execution until designated principals either approve or reject the action\. After the required number of approvals is reached, the Automation execution resumes\. 


****  

| Document Name\(s\) | Purpose | 
| --- | --- | 
|  AWS\-DeleteCloudFormationStack AWS\-DeleteCloudFormationStackWithApproval  |  Use this document to delete a AWS CloudFormation stack\. You must specify the stack ID\.  | 
|  AWS\-RestartEC2Instance AWS\-RestartEC2InstanceWithApproval  |  Use this document to restart one or more Amazon EC2 instances \(Windows or Linux\)\. Separate instance IDs with a comma \(,\)\. If you want to change the state of one or more instances from a custom Automation document, then specify the [aws:changeInstanceState](automation-actions.md#automation-action-changestate) action in your document\.  | 
|  AWS\-StartEC2Instance AWS\-StartEC2InstanceWithApproval  |  Use this document to start one or more Amazon EC2 instances \(Windows or Linux\)\. Separate instance IDs with a comma \(,\)\. If you want to change the state of one or more instances from a custom Automation document, then specify the [aws:changeInstanceState](automation-actions.md#automation-action-changestate) action in your document\.  | 
|  AWS\-StopEC2Instance AWS\-StopEC2InstanceWithApproval  |  Use this document to stop one or more Amazon EC2 instances \(Windows or Linux\)\. Separate instance IDs with a comma \(,\)\. If you want to change the state of one or more instances from a custom Automation document, then specify the [aws:changeInstanceState](automation-actions.md#automation-action-changestate) action in your document\.  | 
|  AWS\-TerminateEC2Instance AWS\-TerminateEC2InstanceWithApproval  |  Use this document to terminate one or more Amazon EC2 instances \(Windows or Linux\)\. Separate instance IDs with a comma \(,\)\. If you want to change the state of one or more instances from a custom Automation document, then specify the [aws:changeInstanceState](automation-actions.md#automation-action-changestate) action in your document\.  | 
|  AWS\-UpdateCloudFormationStack AWS\-UpdateCloudFormationStackWithApproval  |  Use this document to perform an update operation on an existing stack by using a specified template\. You must specify a URL to the template that will be used to update the stack\. These documents execute a AWS Lambda function\. You must provide an Amazon Resource Name \(ARN\) for an IAM role that Lambda can use to execute the function\.  | 
|  AWS\-UpdateLinuxAmi  |  Use this document to automate image\-maintenance tasks\. For more information, see [Customizing and Updating Linux AMIs Using AWS\-UpdateLinuxAmi](#automation-awsdocs-linux)  | 
|  AWS\-UpdateWindowsAmi  |  Use this document to automate image\-maintenance tasks\. For more information, see [Customizing and Updating Windows AMIs Using AWS\-UpdateWindowsAmi](#automation-awsdocs-win)  | 
|  AWSSupport\-ExecuteEC2Rescue  |  Use this document to diagnose and troubleshoot problems on Amazon EC2 Windows Server instances\. For more information, see [Run the EC2Rescue Tool on Unreachable Instances](automation-ec2rescue.md)\.  | 
|  AWSSupport\-ResetAccess  |  Use this document to automatically reenable local Administrator password generation on Amazon EC2 Windows instances\. For more information, see [Reset the Local Administrator Password on Amazon EC2 Windows Instances](automation-ec2reset.md)\.  | 

You can view the JSON for these document in the Systems Manager console\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose a document, and then choose **View details**\.

1. Choose the **Content** tab\.

## Customizing and Updating Linux AMIs Using AWS\-UpdateLinuxAmi<a name="automation-awsdocs-linux"></a>

The AWS\-UpdateLinuxAmi document enables you to automate image\-maintenance tasks without having to author the workflow in JSON or YAML\. You can use the AWS\-UpdateLinuxAmi document to perform the following types of tasks\.

+ Upgrade all distribution packages and Amazon software on an Amazon Linux, Red Hat, Ubuntu, SLES, or Cent OS Amazon Machine Image \(AMI\)\. This is the default document behavior\.

+ Install the SSM Agent on an existing image to enable Systems Manager capabilities, such as remote command execution using Run Command or software inventory collection using Inventory\.

+ Install additional software packages\.

**Before You Begin**  
Before you begin working with Automation documents, configure roles and, optionally, CloudWatch Events for Automation\. For more information, see [Setting Up Automation](automation-setup.md)\.

The AWS\-UpdateLinuxAmi document accepts the following input parameters\.


****  

| Parameter | Type | Description | 
| --- | --- | --- | 
|  SourceAmiId  |  String  |  \(Required\) The source AMI ID\.  | 
|  InstanceIamRole  |  String  |  \(Optional\) The name of the AWS Identity and Access Management \(IAM\) instance profile role you created in [Setting Up Automation](automation-setup.md)\. The instance profile role gives Automation permission to perform actions on your instances, such as executing commands or starting and stopping services\. The Automation document uses only the name of the instance profile role\. If you specify the Amazon Resource Name \(ARN\), the Automation execution fails\.  | 
|  AutomationAssumeRole  |  String  |  \(Optional\) The name of the IAM service role you created in [Setting Up Automation](automation-setup.md)\. The service role \(also called an assume role\) gives Automation permission to assume your IAM role and perform actions on your behalf\. For example, the service role allows Automation to create a new AMI when executing the `aws:createImage` action in an Automation document\. For this parameter, the complete ARN must be specified\.  | 
|  TargetAmiName  |  String  |  \(Optional\) The name of the new AMI after it is created\. The default name is a system\-generated string that includes the source AMI ID, and the creation time and date\.  | 
|  InstanceType  |  String  |  \(Optional\) The type of instance to launch as the workspace host\. Instance types vary by region\. The default type is t2\.micro\.  | 
|  PreUpdateScript  |  String  |  \(Optional\) URL of a script to run before updates are applied\. Default \(\\"none\\"\) is to not run a script\.  | 
|  PostUpdateScript  |  String  |  \(Optional\) URL of a script to run after package updates are applied\. Default \(\\"none\\"\) is to not run a script\.  | 
|  IncludePackages  |  String  |  \(Optional\) Only update these named packages\. By default \(\\"all\\"\), all available updates are applied\.  | 
|  ExcludePackages  |  String  |  \(Optional\) Names of packages to hold back from updates, under all conditions\. By default \(\\"none\\"\), no package is excluded\.  | 

**Automation Steps**  
The AWS\-UpdateLinuxAmi document includes the following Automation steps, by default\.

**Step 1: launchInstance \(aws:runInstances action\) **  
This step launches an instance using Amazon EC2 userdata and an IAM instance profile role\. Userdata installs the appropriate SSM Agent, based on the operating system\. Installing the SSM Agent enables you to utilize Systems Manager capabilities such as Run Command, State Manager, and Inventory\.

**Step 2: updateOSSoftware \(aws:runCommand action\) **  
This step executes the following commands on the launched instance:  

+ Downloads an update script from Amazon S3\.

+ Executes an optional pre\-update script\.

+ Updates distribution packages and Amazon software\.

+ Executes an optional post\-update script\.
The execution log is stored in the /tmp folder for the user to view later\.  
If you want to upgrade a specific set of packages, you can supply the list using the `IncludePackages` parameter\. When provided, the system attempts to update only these packages and their dependencies\. No other updates are performed\. By default, when no *include* packages are specified, the program updates all available packages\.  
If you want to exclude upgrading a specific set of packages, you can supply the list to the `ExcludePackages` parameter\. If provided, these packages remain at their current version, independent of any other options specified\. By default, when no *exclude* packages are specified, no packages are excluded\.

**Step 3: stopInstance \(aws:changeInstanceState action\)**  
This step stops the updated instance\.

**Step 4: createImage \(aws:createImage action\) **  
This step creates a new AMI with a descriptive name that links it to the source ID and creation time\. For example: “AMI Generated by EC2 Automation on \{\{global:DATE\_TIME\}\} from \{\{SourceAmiId\}\}” where DATE\_TIME and SourceID represent Automation variables\.

**Step 5: terminateInstance \(aws:changeInstanceState action\) **  
This step cleans up the execution by terminating the running instance\.

**Output**  
The execution returns the new AMI ID as output\.

You can use the AWS\-UpdateLinuxAmi document as a template to create your own document, as described in the next section\. For information about actions \(steps\) that are supported in Automation documents, see [Systems Manager Automation Document Reference](automation-actions.md)\. For information about how to use Automation documents, see [Systems Manager Automation Walkthroughs](automation-walk.md)

## Customizing and Updating Windows AMIs Using AWS\-UpdateWindowsAmi<a name="automation-awsdocs-win"></a>

The AWS\-UpdateWindowsAmi document enables you to automate image\-maintenance tasks on your Amazon Windows AMIs without having to author the workflow in JSON or YAML\. This document is supported for Windows Server 2008 R2 or later\. You can use the AWS\-UpdateWindowsAmi document to perform the following types of tasks\.

+ Install all Windows updates and upgrade Amazon software \(default behavior\)\.

+ Install specific Windows updates and upgrade Amazon software\.

+ Customize an AMI using your scripts\.

**Before You Begin**  
Before you begin working with Automation documents, configure roles and, optionally, CloudWatch Events for Automation\. For more information, see [Setting Up Automation](automation-setup.md)\.

**Note**  
Updates to the SSM Agent are typically rolled out to different regions at different times\. When you customize or update an AMI, use only source AMIs published for the region that you are working in\. This will ensure that you are working with the latest SSM Agent released for that region and avoid compatibility issues\.

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
This step enables you to specify a script as a string that executes before updates are installed\.

**Step 3: updateEC2Config \(aws:runCommand action\)**  
This step uses the AWS\-InstallPowerShellModule public document to download an AWS public PowerShell module\. Systems Manager verifies the integrity of the module by using an SHA\-256 hash\. Systems Manager then checks the operating system to determine whether to update EC2Config or EC2Launch\. EC2Config runs on Windows Server 2008 R2 through Windows Server 2012 R2\. EC2Launch runs on Windows Server 2016\.

**Step 4: updateSSMAgent \(aws:runCommand action\)**  
This step updates the SSM Agent by using the AWS\-UpdateSSMAgent public document\.

**Step 5: updateAWSPVDriver \(aws:runCommand action\)**  
This step updates AWS PV drivers by using the AWS\-ConfigureAWSPackage public document\.

**Step 6: updateAwsEnaNetworkDriver \(aws:runCommand action\)**  
This step updates AWS ENA Network drivers by using the AWS\-ConfigureAWSPackage public document\.

**Step 7: installWindowsUpdates \(aws:runCommand action\) **  
This step installs Windows updates by using the AWS\-InstallWindowsUpdates public document\. By default, Systems Manager searches for and installs all missing updates\. You can change the default behavior by specifying one of the following parameters: `IncludeKbs`, `ExcludeKbs`, `Categories`, or `SeverityLevels`\. 

**Step 8: runPostUpdateScript \(aws:runCommand action\)**  
This step enables you to specify a script as a string that executes after the updates have been installed\.

**Step 9: runSysprepGeneralize \(aws:runCommand action\) **  
This step uses the AWS\-InstallPowerShellModule public document to download an AWS public PowerShell module\. Systems Manager verifies the integrity of the module by using an SHA\-256 hash\. Systems Manager then executes sysprep using AWS\-supported methods for either EC2Launch \(Windows Server 2016\) or EC2Config \(Windows Server 2008 R2 through 2012 R2\)\.

**Step 10: stopInstance \(aws:changeInstanceState action\) **  
This step stops the updated instance\. 

**Step 11: createImage \(aws:createImage action\) **  
This step creates a new AMI with a descriptive name that links it to the source ID and creation time\. For example: “AMI Generated by EC2 Automation on \{\{global:DATE\_TIME\}\} from \{\{SourceAmiId\}\}” where DATE\_TIME and SourceID represent Automation variables\.

**Step 12: TerminateInstance \(aws:changeInstanceState action\) **  
This step cleans up the execution by terminating the running instance\. 

**Output**  
This section enables you to designate the outputs of various steps or values of any parameter as the Automation output\. By default, the output is the ID of the updated Windows AMI created by the execution\.

You can use the AWS\-UpdateWindowsAmi document as a template to create your own document, as described in the next section\. For information about actions \(steps\) that are supported in Automation documents, see [Systems Manager Automation Document Reference](automation-actions.md)\. For information about how to use Automation documents, see [Systems Manager Automation Walkthroughs](automation-walk.md)