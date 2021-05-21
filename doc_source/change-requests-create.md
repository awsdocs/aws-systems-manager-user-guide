# Creating change requests<a name="change-requests-create"></a>

When you create a change request in Change Manager, a capability of AWS Systems Manager, the change template you select typically does the following:
+ Designates approvers for the change request or specifies how many approvals are required
+ Specifies the Amazon Simple Notification Service \(Amazon SNS\) topic to use to notify approvers about your change request
+ Specifies an Amazon CloudWatch alarm to monitor the runbook workflow for the change request
+ Identifies which Automation runbooks you can choose from to make the requested change

In some cases, a change template might be configured so you specify your own Automation runbook to use, and to specify who should review and approve the request\.

**Important**  
If you use Change Manager across an organization, we recommend always making changes from the delegated administrator account\. While you can make changes from other accounts in the organization, those changes will not be reported in or viewable from the delegated administrator account\.

The following procedure describes how to create a change request by using the Systems Manager console\.

**To create a change request**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Change Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Change Manager**\.

1. Choose **Create request**\.

1. Search for and select a change template that you want to use for this change request\.

1. Choose **Next**\.

1. For **Name**, enter a name for the change request that makes its purpose easy to identify, such as **UpdateEC2LinuxAMI\-us\-east\-2**\.

1. For **Runbook**, select the runbook you want to use to make your requested change\.
**Note**  
If the option to select a runbook is not available, the change template author has specified which runbook must be used\.

1. For **Change request information**, use Markdown to provide additional information about the change request to help reviewers decide whether to approve or reject the change request\. The author of the template you are using may have provided instructions or questions for you to answer\.
**Note**  
Markdown is a markup language that allows you to add wiki\-style descriptions to documents and individual steps within the document\. For more information about using Markdown, see [Using Markdown in AWS](https://docs.aws.amazon.com/general/latest/gr/aws-markdown.html)\.

1. In the **Workflow start time** section, choose one of the following:
   + **Run the operation at a scheduled time** – For **Requested start time**, enter the date and time you propose for running the runbook workflow for this request\. For **Estimated end time**, enter the date and time that you expect the runbook workflow to complete\. \(This time is an estimate only that you are providing for reviewers\.\)
**Tip**  
Choose **View Change Calendar** to check for any blocking events for the time you specify\.
   + **Run the operation as soon as possible after approval** – If the change request is approved, the runbook workflow runs as soon as there is a non\-restricted period when changes can be made\.

1. In the **Change request approvers** section, do the following:

   1. Choose **Add approver**, and then select one or more users or groups from the lists of available reviewers\.
**Note**  
One or more approvers might already be specified\. This means that mandatory approvers are already specified in the change template you have selected\. These approvers cannot be removed from the request\. If the **Add approver** button is not enabled, the template you have chosen doesn't allow additional reviewers to be added to requests\.

   1. Under **SNS topic to notify approvers**, choose one of the following to specify the Amazon SNS topic in your account to use for sending notifications to the approvers you are adding to this change request\.
**Note**  
If the option to specify an Amazon SNS topic is not available, the change template you selected already specifies the Amazon SNS topic to use\.
      + **Enter an SNS Amazon Resource Name \(ARN\)** – For **Topic ARN**, enter the ARN of an existing Amazon SNS topic\. This topic can be in any of your organization's accounts\.
      + **Select an existing SNS topic** – For **Target notification topic**, select the ARN of an existing Amazon SNS topic in your current account\. \(This option is not available if you haven't yet created any Amazon SNS topics in your current Amazon Web Servicesaccount and AWS Region\.\)
**Note**  
The Amazon SNS topic you select must be configured to specify the notifications it sends and the subscribers they are sent to\. Its access policy must also grant permissions to Systems Manager so Change Manager can send notifications\. For information, see [Configuring Amazon SNS topics for Change Manager notifications](change-manager-sns-setup.md)\. 

   1. Choose **Add notification**\.

1. Choose **Next**\.

1. For **IAM role**, select an AWS Identity and Access Management \(IAM\) role *in your current account * that has the permissions needed to run the runbooks that are specified for this change request\.

   This role is also referred to as the service role, or assume role, for Automation\. For more information about this role, see [Setting up Automation](automation-setup.md)\.

1. In the **Deployment location** section, choose one of the following:
**Note**  
If you are using Change Manager with a single Amazon Web Services account only and not with an organization set up in AWS Organizations, you don't need to specify a deployment location\.
   + **Apply change to this account** – The runbook workflow runs in the current account only\. For an organization, this means the delegated administrator account\.
   + **Apply change to multiple organizational units \(OUs\)** – Do the following: 

     1. For **Accounts and organizational units \(OUs\)**, enter the ID of a member account in your organization, in the format **123456789012**, or the ID of an organizational unit, in the format **o\-o96EXAMPLE**\. 

     1. \(Optional\) For **Execution role name**, enter the name of the IAM role *in the target account* or OU that has the permissions needed to run the runbooks that are specified for this change request\. All accounts in any OU you specify should use the same name for this role\.

     1. \(Optional\) Choose **Add another target location** for each additional account or OU you want to specify and repeat steps a and b\. 

     1. For **Target AWS Region**, select the Region to make the change in, such as `Ohio (us-east-2)` for the US East \(Ohio\) Region\.

     1. Expand **Rate control**\. 

        For **Concurrency**, enter a number, then from the list select whether this represents the number or percentage of accounts the runbook workflow can run in at the same time\. 

        For **Error threshold**, enter a number, then from the list select whether this represents the number or percentage of accounts where runbook workflow can fail before the operation is stopped\. 

1. In the **Deployment targets** section, do the following:

   1. Choose one of the following:
      + **Single resource** – The change is to be made for just one resource\. For example, a single instance or a single Amazon Machine Image \(AMI\), depending on the operation defined in the runbooks for this change request\.
      + **Multiple resources** – For **Parameter**, select from the available parameters from the runbooks for this change request\. This selection reflects the type of resource being updated\.

        For example, if the runbook for this change request is `AWS-RetartEC2Instance`, you might choose `InstanceId`, and then define which instances are updated by selecting from the following:
        + **Specify tags** – Enter a key\-value pair that all resources to be updated are tagged with\.
        + **Choose a resource group** – Choose the name of the resource group that all resources to be updated belong to\.
        + **Specify parameter values** – Identify the resources to update in the **Runbook parameters** section\.
        + **Target all instances** – Make the change on all managed instances in the target locations\.

   1. If you chose **Multiple resources**, expand **Rate control**\. 

      For **Concurrency**, enter a number, then from the list select whether this represents the number or percentage of targets the runbook workflow can update at the same time\. 

      For **Error threshold**, enter a number, then from the list select whether this represents the number or percentage of targets where the update can fail before the operation is stopped\. 

1. If you chose **Specify parameter values** to update multiple resources in the previous step: In the **Runbook parameters** section, specify values for the required input parameters\. The parameter values you must supply are based on the contents of the Automation runbooks associated with the change template you chose\. 

   For example, if the change template uses the `AWS-RetartEC2Instance` runbook, then you must enter one or more instance IDs for the **InstanceId** parameter\. Alternatively, choose **Show interactive instance picker** and select available instances one by one\. 

1. Choose **Next**\.

1. In the **Review and submit** page, double\-check the resources and options you have specified for this change request\.

   Choose the **Edit** button for any section you want to make changes to\.

   When you are satisfied with the change request details, choose **Submit for approval**\.

If an Amazon SNS topic has been specified in the change template you chose for the request, notifications are sent when the request is rejected or approved\. If you don't receive notifications for the request, you can return to Change Manager to check the status of your request\. 