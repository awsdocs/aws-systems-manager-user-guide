# Walkthrough: Patch a Linux AMI \(AWS CLI\)<a name="automation-walk-patch-linux-ami-cli"></a>

This Systems Manager Automation walkthrough shows you how to use the AWS CLI and the Systems Manager `AWS-UpdateLinuxAmi` document to automatically patch a Linux AMI with the latest versions of packages that you specify\. The `AWS-UpdateLinuxAmi` document also automates the installation of additional site\-specific packages and configurations\. You can update a variety of Linux distributions using this walkthrough, including Ubuntu, CentOS, RHEL, SLES, or Amazon Linux AMIs\. For a full list of supported Linux versions, see [Patch Manager prerequisites](patch-manager-prerequisites.md)\.

The `AWS-UpdateLinuxAmi` document enables you to automate image\-maintenance tasks without having to author the workflow in JSON or YAML\. You can use the `AWS-UpdateLinuxAmi` document to perform the following types of tasks\.
+ Upgrade all distribution packages and Amazon software on an Amazon Linux, Red Hat, Ubuntu, SLES, or Cent OS Amazon Machine Image \(AMI\)\. This is the default document behavior\.
+ Install SSM Agent on an existing image to enable Systems Manager capabilities, such as remote command execution using Run Command or software inventory collection using Inventory\.
+ Install additional software packages\.

**Before You Begin**  
Before you begin working with Automation documents, configure roles and, optionally, EventBridge for Automation\. For more information, see [Getting started with Automation](automation-setup.md)\. This walkthrough also requires that you specify the name of an AWS Identity and Access Management \(IAM\) instance profile\. For more information about creating an IAM instance profile, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\.

The `AWS-UpdateLinuxAmi` document accepts the following input parameters\.


****  

| Parameter | Type | Description | 
| --- | --- | --- | 
|  SourceAmiId  |  String  |  \(Required\) The source AMI ID\. You can automatically reference the latest ID of an EC2 AMI for Linux by using a Systems Manager Parameter Store *public* parameter\. For more information, see [Query for the latest Amazon Linux AMI IDs using AWS Systems Manager Parameter Store](http://aws.amazon.com/blogs/compute/query-for-the-latest-amazon-linux-ami-ids-using-aws-systems-manager-parameter-store/)\.  | 
|  IamInstanceProfileName  |  String  |  \(Required\) The name of the IAM instance profile role you created in [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. The instance profile role gives Automation permission to perform actions on your instances, such as running commands or starting and stopping services\. The Automation document uses only the name of the instance profile role\. If you specify the Amazon Resource Name \(ARN\), the Automation execution fails\.  | 
|  AutomationAssumeRole  |  String  |  \(Required\) The name of the IAM service role you created in [Getting started with Automation](automation-setup.md)\. The service role \(also called an assume role\) gives Automation permission to assume your IAM role and perform actions on your behalf\. For example, the service role allows Automation to create a new AMI when running the `aws:createImage` action in an Automation document\. For this parameter, the complete ARN must be specified\.  | 
|  TargetAmiName  |  String  |  \(Optional\) The name of the new AMI after it is created\. The default name is a system\-generated string that includes the source AMI ID, and the creation time and date\.  | 
|  InstanceType  |  String  |  \(Optional\) The type of instance to launch as the workspace host\. Instance types vary by region\. The default type is t2\.micro\.  | 
|  PreUpdateScript  |  String  |  \(Optional\) URL of a script to run before updates are applied\. Default \(\\"none\\"\) is to not run a script\.  | 
|  PostUpdateScript  |  String  |  \(Optional\) URL of a script to run after package updates are applied\. Default \(\\"none\\"\) is to not run a script\.  | 
|  IncludePackages  |  String  |  \(Optional\) Only update these named packages\. By default \(\\"all\\"\), all available updates are applied\.  | 
|  ExcludePackages  |  String  |  \(Optional\) Names of packages to hold back from updates, under all conditions\. By default \(\\"none\\"\), no package is excluded\.  | 

**Automation Steps**  
The `AWS-UpdateLinuxAmi` document includes the following Automation steps, by default\.

**Step 1: launchInstance \(aws:runInstances action\) **  
This step launches an instance using Amazon EC2 userdata and an IAM instance profile role\. Userdata installs the appropriate SSM Agent, based on the operating system\. Installing SSM Agent enables you to utilize Systems Manager capabilities such as Run Command, State Manager, and Inventory\.

**Step 2: updateOSSoftware \(aws:runCommand action\) **  
This step runs the following commands on the launched instance:  
+ Downloads an update script from Amazon S3\.
+ Runs an optional pre\-update script\.
+ Updates distribution packages and Amazon software\.
+ Runs an optional post\-update script\.
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

**Note**  
By default, when Automation runs the `AWS-UpdateLinuxAmi` document, the system creates a temporary instance in the default VPC \(172\.30\.0\.0/16\)\. If you deleted the default VPC, you will receive the following error:  
VPC not defined 400  
To solve this problem, you must make a copy of the `AWS-UpdateLinuxAmi` document and specify a subnet ID\. For more information, see [VPC not defined 400](automation-troubleshooting.md#automation-trbl-common-vpc)\.

**To create a patched AMI using Automation**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to run the `AWS-UpdateLinuxAmi` document and run the Automation workflow\. In the parameters section, specify an AMI source ID, an EC2 instance profile role, and your Automation service role\.

   ```
   aws ssm start-automation-execution \
       --document-name "AWS-UpdateLinuxAmi" \
       --parameters \
       SourceAmiId=ami-0080e4c5bc078760e, \
       IamInstanceProfileName=ManagedInstanceRole, \
       AutomationAssumeRole='arn:aws:iam::{{global:ACCOUNT_ID}}:role/AutomationServiceRole'"
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

   The update process can take 30 minutes or more to complete\.
**Note**  
You can also monitor the status of the workflow in the console\. In the execution list, choose the execution you just ran and then choose the **Steps** tab\. This tab shows you the status of the workflow actions\.

After the workflow finishes, launch a test instance from the updated AMI to verify changes\.

**Note**  
If any step in the workflow fails, information about the failure is listed on the **Automation Executions** page\. The workflow is designed to terminate the temporary instance after successfully completing all tasks\. If a step fails, the system might not terminate the instance\. So if a step fails, manually terminate the temporary instance\.