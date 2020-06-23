# Getting started with Automation<a name="automation-setup"></a>

To set up Automation, you must verify user access to the Automation service and situationally configure roles so that the service can perform actions on your resources\. To ensure proper access to Systems Manager Automation, review the following user and service role requirements\.

## Verifying user access for Automation workflows<a name="automation-setup-user-access"></a>

Verify that you have permission to run Automation workflows\. If your AWS Identity and Access Management \(IAM\) user account, group, or role is assigned administrator permissions, then you have access to Systems Manager Automation\. If you don't have administrator permissions, then an administrator must give you permission by assigning the `AmazonSSMFullAccess` managed policy, or a policy that provides comparable permissions, to your IAM account, group, or role\.

**Important**  
The IAM policy `AmazonSSMFullAccess` grants permissions to Systems Manager actions\. However, some Automation documents require permissions to other services, such as the document `AWS-ReleaseElasticIP`, which requires IAM permissions for `ec2:ReleaseAddress`\. Therefore, you must review the actions taken in an Automation document to ensure your IAM user account, group, or role is assigned the necessary permissions to perform the actions included in the document\.

## Configuring a service role \(assume role\) access for Automation workflows<a name="automation-setup-configure-role"></a>

Automation workflows can be initiated under the context of a service role \(or *assume role*\)\. This allows the service to perform actions on your behalf\. If you do not specify an assume role, Automation uses the context of the user who invoked the execution\.

However, the following situations require that you specify a service role for Automation:
+ When you want to restrict a user's privileges on a resource, but you want the user to run an Automation workflow that requires elevated privileges\. In this scenario, you can create a service role with elevated privileges and allow the user to run the workflow\.
+ When you create a State Manager Association that runs an Automation workflow\.
+ When you have operations that you expect to run longer than 12 hours\.
+ When you are running an Automation document not owned by Amazon that uses the `aws:executeScript` action to call an AWS API operation or to act on an AWS resource\. For information, see [Permissions for running Automation executions](automation-document-script.md#execution-permissions)\.

If you need to create a service role for Automation, you can use one of the following methods\.

**Topics**
+ [Verifying user access for Automation workflows](#automation-setup-user-access)
+ [Configuring a service role \(assume role\) access for Automation workflows](#automation-setup-configure-role)
+ [Method 1: Use AWS CloudFormation to configure a service role for Automation](automation-cf.md)
+ [Method 2: Use IAM to configure roles for Automation](automation-permissions.md)