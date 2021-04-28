# Updating a Change Calendar entry<a name="change-calendar-update"></a>

You can update the description of an entry in Change Calendar, a capability in AWS Systems Manager, but not its name\. Although you can change the default state of a calendar entry, be aware that this reverses the behavior of change actions during events that are associated with the calendar entry\. For example, if you change the state of a calendar from **Open by default** to **Closed by default**, unwanted changes might be made during event periods when the users who created the associated events are not expecting changes\.

When you update a Change Calendar entry, you are editing the Change Calendar document that you created when you created the entry\.

**To update a Change Calendar entry**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Change Calendar**\.

1. In the **Change Calendar** list, choose the name of the calendar entry that you want to update\.

1. On the calendar entry's details page, choose **Edit**\.

1. In **Description**, you can change the description text\. You cannot edit the name of a Change Calendar entry\.

1. To change the calendar state, in **Calendar type**, choose a different value\. Be aware that this reverses the behavior of change actions during events that are associated with the calendar entry\. Before you change the calendar type, you should verify with other Change Calendar users that changing the calendar type does not allow unwanted changes during events that they have created\.
   + **Open by default** \- The calendar is open \(Automation actions can run until an event starts\), then closed for the duration of an associated event\.
   + **Closed by default** \- The calendar is closed \(Automation actions cannot run until an event starts\) but open for the duration of an associated event\.

1. Choose **Save**\.

   Your calendar entry cannot prevent or allow any actions until you add at least one event\. For information about how to add an event, see [Creating a Change Calendar event](change-calendar-create-event.md)\.