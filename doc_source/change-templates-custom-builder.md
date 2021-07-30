# Creating change templates using Builder<a name="change-templates-custom-builder"></a>

Using the Builder for change templates in Change Manager, a capability of AWS Systems Manager, you can configure the runbook workflow defined in your change template without having to use JSON or YAML syntax\. After you specify your options, the system converts your input into the YAML format that Systems Manager can use to run runbook workflows\.

**To create a change template using Builder**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Change Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Change Manager**\.

1. Choose **Create template**\.

1. For **Name**, enter a name for the template that makes its purpose easy to identify, such as **UpdateEC2LinuxAMI**\.

1. In the **Change template details** section, do the following:
   + For **Description**, provide a brief explanation of how and when the change template you're creating is to be used\. 

     This description helps users who create change requests determine whether they're using the correct change template\. It helps those who review change requests understand whether the request should be approved\.
   + For **Change template type**, specify whether you're creating a standard change template or an emergency change template\.

     An emergency change template is used for situations when a change must be made even if changes are otherwise blocked by an event in the calendar in use by AWS Systems Manager Change Calendar\. Change requests created from an emergency change template must still be approved by its designated approvers, but the requested changes can still run even when the calendar is blocked\.
   + For **Runbook options**, specify the runbooks that users can choose from when creating a change request\. You can add a single runbook or multiple runbooks\. Alternatively, you can allow requesters to specify which runbook to use\. In any of these cases, only one runbook can be included in the change request\.
   + For **Runbook**, select the names of the runbooks and the versions of those runbooks that users can choose from for their change requests\. No matter how many runbooks you add to the change template, only one can be selected per change request\.

     You don't specify a runbook if you chose **Any runbook can be used** earlier\.
**Tip**  
Select a runbook and runbook version, and then choose **View** to examine the contents of the runbook in the Systems Manager Documents interface\.

1. In the **Template information** section, use Markdown to enter information for users who create change requests from this change template\. We have provided a set of questions that you can include for users who create change requests, or you can add other information and questions instead\. 
**Note**  
Markdown is a markup language that allows you to add wiki\-style descriptions to documents and individual steps within the document\. For more information about using Markdown, see [Using Markdown in AWS](https://docs.aws.amazon.com/general/latest/gr/aws-markdown.html)\.

   We recommend providing questions for users to answer about their change requests to help approvers decide whether or not to grant each change request, such as listing any manual steps required to run as part of the change and a rollback plan\. 
**Tip**  
Toggle between **Hide preview** and **Show preview** to see what your content looks like as you compose\.

1. In the **Change request approvals** section, do the following:
   + \(Optional\) If you want to allow change requests that are created from this change template to run automatically, without review by any approvers \(with the exception of change freeze events\), select **Enable auto\-approval**\.
**Note**  
Enabling auto\-approvals in a change template provides users with the *option* of bypassing reviewers\. They can still choose to specify reviewers when creating a change request\. Therefore, you must still specify reviewer options in the change template\.
**Important**  
If you enable auto\-approval for a change template, users can submit change requests using that template that do not require review by reviewers before they run \(with the exception of change freeze event approvers\)\. If you want to restrict a particular user, group, or IAM role from submitting auto\-approval requests, you can use a condition in an IAM policy for this purpose\. For more information, see [Controlling access to auto\-approval runbook workflows](change-manager-auto-approval-access.md)\.
   + To add mandatory first\-level approvers, choose **Add approver**, and then choose from the following:
     + **Template specified approvers** – Choose one or more users, groups, or AWS Identity and Access Management \(IAM\) roles from your account to approve change requests created from this change template\. Any change requests that are created using this template must be reviewed and approved by each approver you specify\.
     + **Request specified approvers** – The user who makes the change request specifies reviewers at the time they make the request and can choose from a list of users in your account\. 

       The number you enter in the **Required** column determines how many reviewers must be specified by a change request that uses this change template\. 
   + For **SNS topic to notify approvers**, do the following:

     1. Choose one of the following to specify the Amazon Simple Notification Service \(Amazon SNS\) topic in your account to use for sending notifications to approvers that a change request is ready for their review:
        + **Enter an SNS Amazon Resource Name \(ARN\)** – For **Topic ARN**, enter the ARN of an existing Amazon SNS topic\. This topic can be in any of your organization's accounts\.
        + **Select an existing SNS topic** – For **Target notification topic**, select the ARN of an existing Amazon SNS topic in your current AWS account\. \(This option isn't available if you haven't yet created any Amazon SNS topics in your current AWS account and AWS Region\.\)
        + **Specify SNS topic when the change request is created **– The user who creates a change request can specify the Amazon SNS topic to use for notifications\.
**Note**  
The Amazon SNS topic you select must be configured to specify the notifications it sends and the subscribers they're sent to\. Its access policy must also grant permissions to Systems Manager so Change Manager can send notifications\. For information, see [Configuring Amazon SNS topics for Change Manager notifications](change-manager-sns-setup.md)\. 

     1. Choose **Add notification**\.

1. \(Optional\) To add an additional level of approvers, choose **Add approval level** and choose between template\-specified approvers and request\-specified approvers for this level\. Then choose an SNS topic to notify this level of approvers\.

   After all approvals have been received by first\-level approvers, second\-level approvers are notified, and so on\.

   You can add a maximum of five levels of approvers in each template\. You might, for example, require approvals from users in technical roles for the first level, then managerial approval for the second level\.

1. In the **Monitoring** section, for **CloudWatch alarm to monitor**, enter the name of an Amazon CloudWatch alarm in the current account to monitor the progress of runbook workflows that are based on this template\. 
**Tip**  
To create a new alarm, or to review the settings of an alarm you want to specify, choose **Open the Amazon CloudWatch console**\. For information about working with CloudWatch alarms, see [Using Amazon CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\.

1. In the **Notifications** section, do the following:

   1. Choose one of the following to specify the Amazon SNS topic in your account to use for sending notifications about change requests that are created using this change template: 
      + **Enter an SNS Amazon Resource Name \(ARN\)** – For **Topic ARN**, enter the ARN of an existing Amazon SNS topic\. This topic can be in any of your organization's accounts\.
      + **Select an existing SNS topic** – For **Target notification topic**, select the ARN of an existing Amazon SNS topic in your current AWS account\. \(This option isn't available if you haven't yet created any Amazon SNS topics in your current AWS account and AWS Region\.\)
**Note**  
The Amazon SNS topic you select must be configured to specify the notifications it sends and the subscribers they're sent to\. Its access policy must also grant permissions to Systems Manager so Change Manager can send notifications\. For information, see [Configuring Amazon SNS topics for Change Manager notifications](change-manager-sns-setup.md)\. 

   1. Choose **Add notification**\.

1. \(Optional\) In the **Tags** section, apply one or more tag key name/value pairs to the change template\.

   Tags are optional metadata that you assign to a resource\. By using tags, you can categorize a resource in different ways, such as by purpose, owner, or environment\. For example, you might want to tag a change template to identify the type of change it makes and the environment it runs in\. In this case, you could specify the following key name/value pairs:
   + `Key=TaskType,Value=InstanceRepair`
   + `Key=Environment,Value=Production`

   For more information about tagging Systems Manager resources, see [Tagging Systems Manager resources](tagging-resources.md)\.

1. Choose **Save and preview**\.

1. Review the details of the change template you're creating\.

   If you want to make change to the change template before submitting it for review, choose **Actions, Edit**\.

   If you're satisfied with the contents of the change template, choose **Submit for review**\. The users in your organization or account who have been specified as template reviewers on the **Settings** tab in Change Manager are notified that a new change template is pending their review\. 

   If an Amazon SNS topic has been specified for change templates, notifications are sent when the change template is rejected or approved\. If you don't receive notifications related to this change template, you can return to Change Manager later to check on its status\.