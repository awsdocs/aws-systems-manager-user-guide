# Controlling Access to Maintenance Windows<a name="sysman-maintenance-permissions"></a>

Before users in your account can create and schedule maintenance window tasks, they must be granted the necessary permissions\. To grant these permissions to users, an administrator must perform these two tasks:

**Task 1: Configure instance permissions**  
Provide the Maintenance Windows service with the AWS Identity and Access Management \(IAM\) permissions needed to run maintenance window tasks on your instances by doing one of the following: 
+ Create a custom service role for maintenance window tasks
+ Create a service\-linked role for Systems Manager

You specify one of these roles as part of the configuration when you create a maintenance window task\. This allows Systems Manager to run tasks in maintenance windows on your behalf\.

**Note**  
A service\-linked role for Systems Manager might already have been created in your account\. Currently, the service\-linked role also provides permissions for the Inventory capability\.

To help you decide whether to use a custom service role or the Systems Manager service\-linked role with a maintenance window task, see [Should I Use a Service\-Linked Role or a Custom Service Role to Run Maintenance Window Tasks?](#maintenance-window-tasks-service-role)\.

**Task 2: Configure user permissions**  
Granting `iam:PassRole` permissions to the users in your account who assigns tasks to maintenance windows\. This allows them to pass the role to the maintenance window service\. Without this explicit permission, a user can't assign tasks to a maintenance window\. 

## Should I Use a Service\-Linked Role or a Custom Service Role to Run Maintenance Window Tasks?<a name="maintenance-window-tasks-service-role"></a>

To run maintenance tasks on your target instances, the Maintenance Windows service must have permission to access and run tasks on your instances\. You can provide this permission by specifying either the Systems Manager service\-linked role or a custom service role as part of a task configuration\.

The type of role you should choose depends on the following factors:

**Custom service role**: Use a custom service role for maintenance window tasks in these cases:
+ If you want to use a more restrictive set of permissions than those provided by the service\-linked role\. The service\-linked role supports very limited resource\-level constraints\. For example, say you want to allow maintenance window tasks to run on a limited set of instances, or you want to allow only certain SSM documents run on your target instances\. In these cases, you specify stricter permissions in a custom service role\.
+ If you need a more permissive or expanded set of permissions than those provided by the service\-linked role\. Some actions in Automation documents require expanded permissions\.

  For example, some Automation actions work with AWS CloudFormation stacks\. Therefore, the permissions `cloudformation:CreateStack`, `cloudformation:DescribeStack`, and `cloudformation:DeleteStack` are required\. 

  Another example: the Automation document `AWS-CopySnapshot` requires permission to create an Amazon Elastic Block Store \(Amazon EBS\) snapshot, and so the service role needs the permission `ec2:CreateSnapshot`\. This permission isn't included in the service\-linked role for Systems Manager\. 

  For information about the role permissions needed by Automation documents, see the document descriptions in [Systems Manager Automation Document Details Reference](automation-documents-reference-details.md)\.

**Systems Manager service\-linked role**: We recommend that you use a Systems Manager service\-linked role in all other cases\.

For more information about the Systems Manager service\-linked role, see [Using Service\-Linked Roles for Systems Manager](using-service-linked-roles.md)\.

**Topics**
+ [Should I Use a Service\-Linked Role or a Custom Service Role to Run Maintenance Window Tasks?](#maintenance-window-tasks-service-role)
+ [Control Access to Maintenance Windows \(Console\)](sysman-maintenance-perm-console.md)
+ [Control Access to Maintenance Windows \(AWS CLI\)](sysman-maintenance-perm-cli.md)
+ [Control Access to Maintenance Windows \(Tools for Windows PowerShell\)](sysman-maintenance-perm-ps.md)
+ [Troubleshooting IAM Maintenance Window Permissions](maintenance-window-role-troubleshooting.md)