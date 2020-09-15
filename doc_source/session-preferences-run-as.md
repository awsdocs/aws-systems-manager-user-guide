# Enable run as support for Linux instances<a name="session-preferences-run-as"></a>

By default, sessions are launched using the credentials of a system\-generated `ssm-user` account that is created on a managed instance\. \(On Linux machines, this account is added to `/etc/sudoers/`\.\) You can instead launch sessions using the credentials of an operating system account\. Only the operating system account's credentials are used to start the session\. Other operating system account settings, like shell profiles, are not applied to your sessions\. Session Manager provides two methods for specifying the operating system account to use\.

**Method 1: Tag an IAM user or role \(recommended\)**  
You can specify the operating system user account that is used to start sessions by tagging an IAM user or associated role with the AWS\-provided key name `SSMSessionRunAs`, and specifying the OS user name as its value\. For example, if the OS user account name is `DevRoleLogin`, the corresponding tag to use is `SSMSessionRunAs = DevRoleLogin`\.

Using this method, you could specify a different OS account name for each IAM user or role you tag, or use the same OS user name for them all\.

For more information about tagging IAM entities, see the following topics:
+ [Tagging IAM Entities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html) in the *IAM User Guide*
+ [Add Tags to Manage Your AWS IAM Users and Roles](http://aws.amazon.com/blogs/security/add-tags-to-manage-your-aws-iam-users-and-roles/) on the *AWS Security Blog*

**Method 2: Specify an OS user name in Session Manager preferences**  
When you configure Session Manager preferences in the console or by using the AWS CLI, you can specify the operating system user name to start sessions with\. 

Using this method, all sessions are run by the same OS user for all the IAM users in your account who connect to the instance using Session Manager\.

**How It Works**  
If you enable Run As support for sessions, the system checks for access permissions as follows:

1. For the user who is starting the session, has their IAM user account or role been tagged with `SSMSessionRunAs = os-user-account-name`?

   If Yes, does the user name exist on the instance? If it does, start the session\. If it does not, do not allow a session to start\.

   If the IAM user's account or role has *not* been tagged with `SSMSessionRunAs = os-user-account-name`, continue to step 2\.

1. If the IAM user's account or role hasn't been tagged with `SSMSessionRunAs = os-user-account-name`, has an OS user name been specified in the AWS account's Session Manager preferences?

   If Yes, does the user name exist on the instance? If it does, start the session\. If it does not, do not allow a session to start\. 

   At this point, Session Manager does not fall back on the default `ssm-user` account\. In other words, enabling Run As support prevents sessions from being started using an `ssm-user` account on an instance\.

**To enable run as support for Linux instances**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Session Manager**\.

1. Choose the **Preferences** tab, and then choose **Edit**\.

1. Select the check box next to **Enable Run As support for Linux instances**\.

1. Do one of the following:
   + **Option 1**: For **\(Optional\) Enter an operating system user name for starting sessions**, enter the name of the operating system user account on the target instance that you want to use to start sessions\.
   + **Option 2**: Choose the **IAM console** link\. In the navigation pane, choose either **Users** or **Roles**\. Choose the entity \(user or role\) to add tags to, and then choose the **Tags** tab\. Enter `SSMSessionRunAs` for the key name\. Enter the name of a user account on your target instance for the key value\. Choose **Save changes**\. 

     The following is an example\.  
![\[Illustration of specifying tags for Session Manager Run As permission. Key = SSMSessionRunAs,Value=My-OS-User-Name\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/ssn-run-as-tags.png)

1. Choose **Save**\.