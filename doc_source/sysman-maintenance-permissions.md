# Controlling Access to Maintenance Windows<a name="sysman-maintenance-permissions"></a>

Before users in your account can create and schedule Maintenance Window tasks, they must be granted the necessary permissions\. To grant these permissions to users, an administrator must perform these two tasks:

**Task 1: Configure instance permissions**  
Provide the Maintenance Window service with the AWS Identity and Access Management \(IAM\) permissions needed to run Maintenance Window tasks on your instances by doing one of the following: 
+ Create a custom service role for Maintenance Window tasks
+ Create a service\-linked role for Systems Manager

You specify one of these roles as part of the configuration when you create a Maintenance Window task\. This allows Systems Manager to run tasks in Maintenance Windows on your behalf\.

**Note**  
A service\-linked role for Systems Manager might already have been created in your account\. Currently, the service\-linked role also provides permissions for the Inventory capability\.

To help you decide whether to use a custom service role or the Systems Manager service\-linked role with a Maintenance Window task, see [Should I Use a Service\-Linked Role or a Custom Service Role to Run Maintenance Window Tasks?](#maintenance-window-tasks-service-role)\.

**Task 2: Configure user permissions**  
Granting `iam:PassRole` permissions to the users in your account who assigns tasks to Maintenance Windows\. This allows them to pass the role to the Maintenance Window service\. Without this explicit permission, a user can't assign tasks to a Maintenance Window\. 

## Should I Use a Service\-Linked Role or a Custom Service Role to Run Maintenance Window Tasks?<a name="maintenance-window-tasks-service-role"></a>

To run maintenance tasks on your target instances, the Maintenance Window service must have permission to access and run tasks on your instances\. You can provide this permission by specifying either the Systems Manager service\-linked role or a custom service role as part of a task configuration\.

The type of role you should choose depends on two factors:

**Custom service role**: Use a custom service role for Maintenance Window tasks in these cases:
+ If you want to use Amazon Simple Notification Service \(Amazon SNS\) to send notifications related to Maintenance Window tasks run through Run Command\. You can enable SNS notifications when you create a Maintenance Window task\.
+ If you want to use a more restrictive set of permissions than those provided by the service\-linked role\. The service\-linked role supports very limited resource\-level constraints\. For example, say you want to allow Maintenance Window tasks to run on a limited set of instances, or you want to allow only certain SSM documents run on your target instances\. In these cases, you specify stricter permissions in a custom service role\.

**Systems Manager service\-linked role**: We recommend that you use a Systems Manager service\-linked role in all other cases\.

For more information about the Systems Manager service\-linked role, see [Service\-Linked Role Permissions for Systems Manager](using-service-linked-roles.md#slr-permissions)\.

**Topics**
+ [Should I Use a Service\-Linked Role or a Custom Service Role to Run Maintenance Window Tasks?](#maintenance-window-tasks-service-role)
+ [Control Access to Maintenance Windows \(Console\)](sysman-maintenance-perm-console.md)
+ [Control Access to Maintenance Windows \(AWS CLI\)](sysman-maintenance-perm-cli.md)
+ [Control Access to Maintenance Windows \(Tools for Windows PowerShell\)](sysman-maintenance-perm-ps.md)
+ [Troubleshooting IAM Maintenance Window Permissions](maintenance-window-role-troubleshooting.md)