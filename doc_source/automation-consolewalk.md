# Automation Console Walkthrough: Patch a Linux AMI<a name="automation-consolewalk"></a>

This Systems Manager Automation walkthrough shows you how to use the console and the Systems Manager AWS\-UpdateLinuxAmi document to automatically patch a Linux AMI with the latest versions of packages that you specify\. You can update any of the following Linux versions using this walkthrough: Ubuntu, CentOS, RHEL, SLES, or Amazon Linux AMIs\. The AWS\-UpdateLinuxAmi document also automates the installation of additional site\-specific packages and configurations\.

The walkthrough shows you how to specify parameters for the AWS\-UpdateLinuxAmi document at runtime\. If you want to add steps to your automation or specify default values, you can use the AWS\-UpdateLinuxAmi document as a template\.

For more information about working with Systems Manager documents, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\. For information about actions you can add to a document, see [Systems Manager Automation Document Reference](automation-actions.md)\.

When you run the AWS\-UpdateLinuxAmi document, Automation performs the following tasks\.

1. Launches a temporary Amazon EC2 instance from a Linux AMI\. The instance is configured with a User Data script that installs SSM Agent\. SSM Agent runs scripts sent remotely from Systems Manager Run Command\.

1. Updates the Instance by performing the following actions:

   1. \(Optional\) Invokes a user\-provided pre\-update script on the instance\.

   1. Updates AWS tools on the instance, if any tools are present\.

   1. Updates distribution packages on the instance by using the native package manager\.

   1. \(Optional\) Invokes a user\-provided post\-update script on the instance\.

1. Stops the temporary instance\.

1. Creates a new AMI from the stopped instance\.

1. Terminates the instance\.

After Automation successfully completes this workflow, the new AMI is available in the console on the **AMIs** page\.

**Important**  
If you use Automation to create an AMI from an instance, be aware that credentials, passwords, data, or other confidential information on the instance are recorded on the new image\. Use caution when creating an AMI from an instance\.

As you get started with Automation, note the following restrictions\.
+ Automation does not perform resource clean\-up\. In the event your workflow stops before reaching the final instance\-termination step in the example workflow, you might need to stop instances manually or disable services that were started during the Automation workflow\.
+ If you use userdata with Automation, the userdata must be base\-64 encoded\.
+ Automation retains execution records for 30 days\.
+ Systems Manager and Automation have the following [service limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_ssm)\.

**Before you begin**  
Create an AWS Identity and Access Management \(IAM\) instance profile role and Automation service role \(or assume role\)\. For more information about these roles and how to quickly create them from an AWS CloudFormation template, see [Method 1: Use AWS CloudFormation to Configure Roles for Automation](automation-cf.md)\.

We recommend that you also collect the following information before you begin\.
+ The source ID of the AMI to update\.
+ \(Optional\) The URL of a script to run before updates are applied\.
+ \(Optional\) The URL of a script to run after updates are applied\.
+ \(Optional\) The names of specific packages to update\. By default, Automation updates all packages\.
+ \(Optional\) The names of specific packages to exclude from updating\.

**Note**  
By default, when Automation runs the AWS\-UpdateLinuxAmi document, the system creates a temporary instance in the default VPC \(172\.30\.0\.0/16\)\. If you deleted the default VPC, you will receive the following error:  
VPC not defined 400  
To solve this problem, you must make a copy of the AWS\-UpdateLinuxAmi document and specify a subnet ID\. For more information, see [VPC not defined 400](automation-troubleshooting.md#automation-trbl-common-vpc)\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To create a patched AMI using Automation \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Automation**\.

1. Choose **Execute automation**\.

1. In the **Automation document** list, choose **AWS\-UpdateLinuxAmi**\.

1. In the **Document details** section, verify that **Document version** is set to **1**\.

1. In the **Execution mode** section, choose **Execute the entire automation at once**\.

1. Leave the **Targets and Rate Control** option disabled\.

1. In the **Input parameters** section, enter the information you collected in the **Before You Begin** section\.

1. Choose **Execute automation**\. The console displays the status of the Automation execution\.

**To create a patched AMI using Automation \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Services** in the navigation pane, and then choose **Automations**\.

1. Choose **Run automation**\.

1. In the **Document name** list, choose **AWS\-UpdateLinuxAmi**\.

1. In the **Version** list, choose **1**\.

1. In the **Input parameters** section, enter the information you collected in the **Before You Begin** section\.

1. Choose **Run automation**\. The system displays an automation execution ID\. Choose **OK**\.

1. In the execution list, choose the execution you just ran and then choose the **Steps** tab\. This tab shows you the status of the workflow actions\. The update process can take 30 minutes or more to complete\.

After the workflow finishes, launch a test instance from the updated AMI to verify changes\.

**Note**  
If any step in the workflow fails, information about the failure is listed on the **Automation Executions** page\. The workflow is designed to terminate the temporary instance after successfully completing all tasks\. If a step fails, the system might not terminate the instance\. So if a step fails, manually terminate the temporary instance\.