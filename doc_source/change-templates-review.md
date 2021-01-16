# Reviewing and approving or rejecting change templates<a name="change-templates-review"></a>

If you are specified as a reviewer for change templates in Change Manager, you are notified when a new change template, or new version of a change template, is awaiting your review\. An Amazon Simple Notification Service \(Amazon SNS\) topic sends the notifications\.

**Note**  
This functionality depends on whether your account has been configured to use an Amazon SNS topic to send change template review notifications\. For information about specifying a template reviewer notification topic, see [Task 1: Configuring Change Manager user identity management and template reviewers](change-manager-account-setup.md#cm-configure-account-task-1)\.

To review the change template, follow the link in your notification, sign in to the AWS Management Console, and follow the steps in this procedure\.

**To review and approve or reject a change template**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Change Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Change Manager**\.

1. In the **Change templates** section at the bottom of the **Overview** tab, choose the number in **Pending review**\.

1. In the **Change templates** list, locate and choose the name of change template to review\.

1. In the summary page, review the proposed content of the change template and do one of the following:
   + To approve the change template, which enables it for use in change requests, choose **Actions, Approve template**\.
   + To reject the change template, which prevents it from being used in change requests, choose ** Action, Reject template**\.