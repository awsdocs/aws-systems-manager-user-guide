# Controlling Access to Maintenance Windows<a name="sysman-maintenance-permissions"></a>

Before users in your account can create and schedule Maintenance Window tasks, they must be granted the necessary permissions\. The process of granting these permissions consists of two tasks:

1. Task 1: Providing the Maintenance Window service with the AWS Identity and Access Management \(IAM\) permissions needed to run Maintenance Window tasks on your instances\. You do this by creating a custom service role for Maintenance Window tasks, and then specifying this role as part of the configuration when you create a Maintenance Window task\. 

1. Task 2: Granting `iam:PassRole` permissions to the users in your account who will assign tasks to Maintenance Windows\. This allows them to pass the role to the Maintenance Window service\. Without this explicit permission, a user can't assign tasks to a Maintenance Window\. 

**Topics**
+ [Control Access to Maintenance Windows \(Console\)](sysman-maintenance-perm-console.md)
+ [Control Access to Maintenance Windows \(AWS CLI\)](sysman-maintenance-perm-cli.md)
+ [Control Access to Maintenance Windows \(Tools for Windows PowerShell\)](sysman-maintenance-perm-ps.md)
+ [Troubleshooting IAM Maintenance Window Permissions](maintenance-window-role-troubleshooting.md)