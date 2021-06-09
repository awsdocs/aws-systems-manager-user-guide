# Setting up Automation<a name="automation-setup"></a>

To set up Automation, a capability of AWS Systems Manager, you must verify user access to the Automation service and situationally configure roles so that the service can perform actions on your resources\. To ensure proper access to AWS Systems Manager Automation, review the following user and service role requirements\.

## Verifying user access for runbooks<a name="automation-setup-user-access"></a>

Verify that you have permission to use runbooks\. If your AWS Identity and Access Management \(IAM\) user account, group, or role is assigned administrator permissions, then you have access to Systems Manager Automation\. If you don't have administrator permissions, then an administrator must give you permission by assigning the `AmazonSSMFullAccess` managed policy, or a policy that provides comparable permissions, to your IAM account, group, or role\.

**Important**  
The IAM policy `AmazonSSMFullAccess` grants permissions to Systems Manager actions\. However, some runbooks require permissions to other services, such as the runbook `AWS-ReleaseElasticIP`, which requires IAM permissions for `ec2:ReleaseAddress`\. Therefore, you must review the actions taken in a runbook to ensure your IAM user account, group, or role is assigned the necessary permissions to perform the actions included in the runbook\.

## Configuring a service role \(assume role\) access for automations<a name="automation-setup-configure-role"></a>

Automations can be initiated under the context of a service role \(or *assume role*\)\. This allows the service to perform actions on your behalf\. If you don't specify an assume role, Automation uses the context of the user who invoked the automation\.

However, the following situations require that you specify a service role for Automation:
+ When you want to restrict a user's privileges on a resource, but you want the user to run an automation that requires elevated privileges\. In this scenario, you can create a service role with elevated privileges and allow the user to run the automation\.
+ When you create a Systems Manager State Manager association that runs a runbook\.
+ When you have operations that you expect to run longer than 12 hours\.
+ When you're running a runbook not owned by Amazon that uses the `aws:executeScript` action to call an AWS API operation or to act on an AWS resource\. For information, see [Permissions for using runbooks](automation-document-script.md#execution-permissions)\.

If you need to create a service role for Automation, you can use one of the following methods\.

**Topics**
+ [Verifying user access for runbooks](#automation-setup-user-access)
+ [Configuring a service role \(assume role\) access for automations](#automation-setup-configure-role)
+ [Method 1: Use AWS CloudFormation to configure a service role for Automation](automation-cf.md)
+ [Method 2: Use IAM to configure roles for Automation](automation-permissions.md)