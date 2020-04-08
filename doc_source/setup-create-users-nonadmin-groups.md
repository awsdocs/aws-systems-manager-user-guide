# Task 2: Create user groups<a name="setup-create-users-nonadmin-groups"></a>

You can create a user group for each policy and assign users to a group rather than attaching individual policies to each user\.

You can create multiple user groups with different permission sets by omitting recommended or optional policies\. You can also create custom IAM policies to grant any combination of permissions for a user\. For example, you can grant a user group permission to use only the Session Manager capability in Systems Manager, as described in [Control user session access to instances](session-manager-getting-started-restrict-access.md)\.

For additional examples of custom IAM policies for Systems Manager, see [Customer managed policy examples](security_iam_id-based-policy-examples.md#customer-managed-policies)\.

For comprehensive information about using IAM policies for Systems Manager access, see [Identity and access management for AWS Systems Manager](security-iam.md)\.

**To create a user group**

Use the following procedure to create a user group for your Systems Manager users\. You can repeat this procedure to create additional user groups with different sets of permissions\.

1. In the navigation pane of the IAM console, choose **Groups**, and then choose **Create New Group**\. 

1. On the **Set Group Name** page, enter a name for the group, such as **SSMUserGroup** or another name that you prefer\.

1. Choose **Next Step**\.

1. On the **Attach Policy** page, for **Filter**, enter **SSM**\.

1. In the policy list, do the following: 
   + If you want to provide users with permission to use Resource Groups and the Tag Editor, choose the **SSMTagEditorAndResourceGroupAccess** policy that you created in the procedure [Task 1: Create policies for Tag Editor and Resource Groups](setup-create-users-nonadmin-policies.md)\. If you gave the policy a different name, choose that name instead\.
   + To provide users in this group with full access to the Systems Manager console, select the box next to `AmazonSSMFullAccess`\.

     \-or\-

     If you want users in this group only to view Systems Manager data, and not create or update resources, select the box beside `AmazonSSMReadOnlyAccess`\.
   + To provide users with access to the **Built\-In Insights** and **Dashboard by CloudWatch** pages in the Systems Manager console, select the boxes next to these managed policies: 
     + **AWSHealthFullAccess**

       This policy grants full access to the AWS Health APIs and Notifications and the Personal Health Dashboard\. It also provides access to portions of the Built\-In Insights Dashboard in the Systems Manager console\.
     + **AWSConfigUserAccess**

       This policy provides read\-only access to use AWS Config, including searching by tags on resources, and reading all tags\. It also provides access to portions of the Built\-In Insights Dashboard in the Systems Manager console\.
     + **CloudWatchReadOnlyAccess**

       This policy provides read\-only access to CloudWatch, which is needed to view information on the **Dashboard by CloudWatch** in the Systems Manager console\.
   + Add any other policies that provide permissions you want to grant to this user group\.

1. Choose **Next Step**\.

1. On the **Review** page, verify that the correct policies are added to this group, and then choose **Create Group**\.

Continue to [Task 3: Create users and assign permissions](setup-create-users-nonadmin-users.md)\.