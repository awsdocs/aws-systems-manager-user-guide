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