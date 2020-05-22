# Step 3: Create non\-Admin IAM users and groups for Systems Manager<a name="setup-create-iam-user"></a>

Users in the administrators group for an account have access to all AWS services and resources in that account\. This section describes how to create users with permissions that are limited to AWS Systems Manager\.

The following Systems Manager capabilities may have additional or alternative procedures for granting user access:
+ **Session Manager** \- See [Control user session access to instances](session-manager-getting-started-restrict-access.md)\.
+ **Distributor** \- See [Control User Access to Packages](distributor-getting-started-restrict-access.md)\.
+ **Maintenance Windows** \- See [Controlling access to maintenance windows](sysman-maintenance-permissions.md) \(see the instructions for assigning the IAM PassRole policy to an IAM user or group\)\.

For more information about using IAM policies to control user access to Systems Manager capabilities and resources, see [AWS Systems Manager identity\-based policy examples](security_iam_id-based-policy-examples.md)\.

For information about how to change permissions for an IAM user account, group, or role, see [Changing Permissions for an IAM User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html) in the *IAM User Guide*\.

**Topics**
+ [Task 1: Create user groups](setup-create-users-nonadmin-groups.md)
+ [Task 2: Create users and assign permissions](setup-create-users-nonadmin-users.md)