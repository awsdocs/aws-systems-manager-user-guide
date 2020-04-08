# Create a Change Calendar event<a name="change-calendar-create-event"></a>

When you add an event to a Change Calendar entry, you are specifying a period of time during which the default action of the calendar entry is suspended\. For example, if the calendar entry type is closed by default, the calendar is open to changes during events\. In this release, you can only create a Change Calendar event by using the console\. Events are added to the Systems Manager Change Calendar document that you create when you create a Change Calendar entry\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Change Calendar**\.

1. In the **Change Calendar** list, choose the name of the calendar entry to which you want to add an event\.

1. On the calendar entry's details page, choose **Create event**\.

1. On the **Create scheduled event** page, in **Event details**, enter a display name for your event\. Event names can contain letters, numbers, periods, dashes, and underscores\. The name should be specific enough to identify the purpose of the event\. An example is **nighttime\-hours**\. You cannot update this name after you create the event\.

1. In **Description**, enter a description for your event\. For example, **The support team is not available during these hours**\.

1. In **Event start date**, enter or choose a day in the format `MM/DD/YYYY` to start the event, and enter a time on the specified day in the format `hh:mm:ss` \(hours, minutes, and seconds\) to start the event\.

1. In **Event end date**, enter or choose a day in the format `MM/DD/YYYY` to end the event, and enter a time on the specified day in the format `hh:mm:ss` \(hours, minutes, and seconds\) to end the event\.

1. In **Schedule time zone**, choose a time zone that applies to the start and end times of the event\. You can enter part of a city name or time zone difference from Greenwich Mean Time \(GMT\) to find a time zone faster\. The default is Universal Coordinated Time \(UTC\)\.

1. To create an event that recurs daily, weekly, or monthly, turn on **Recurrence**\.

1. Choose **Create scheduled event**\. The new event is added to your calendar entry, and is displayed on the **Events** tab of the calendar entry's details page\.