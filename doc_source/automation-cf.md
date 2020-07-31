# Method 1: Use AWS CloudFormation to configure a service role for Automation<a name="automation-cf"></a>

You can create a service role for Automation from an AWS CloudFormation template\. After you create the service role, you can specify the service role in Automation workflows using the parameter `AutomationAssumeRole`\. For information about how to run an Automation workflow using the Automation service role, see [Running an automation by using an IAM service role](automation-walk-security-assume.md)\.

**Topics**
+ [Create the service role using AWS CloudFormation](#automation-cf-create)
+ [Copy role information for Automation](#automation-cf-copy)

## Create the service role using AWS CloudFormation<a name="automation-cf-create"></a>

Use the following procedure to create the required IAM role for Systems Manager Automation by using AWS CloudFormation\.

**To create the required IAM role**

1. Download the [AWS\-SystemsManager\-AutomationServiceRole\.zip](samples/AWS-SystemsManager-AutomationServiceRole.zip) folder\. This folder includes the `AWS-SystemsManager-AutomationServiceRole.yaml` AWS CloudFormation template file\.

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Choose **Create Stack**\.

1. In the **Specify template** section, choose **Upload a template file**\.

1. Choose **Browse**, and then choose the `AWS-SystemsManager-AutomationServiceRole.yaml` AWS CloudFormation template file\.

1. Choose **Next**\.

1. On the **Specify stack details** page, in the **Stack name** field, enter a name\. 

1. On the **Configure stack options** page, you don’t need to make any selections\. Choose **Next**\.

1. On the **Review** page, scroll down and choose the **I acknowledge that AWS CloudFormation might create IAM resources** option\.

1. Choose **Create**\.

AWS CloudFormation shows the **CREATE\_IN\_PROGRESS** status for approximately three minutes\. The status changes to **CREATE\_COMPLETE** after the stack is created and your roles are ready to use\.

**Important**  
If you run an automation workflow that invokes other services by using an AWS Identity and Access Management \(IAM\) service role, be aware that the service role must be configured with permission to invoke those services\. This requirement applies to all AWS Automation documents \(`AWS-*` documents\) such as the `AWS-ConfigureS3BucketLogging`, `AWS-CreateDynamoDBBackup`, and `AWS-RestartEC2Instance` documents, to name a few\. This requirement also applies to any custom Automation documents you create that invoke other AWS services by using actions that call other services\. For example, if you use the `aws:executeAwsApi`, `aws:createStack`, or `aws:copyImage` actions, then you must configure the service role with permission to invoke those services\. You can enable permissions to other AWS services by adding an IAM inline policy to the role\. For more information, see [\(Optional\) add an Automation inline policy to invoke other AWS services](automation-permissions.md#automation-role-add-inline-policy)\.

## Copy role information for Automation<a name="automation-cf-copy"></a>

Use the following procedure to copy information about the Automation service role from the AWS CloudFormation console\. You must specify these roles when you run an Automation document\.

**Note**  
You do not need to copy role information using this procedure if you run the `AWS-UpdateLinuxAmi` or `AWS-UpdateWindowsAmi` documents\. These documents already have the required roles specified as default values\. The roles specified in these documents use IAM managed policies\. 

**To copy the role names**

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Select the Automation **Stack name** you created in the previous procedure\.

1. Choose the **Resources** tab\.

1. Choose the **Physical ID** link for **AutomationServiceRole**\. The IAM console opens to a summary of the Automation service role\.

1. Copy the Amazon Resource Name \(ARN\) next to **Role ARN**\. The ARN is similar to the following: `arn:aws:iam::12345678:role/AutomationServiceRole`

1. Paste the ARN into a text file to use later\.

You have finished configuring the service role for Automation\. You can now use the Automation service role ARN in your Automation documents\.