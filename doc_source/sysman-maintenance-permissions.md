# Controlling access to maintenance windows<a name="sysman-maintenance-permissions"></a>

Before users in your account can create and schedule maintenance window tasks, they must be granted the necessary permissions\. To grant these permissions to users, an administrator must perform these two tasks:

**Task 1: Configure instance permissions**  
Provide the Maintenance Windows service with the AWS Identity and Access Management \(IAM\) permissions needed to run maintenance window tasks on your instances by doing one of the following: 
+ Create a custom service role for maintenance window tasks
+ Create a service\-linked role for AWS Systems Manager

You specify one of these roles as part of the configuration when you create a maintenance window task\. This allows Systems Manager to run tasks in maintenance windows on your behalf\.

**Note**  
A service\-linked role for Systems Manager might already have been created in your account\. Currently, the service\-linked role also provides permissions for the Inventory capability\.

To help you decide whether to use a custom service role or the Systems Manager service\-linked role with a maintenance window task, see [Should I use a service\-linked role or a custom service role to run maintenance window tasks?](#maintenance-window-tasks-service-role)\.

**Task 2: Configure permissions for users who are allowed to register maintenance window tasks**  
Allow `iam:PassRole` permissions for the users in your account who assign tasks to maintenance windows\. This allows them to pass the role to the maintenance window service\. Without this explicit permission, a user can't assign tasks to a maintenance window when using a custom service role to run maintenance window tasks\. 

**Task 3: Configure permissions for users who are not allowed to register maintenance window tasks**  
Deny `ssm:RegisterTaskWithMaintenanceWindow` permissions for the users in your account who you don't want to register tasks with maintenance windows\. This prevents users from registering a maintenance window task by using the service\-linked role in a maintenance window task registration request\.

**Before you begin**  
In order to complete the tasks in the section, you need one or both of the following resources\.
+ You are assigning permissions to IAM users or groups\. These users or groups should already have been granted general permissions for working with maintenance windows\. This can be done by assigning the IAM policy `AmazonSSMFullAccess` to the users or groups, or by creating and assigning an IAM policy that provides a smaller set of access permissions for Systems Manager that covers maintenance window tasks\. For more information, see [Create user groups](setup-create-users-nonadmin-groups.md) and [Create users and assign permissions](setup-create-users-nonadmin-users.md)\.
+ \(Optional\) For maintenance windows that run Run Command tasks, you can choose for Amazon Simple Notification Service \(Amazon SNS\) status notifications to be sent\. For information about configuring Amazon SNS notifications for Systems Manager, including information about creating an IAM role to use for sending SNS notifications, see [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.

## Should I use a service\-linked role or a custom service role to run maintenance window tasks?<a name="maintenance-window-tasks-service-role"></a>

To run maintenance tasks on your target instances, the Maintenance Windows service must have permission to access and run tasks on your instances\. You can provide this permission by specifying either the Systems Manager service\-linked role or a custom service role as part of a task configuration\.

The type of role you should choose depends on the following factors:

**Custom service role**: Use a custom service role for maintenance window tasks in these cases:
+ If you want to use a more restrictive set of permissions than those provided by the service\-linked role\. The service\-linked role supports very limited resource\-level constraints\. For example, say you want to allow maintenance window tasks to run on a limited set of instances, or you want to allow only certain SSM documents run on your target instances\. In these cases, you specify stricter permissions in a custom service role\.
+ If you need a more permissive or expanded set of permissions than those provided by the service\-linked role\. Some actions in Automation documents require expanded permissions\.

  For example, some Automation actions work with AWS CloudFormation stacks\. Therefore, the permissions `cloudformation:CreateStack`, `cloudformation:DescribeStack`, and `cloudformation:DeleteStack` are required\. 

  Another example: the Automation document `AWS-CopySnapshot` requires permission to create an Amazon Elastic Block Store \(Amazon EBS\) snapshot, and so the service role needs the permission `ec2:CreateSnapshot`\. This permission isn't included in the service\-linked role for Systems Manager\. 

  For information about the role permissions needed by Automation documents, see the document descriptions in [ Automation document details reference](automation-documents-reference-details.md)\.

**Systems Manager service\-linked role**: We recommend that you use a Systems Manager service\-linked role in all other cases\.

For more information about the Systems Manager service\-linked role, see [Using service\-linked roles for Systems Manager](using-service-linked-roles.md)\.

**Topics**
+ [Should I use a service\-linked role or a custom service role to run maintenance window tasks?](#maintenance-window-tasks-service-role)
+ [Control access to maintenance windows \(console\)](sysman-maintenance-perm-console.md)
+ [Control access to maintenance windows \(AWS CLI\)](sysman-maintenance-perm-cli.md)
+ [Control access to maintenance windows \(Tools for Windows PowerShell\)](sysman-maintenance-perm-ps.md)