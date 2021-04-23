# Configuring Change Manager options and best practices<a name="change-manager-account-setup"></a>

The tasks in this section must be performed whether you are using Change Manager, a capability of AWS Systems Manager, across an organization or in a single account\.

If you are using Change Manager for an organization, you can perform the following tasks in either your delegated administrator account or any account in an organization unit that you have enabled for use with Change Manager\.

**Topics**
+ [Task 1: Configuring Change Manager user identity management and template reviewers](#cm-configure-account-task-1)
+ [Task 2: Configuring Change Manager change freeze event approvers and best practices](#cm-configure-account-task-2)
+ [Configuring Amazon SNS topics for Change Manager notifications](change-manager-sns-setup.md)

## Task 1: Configuring Change Manager user identity management and template reviewers<a name="cm-configure-account-task-1"></a>

Perform the task in this procedure the first time you access Change Manager\. You can update these configuration settings later by returning to Change Manager and choosing **Edit** on the **Settings** tab\.

**To configure Change Manager user identity management and template reviewers**

1. Sign in to the AWS Management Console\.

   If you are using Change Manager for an organization, sign in using your credentials for your delegated administrator account\. Note that the user account you use must have the necessary AWS Identity and Access Management \(IAM\) permissions for making updates to your Change Manager settings\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Change Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Change Manager**\.

1. On the service home page, do one of the following:
   + If you are using Change Manager with AWS Organizations , choose **Set up delegated account**\.
   + If you are using Change Manager with a single AWS account, choose **Set up Change Manager**\.

1. For **User identity management**, choose one of the following\.
   + **AWS Identity and Access Management \(IAM\)** – Identify the users who make and approve requests and perform other action in Change Manager by using your existing IAM user accounts and groups\.
   + **AWS Single Sign\-On \(AWS SSO\)** – Enable [AWS SSO](https://docs.aws.amazon.com/singlesignon/latest/userguide/) to create and manage identities, or connect to your existing identity source to identify the users who perform actions in Change Manager\.

1. In the **Template reviewer notification** section, specify the Amazon Simple Notification Service \(Amazon SNS\) topics to use to notify template reviewers that a new change template or change template version is ready for review\. Ensure that the Amazon SNS topic you choose is configured to send notifications to your template reviewers\. 

   For information about creating and configuring Amazon SNS topics for change template reviewer notifications, see [Configuring Amazon SNS topics for Change Manager notifications](change-manager-sns-setup.md)\.

   1. To specify the Amazon SNS topic for template reviewer notification, choose one of the following:
      + **Enter an SNS Amazon Resource Name \(ARN\)** – For **Topic ARN**, enter the ARN of an existing Amazon SNS topic\. This topic can be in any of your organization's accounts\.
      + **Select an existing SNS topic** – For **Target notification topic**, select the ARN of an existing Amazon SNS topic in your current account\. \(This option is not available if you haven't yet created any Amazon SNS topics in your current account and AWS Region\.\)
**Note**  
The Amazon SNS topic you select must be configured to specify the notifications it sends and the subscribers they are sent to\. Its access policy must also grant permissions to Systems Manager so Change Manager can send notifications\. For information, see [Configuring Amazon SNS topics for Change Manager notifications](change-manager-sns-setup.md)\. 

   1. Choose **Add notification**\.

1. In the **Change template reviewers** section, select the users in your organization or account to review new change templates or change template versions before they can be used in your operations\. 

   Change template reviewers are responsible for verifying the suitability and security of templates other users have submitted for use in Change Manager runbook workflows\.

   Select change template reviewers by doing the following:

   1. Choose **Add**\.

   1. Select the check box next to the name of each user or group you want to assign as a change template reviewer\.

   1. Choose **Add approvers**\.

1. Choose **Submit**\.

 After you complete this initial setup process, configure additional Change Manager settings and best practices by following the steps in [Task 2: Configuring Change Manager change freeze event approvers and best practices](#cm-configure-account-task-2)\.

## Task 2: Configuring Change Manager change freeze event approvers and best practices<a name="cm-configure-account-task-2"></a>

After you complete the steps in [Task 1: Configuring Change Manager user identity management and template reviewers](#cm-configure-account-task-1), you can designate extra reviewers for change requests during *change freeze events* and specify which available best practices you want to enable for your Change Manager operations\.

A change freeze event means that restrictions are in place in the current change calendar \(the calendar state in AWS Systems Manager Change Calendar is CLOSED\)\. In these cases, in addition to regular approvers for change requests, change freeze approvers must also grant permission for this change request to run\. If they do not, the change will not be processed until the calendar state is again OPEN\.

**To configure Change Manager change freeze event approvers and best practices**

1. In the navigation pane, choose **Change Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Change Manager**\.

1. Choose the **Settings** tab, and then choose **Edit**\.

1. In the **Approvers for change freeze events** section, select the users in your organization or account who can approve changes to run even when the calendar in use in Change Calendar is currently CLOSED\.
**Note**  
To enable change freeze reviews, you must enable the **Check Change Calendar for restricted change events** option in **Best practices**\.

   Select approvers for change freeze events by doing the following:

   1. Choose **Add**\.

   1. Select the check box next to the name of each user or group you want to assign as an approver for change freeze events\.

   1. Choose **Add approvers**\.

1. In the **Best practices** section near the bottom of the page, enable the best practices you want to enforce for each of the following options\.
   + Option: **Check Change Calendar for restricted change events**

     To specify that Change Manager checks a calendar in Change Calendar to make sure changes are not blocked by scheduled events, first select the **Enabled** check box, and then select the calendar to check for restricted events from the **Change Calendar** list\.

     For more information about Change Calendar, see [AWS Systems Manager Change Calendar](systems-manager-change-calendar.md)\.
   + Option: **SNS topic for approvers for closed events**

     1. Choose one of the following to specify the Amazon Simple Notification Service \(Amazon SNS\) topic in your account to use for sending notifications to approvers during change freeze events\. \(Note that you must also specify approvers in the **Approvers for change freeze events** section above **Best practices**\.\)
        + **Enter an SNS Amazon Resource Name \(ARN\)** – For **Topic ARN**, enter the ARN of an existing Amazon SNS topic\. This topic can be in any of your organization's accounts\.
        + **Select an existing SNS topic** – For **Target notification topic**, select the ARN of an existing Amazon SNS topic in your current account\. \(This option is not available if you haven't yet created any Amazon SNS topics in your current account and AWS Region\.\)
**Note**  
The Amazon SNS topic you select must be configured to specify the notifications it sends and the subscribers they are sent to\. Its access policy must also grant permissions to Systems Manager so Change Manager can send notifications\. For information, see [Configuring Amazon SNS topics for Change Manager notifications](change-manager-sns-setup.md)\. 

     1. Choose **Add notification**\.
   + Option: **Require monitors for all templates**

     If you want to ensure that all templates for your organization or account specify an Amazon CloudWatch alarm to monitor your change operation, select the **Enabled** check box\.
   + Option: **Require template review and approval before use**

     To ensure that no change requests are created, and no runbook workflows run, without being based on a template that has been reviewed and approved, select the **Enabled** check box\.

1. Choose **Save**\.