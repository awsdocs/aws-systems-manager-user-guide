# Create a Change Calendar entry<a name="change-calendar-create"></a>

When you create an AWS Systems ManagerChange Calendar entry, you are creating a Systems Manager document that uses the `text` format\.

## Create a Change Calendar entry \(console\)<a name="change-calendar-create-console"></a>

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Change Calendar**\.

1. On the **Change Calendar** home page, choose **Create calendar**\.

1. On the **Create calendar** page, in **Calendar details**, enter a name for your calendar entry\. Calendar entry names can contain letters, numbers, periods, dashes, and underscores\. The name should be specific enough to identify the purpose of the calendar entry at a glance\. An example is **support\-off\-hours**\. You cannot update this name after you create the calendar entry\.

1. In **Description**, enter a description for your calendar entry\.

1. In **Calendar type**, choose one of the following\.
   + **Open by default** \- the calendar is open \(Automation actions can run until an event starts\), then closed for the duration of an associated event\.
   + **Closed by default** \- the calendar is closed \(Automation actions cannot run until an event starts\) but open for the duration of an associated event\.

1. Choose **Create calendar**\.

   After the calendar entry is created, Systems Manager displays your calendar entry in the **Change Calendar** list\. The columns show the calendar version, and the calendar owner's AWS account number\. Your calendar entry cannot prevent or allow any actions until you add at least one event\. For information about how to add an event, see [Create a Change Calendar event](change-calendar-create-event.md)\.