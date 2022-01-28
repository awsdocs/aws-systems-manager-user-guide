# Updating all events from a third\-party calendar provider<a name="change-calendar-import-add-remove"></a>

If several events are added to or removed from your source calendar after you have imported its iCalendar `.ics` file, you can reflect those changes in Change Calendar\. First, re\-export the source calendar, and then import the new file into Change Calendar, which is a capability of AWS Systems Manager\. Events in your change calendar will be updated to reflect the contents of the newer file\. 

**To update all events from a third\-party calendar provider**

1. In your third\-party calendar, add or remove events as you want them to be reflected in Change Calendar, and then re\-export the calendar to a new `.ics` file\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Change Calendar**\.

1. From the list of calendars, choose the calendar name from the list\.

1. Choose **Choose file**, and then browse to and select the replacement `.ics` file\.

1. In response to the notification about overwriting the existing file, choose **Confirm**\.