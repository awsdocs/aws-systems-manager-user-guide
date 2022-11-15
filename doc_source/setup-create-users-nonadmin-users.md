# Task 2: Create users and assign permissions<a name="setup-create-users-nonadmin-users"></a>

Create AWS Identity and Access Management \(IAM\) users for the individuals who require access to AWS Systems Manager\. Then, add each user to the appropriate user group to ensure that they have the right level of permissions\. 

**Note**  
If your organization has an existing identity system, you might want to create a single sign\-on \(SSO\) option\. SSO gives users access to the AWS Management Console for your account without requiring them to have an IAM user identity\. SSO also eliminates the need for users to sign in to your organization's site and to AWS separately\. For more information, see [Enabling custom identity broker access to the AWS Management Console](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_enable-console-custom-url.html)\.

Depending on whether the user accounts for this group were already created, use one of the following procedures:

**To create users and assign permissions**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**, and then choose **Add users**\.

1. For **User name**, enter the name that the user will use to sign in to AWS Systems Manager\.

1. To allow the user access to the AWS API, AWS CLI, AWS SDK, and other development tools, select the check box next to **Access key \- Programmatic access**\.

   This creates an access key for the new user\. You can view or download the access keys when you get to the **Final** page\. 

1. To allow the user access to the AWS Management Console, select the check box next to **Password \- AWS Management Console access**\. 

   The AWS Management Console provides a web interface where you can manage your compute, storage, and other cloud resources\. Within the AWS Management Console, individual services have their own console\. For example, you can manage your compute resources using the Amazon EC2 console and storage through the Amazon S3 console\.

   If you choose **Custom password**, enter an initial password for the user\. Optionally, you can select **Require password reset** to force the user to create a new password the next time the user signs in\.

1. Choose **Next: Permissions**\.

1. For **Set permissions**, ensure the default **Add user to group** is selected\.

1. For **Add user to group**, select the boxes of the user groups to add the user to, and then choose **Next: Tags**\.

1. \(Optional\) Add one or more tag key\-value pairs to organize, track, or control access for this user\. Then choose **Next: Review** to see the list of group memberships that the new user is joining\. 

1. Choose **Create user**\.

1. To view the users' access keys \(access key IDs and secret access keys\), choose **Show** next to each password and access key that you want to see\. To save the access keys, choose `Download .csv` and then save the file to a safe location\.
**Important**  
This is your only opportunity to view or download the secret access keys\. Your users need this information before they can use the AWS API or AWS CLI\. Save the user's new access key ID and secret access key in a safe and secure place\. **You won't have access to the secret keys again after this step\.**

1. Provide each user with his or her credentials\. On the final page you can choose **Send email** next to each user\. Your local mail client opens with a draft that you can customize and send\. The email message template includes the following details for each user:
   + User name
   + URL of the account sign\-in page\. Use the following example, substituting the correct account ID number or account alias\.

     ```
     https://account-ID or alias.signin.aws.amazon.com/console
     ```

   For more information, see [How IAM users sign in to AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_sign-in.html) in the *IAM User Guide*\.
**Important**  
The user's password isn't included in the generated email message\. Provide the password to the user in a way that complies with your organization's security guidelines\.

**To add permissions for an existing user**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**\.

1. Select the name of the user to add to a group\. On the **Summary** page, choose **Add permissions**\.

1. For **Grant permissions**, ensure the default **Add user to group** is selected\.

1. For **Add user to group**, select the check box next to the group to add the user to, such as **SSMUserGroup**, or the name of a different user group that you created\.

1. Add any other available permission policies to assign to the user\.

1. Choose **Next: Review** to see the list of group memberships that will be added to the new user\.

1. Choose **Add permissions**\.

Continue to [Step 3: Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\.