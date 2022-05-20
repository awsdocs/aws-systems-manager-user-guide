# Turn on run as support for Linux and macOS managed nodes<a name="session-preferences-run-as"></a>

By default, sessions are launched using the credentials of a system\-generated `ssm-user` account that is created on a managed node\. \(On Linux and macOS machines, this account is added to `/etc/sudoers/`\.\) You can instead launch sessions using the credentials of an operating system \(OS\) account\. The `root` account is not supported\. OS level and directory policies, like log in restrictions or system resource usage restrictions, might not apply to the OS account since sessions are authenticated by AWS Identity and Access Management \(IAM\) and run within the context of the OS account that you specify\. AWS Systems Manager Session Manager verifies the OS account that you specify exists before starting a session\. If you specify an OS account that doesn't exist, the session fails to start\.

**How it works**  
If you turn on Run As support for sessions, the system checks for access permissions as follows:

1. For the user who is starting the session, has their IAM user account or role been tagged with `SSMSessionRunAs = os user account name`?

   If Yes, does the user name exist on the managed node? If it does, start the session\. If it doesn't, don't allow a session to start\.

   If the IAM user's account or role has *not* been tagged with `SSMSessionRunAs = os user account name`, continue to step 2\.

1. If the IAM user's account or role hasn't been tagged with `SSMSessionRunAs = os user account name`, has an OS user name been specified in the AWS account's Session Manager preferences?

   If Yes, does the user name exist on the managed node? If it does, start the session\. If it doesn't, don't allow a session to start\. 

   At this point, Session Manager doesn't fall back on the default `ssm-user` account\. In other words, allowing Run As support prevents sessions from being started using an `ssm-user` account on a managed node\.

If you complete the following procedure without specifying an OS user account, or tagging an IAM user or role, sessions fail\.

**To turn on Run As support for Linux and macOS managed nodes**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Session Manager**\.

1. Choose the **Preferences** tab, and then choose **Edit**\.

1. Select the check box next to **Enable Run As support for Linux instances**\.

1. Do one of the following:
   + **Option 1**: In the **Operating system user name** field, enter the name of the OS user account on the target managed node that you want to use to start sessions\. Using this option, all sessions are run by the same OS user for all the IAM users in your account who connect to the managed node using Session Manager\.
   + **Option 2** \(Recommended\): Choose the **IAM console** link\. In the navigation pane, choose either **Users** or **Roles**\. Choose the entity \(user or role\) to add tags to, and then choose the **Tags** tab\. Enter `SSMSessionRunAs` for the key name\. Enter the name of a user account on your target managed node for the key value\. Choose **Save changes**\.

     Using this option, you could specify a different OS account name for each IAM user or role you tag, or use the same OS user name for them all\. For more information about tagging IAM resources, see [Tagging IAM resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_tags.html) in the *IAM User Guide*

     The following is an example\.  
![\[Screenshot of specifying tags for Session Manager Run As permission. Key = SSMSessionRunAs,Value=My-OS-User-Name\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/ssn-run-as-tags.png)

1. Choose **Save**\.