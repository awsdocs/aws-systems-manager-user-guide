# Reviewing change request details, tasks, and timelines \(console\)<a name="reviewing-changes"></a>

You can view information about a change request, including requests for which changes have already been processed, in the dashboard of Change Manager, a capability of AWS Systems Manager\. These details include a link to the Automation operation that runs the runbooks that make the change\. An Automation execution ID is generated when the request is created, but the process doesn't run until all approvals have been given and no restrictions are in place to block the change\.

**To review change request details, tasks, and timelines**

1. In the navigation pane, choose **Change Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Change Manager**\.

1. Choose the **Requests** tab\.

1. In the **Change requests** section, search for the change request you want to review\. 

   You can use the **Create date range** options to limit results to a specific time period\.

   You can filter requests by the following properties:
   + `Status`
   + `Request ID`
   + `Approver`
   + `Requester`

   For example, to view details about all change requests that have completed successfully in the past 24 hours, do the following:

   1. For **Create date range**, choose **1d**\.

   1. In the search box, select **Status, CompletedWithSuccess**\. 

   1. In the results, choose the name of the successfully completed change request to review results for\.

1. View information about the change request on the following tabs:
   + **Request details** – View basic details about the change request, including the requester, the change template, and the Automation runbooks selected for the change\. You can also follow a link to the Automation operation details and view information about any runbook parameters specified in the request, Amazon CloudWatch alarms assigned to the change request, and approvals and comments provided for the request\.
   + **Task** – View information about the task in the change, including task status for completed change requests, the targeted resources, the steps in the associated Automation runbooks, and concurrency and error threshold details\.
   + **Timeline** – View a summary of all events associated with the change request, listed by date and time\. The summary indicates when the change request was created, actions by assigned approvers, a note of when approved change requests are scheduled to run, runbook workflow details, and status changes for the overall change process and each step in the runbook\.
   + **Associated events** – View auditable details about change requests that are recorded in [AWS CloudTrail Lake](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-lake.html)\. This includes details such as which API actions were run, the request parameters included for those actions, the user account that ran the action, the resources updated during the process, and more\.

     When you enable AWS CloudTrail Lake event tracking, CloudTrail Lake creates an event data store for events related to your change requests\. The event details are available for the account or organization where the change request ran\. You can turn on CloudTrail Lake event tracking from any change request in your account or organization\. For information about enabling CloudTrail Lake integration and creating an event data store, see [Monitoring your change request events](monitoring-change-request-events.md)\.
**Note**  
There is a charge to use **CloudTrail Lake**\. For details, see [AWS CloudTrail pricing](http://aws.amazon.com/cloudtrail/pricing/)\.