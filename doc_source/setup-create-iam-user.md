# Step 3: Create non\-Admin IAM users and groups for Systems Manager<a name="setup-create-iam-user"></a>

Users in the administrators group for an account have access to all AWS services and resources in that account\. This section describes how to create users with permissions that are limited to AWS Systems Manager\.

**Note**  
You can grant users or groups full Systems Manager access using the AWS Identity and Access Management \(IAM\) policy `AmazonSSMFullAccess`, as described later in this section\. In practice, however, you might want to limit users or groups to only some Systems Manager features\. In the chapters for many Systems Manager capabilities, such as Session Manager and Maintenance Windows, we provide instructions for limiting access to actions and resources for that capability only\.  
For information about using IAM policies to control user access to Systems Manager capabilities and resources, see [AWS Systems Manager identity\-based policy examples](security_iam_id-based-policy-examples.md)\.  
For information about how to change permissions for an IAM user account, group, or role, see [Changing Permissions for an IAM User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html) in the *IAM User Guide*\.

**Topics**
+ [Task 1: Create user groups](setup-create-users-nonadmin-groups.md)
+ [Task 2: Create users and assign permissions](setup-create-users-nonadmin-users.md)