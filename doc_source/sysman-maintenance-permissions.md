# Setting up Maintenance Windows<a name="sysman-maintenance-permissions"></a>

Before users in your AWS account can create and schedule maintenance window tasks using Maintenance Windows, a capability of AWS Systems Manager, they must be granted the necessary permissions\.

**Before you begin**  
To complete the tasks in the section, you need one or both of the following resources set up already:
+ You're assigning permissions to IAM users or groups\. These users or groups should already have been granted general permissions for working with maintenance windows\. Do this by ensuring that the IAM policy `AmazonSSMFullAccess` to the users or groups, or another IAM policy that provides a smaller set of access permissions for Systems Manager that covers maintenance window tasks\. For more information, see [Create user groups](setup-create-users-nonadmin-groups.md) and [Create users and assign permissions](setup-create-users-nonadmin-users.md)\.
+ \(Optional\) For maintenance windows that run Run Command tasks, you can choose to send Amazon Simple Notification Service \(Amazon SNS\) status notifications\. Run Command is a capability of Systems Manager\. If you want to use this option, configure the Amazon SNS topic before completing these setup tasks\. For For information about configuring Amazon SNS notifications for Systems Manager, including information about creating an IAM role to use for sending SNS notifications, see [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.

**Overview of setup tasks**  
To grant the permissions that users need to register maintenance windows, an administrator performs the following tasks\. \(Full instructions are provided in [Use the console to configure permissions for maintenance windows](sysman-maintenance-perm-console.md)\)\.

Task 1: Create a policy to use with the custom maintenance window role  
Maintenance window tasks require an IAM role to provide the permissions required to run on the target resources\. The types of tasks you run and your other operational requirements determine the contents of this policy\.  
We provide a base policy you can adapt in the topic [Task 1: Create a policy for your custom maintenance window service role](sysman-maintenance-perm-console.md#sysman-maintenance-role-policy)\.

Task 2: Create a custom service role for maintenance window tasks  
The policy you create in Task 1 is attached to the maintenance window role you create in Task 2\. When users register a maintenance window task, they specify this custom service role as part of the task configuration\. The permissions in this role allow Systems Manager to run tasks in maintenance windows on your behalf\.  
Previously, the Systems Manager console provided you with the ability to choose the AWS managed IAM service\-linked role `AWSServiceRoleForAmazonSSM` to use as the maintenance role for your tasks\. Using this role and its associated policy, `AmazonSSMServiceRolePolicy`, for maintenance window tasks is no longer recommended\. If you're using this role for maintenance window tasks now, we encourage you to stop using it\. Instead, create your own IAM role that enables communication between Systems Manager and other AWS services when your maintenance window tasks run\.

Task 3: Grant permissions to use the service role to users who register maintenance window tasks   
Providing users with permissions to access the custom maintenance window role lets them use it with their maintenance windows tasks\. This is in addition to permissions that you’ve already given them to work with the Systems Manager API commands for Maintenance Windows\. This role conveys the permissions need to run a maintenance window task\. As a result, a user can't assign tasks to a maintenance window using your custom service role without the ability to pass these IAM permissions\.

Task 4: \(Optional\) Explicitly deny permissions for users who aren't allowed to register maintenance window tasks  
You can deny the `ssm:RegisterTaskWithMaintenanceWindow` permission for the users in your AWS account who you don't want to register tasks with maintenance windows\. This provides an extra layer of prevention for users who shouldn’t register maintenance window tasks\.

**Topics**
+ [Use the console to configure permissions for maintenance windows](sysman-maintenance-perm-console.md)