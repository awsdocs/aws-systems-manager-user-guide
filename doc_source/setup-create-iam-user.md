# Step 3: Create Non\-Admin IAM Users and Groups for Systems Manager<a name="setup-create-iam-user"></a>

Users in the administrators group for an account have access to all AWS services and resources in that account\. This section describes how to create users with permissions that are limited to AWS Systems Manager\.

The following Systems Manager capabilities may have additional or alternative procedures for granting user access:
+ **Session Manager** \- See [Control User Session Access to Instances](session-manager-getting-started-restrict-access.md)\.
+ **Distributor** \- See [Control User Access to Packages](distributor-getting-started-restrict-access.md)\.
+ **Maintenance Windows** \- See [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md) \(see the instructions for assigning the IAM PassRole policy to an IAM user or group\)\.

For more information about using IAM policies to control user access to Systems Manager capabilities and resources, see [Using Identity\-based Policies \(IAM Policies\) for AWS Systems Manager](auth-and-access-control-iam-identity-based-access-control.md)\.

For information about how to change permissions for an IAM user account, group, or role, see [Changing Permissions for an IAM User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html) in the *IAM User Guide*\.

**Topics**
+ [Task 1: Create Policies for Tag Editor and Resource Groups](setup-create-users-nonadmin-policies.md)
+ [Task 2: Create User Groups](setup-create-users-nonadmin-groups.md)
+ [Task 3: Create Users and Assign Permissions](setup-create-users-nonadmin-users.md)