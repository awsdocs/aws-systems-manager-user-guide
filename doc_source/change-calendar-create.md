# Creating a change calendar<a name="change-calendar-create"></a>

When you create an entry in Change Calendar, a capability of AWS Systems Manager, you're creating a Systems Manager document \(SSM document\) that uses the `text` format\.

**To create a change calendar**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Change Calendar**\.

1. Choose **Create calendar**\.

   \-or\-

   If the **Change Calendar** home page opens first, choose **Create change calendar**\.

1. On the **Create calendar** page, in **Calendar details**, enter a name for your calendar entry\. Calendar entry names can contain letters, numbers, periods, dashes, and underscores\. The name should be specific enough to identify the purpose of the calendar entry at a glance\. An example is **support\-off\-hours**\. You can't update this name after you create the calendar entry\.

1. \(Optional\) For **Description**, enter a description for your calendar entry\.

1. \(Optional\) In the **Import calendar** area, choose **Choose file** to select an iCalendar \(`.ics`\) file that you have exported from a third\-party calendar provider\. Importing the file will add its events to your calendar\.

   Supported providers include Google Calendar, Microsoft Outlook, and iCloud Calendar\.

   For more information, see [Importing events from third\-party calendar providers](change-calendar-import.md)\.

1. In **Calendar type**, choose one of the following\.
   + **Open by default** \- The calendar is open \(Automation actions can run until an event starts\), then closed for the duration of an associated event\.
   + **Closed by default** \- The calendar is closed \(Automation actions can't run until an event starts\) but open for the duration of an associated event\.

1. \(Optional\) In **Change management events**, select **Add change management events to the calendar**\. This selection displays all scheduled maintenance windows, State Manager associations, Automation workflows, and Change Manager change requests in your monthly calendar display\.
**Tip**  
If later you want to permanently remove these events types from the calendar display, edit the calendar, clear this check box, and then choose **Save**\.

1. Choose **Create calendar**\.

   After the calendar entry is created, Systems Manager displays your calendar entry in the **Change Calendar** list\. The columns show the calendar version and the calendar owner's AWS account number\. Your calendar entry can't prevent or allow any actions until you have created or imported at least one event\. For information about creating an event, see [Creating a Change Calendar event](change-calendar-create-event.md)\. For information about importing events, see [Importing events from third\-party calendar providers](change-calendar-import.md)\.