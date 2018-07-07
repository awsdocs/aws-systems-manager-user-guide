# Automation CLI Walkthrough: Patch a Linux AMI<a name="automation-cliwalk"></a>

This Systems Manager Automation walkthrough shows you how to use the AWS CLI and the Systems Manager AWS\-UpdateLinuxAmi document to automatically patch a Linux AMI\. You can update any of the following Linux versions using this walkthrough: Ubuntu, CentOS, RHEL, SLES, or Amazon Linux AMIs\. The AWS\-UpdateLinuxAmi document also automates the installation of additional site\-specific packages and configurations\.

When you run the AWS\-UpdateLinuxAmi document, Automation performs the following tasks\.

1. Launches a temporary Amazon EC2 instance from a Linux AMI\. The instance is configured with a User Data script that installs SSM Agent\. The SSM Agent runs scripts sent remotely from Systems Manager Run Command\.

1. Updates the Instance by performing the following actions:

   1. Invokes a user\-provided pre\-update script on the instance\.

   1. Updates AWS tools on the instance, if any tools are present\.

   1. Updates distribution packages on the instance by using the native package manager\.

   1. Invokes a user\-provided post\-update script on the instance\.

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

We recommend that you also collect the source ID of the AMI to update\.

**Note**  
By default, when Automation runs the AWS\-UpdateLinuxAmi document, the system creates a temporary instance in the default VPC \(172\.30\.0\.0/16\)\. If you deleted the default VPC, you will receive the following error:  
VPC not defined 400  
To solve this problem, you must make a copy of the AWS\-UpdateLinuxAmi document and specify a subnet ID\. For more information, see [VPC not defined 400](automation-troubleshooting.md#automation-trbl-common-vpc)\.

**To create a patched AMI using Automation**

1. [Download](https://aws.amazon.com/cli/) the AWS CLI to your local machine\.

1. Run the following command to run the AWS\-UpdateLinuxAmi document and run the Automation workflow\. In the parameters section, In the parameters section, specify your Automation role, an AMI source ID, and an Amazon EC2 instance profile role\.

   ```
   aws ssm start-automation-execution \
   --document-name "AWS-UpdateLinuxAmi" \
   --parameters \
   SourceAmiId=ami-e6d5d2f1
   ```

   The command returns an execution ID\. Copy this ID to the clipboard\. You will use this ID to view the status of the workflow\.

   ```
   {
       "AutomationExecutionId": "ID"
   }
   ```

1. To view the workflow execution using the CLI, run the following command:

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