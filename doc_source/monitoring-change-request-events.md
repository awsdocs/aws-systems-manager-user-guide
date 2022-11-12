# Monitoring your change request events<a name="monitoring-change-request-events"></a>

After turning on integration with AWS CloudTrail Lake and creating an event data store, you can view auditable details about the change requests that are run in your account or organization\. These details include the identify of the user that initiated the change request, the Regions where the changes were made, the source IP address for the request, the AWS access key used for the request, and more\.

The following are samples of event details you can view for a change request after creating the event data store in AWS CloudTrail Lake\.

------
#### [ Details ]

![\[Details of a change request from CloudTrail Lake\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/cm-event-details.png)

------
#### [ Event record ]

![\[JSON record of a change request from CloudTrail Lake\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/cm-event-record.png)

------

**Important**  
If you are using Change Manager for an organization, you can complete the following procedure while signed in to either the management account or the delegated administrator account for Change Manager\.  
However, in order to use the delegated administrator account to complete these steps, the same delegated administrator account must be specified for both CloudTrail and Change Manager\.  
When you sign in to the management account for Change Manager, you can add or change the delegated administrator account for CloudTrail on the CloudTrail [Settings](https://console.aws.amazon.com/cloudtrailv2/home#/settings) page\. This must be done before the delegated administrator account can create an event data store for use by the entire organization\.

**To turn on CloudTrail Lake event tracking from Change Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Change Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Change Manager**\.

1. Choose the **Requests** tab\.

1. Choose any existing change request, and then choose the **Associated events** tab\.

1. Choose **Enable CloudTrail Lake**\.

1. Follow the steps in [Create an event data store](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store.html) in the *AWS CloudTrail User Guide*, beginning with step 3\.

   To ensure that event data for your change requests is stored, make the following selections:
   + After step 8, for **Event type**, leave the defaults **AWS events** and **CloudTrail events** selected\.
   + On step 12, if you are using Change Manager with an organization, select **Enable for all accounts in my organization**\.
   + On step 13, for **Management events**, do not clear the **Write** check box\. 

   Complete the **Create event data store** wizard\. Other options you choose when creating your event data store do not affect the storage of event data for your change requests\.