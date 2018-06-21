# Task 1: Configure User Access for Systems Manager<a name="sysman-access-user"></a>

If your IAM user account, group, or role is assigned administrator permissions, then you have access to Systems Manager\. You can skip this task\. If you don't have administrator permissions, then an administrator must update your IAM user account, group, or role to include the following permissions:
+ **To access Resource Groups**: You must add the `resource-groups:*` permissions entity to your IAM user account, group, or role\. For more information, see [Setting Up Permissions](https://docs.aws.amazon.com/ARG/latest/userguide/gettingstarted-prereqs.html#rg-permissions) in the *AWS Resource Groups* user guide\.
+ **To access Insights**: You must add the following managed policies to your user account, group, or role:
  + AWSHealthFullAccess
  + AWSConfigUserAccess
  + CloudWatchReadOnlyAccess
**Note**  
Access to Inventory and Compliance are covered by the next policies\.
+ **To access Actions and Shared Resources**: You must add either the AmazonSSMFullAccess policy or the AmazonSSMReadOnlyAccess policy\.

For information about how to change permissions for an IAM user account, group, or role, see [Changing Permissions for an IAM User](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html) in the *IAM User Guide*\.