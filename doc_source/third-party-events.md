# Importing and managing events from third\-party calendars<a name="third-party-events"></a>

As an alternative to creating events directly in the AWS Systems Manager console, you can import an iCalendar \(`.ics`\) file from a supported third\-party calendar application\. Your calendar can include both imported events and events that you create in Change Calendar, which is a capability of AWS Systems Manager\.

**Before you begin**  
Before you attempt to import a calendar file, review the following requirements and constraints:

Calendar file format  
Only valid iCalendar files \(`.ics`\) are supported\.

Supported calendar providers  
Only `.ics` files exported from the following third\-party calendar providers are supported:  
+ Google Calendar \([Export instructions](https://support.google.com/calendar/answer/37111)\)
+ Microsoft Outlook \([Export instructions](https://support.microsoft.com/en-us/office/export-an-outlook-calendar-to-google-calendar-662fa3bb-0794-4b18-add8-9968b665f4e6)\)
+ iCloud Calendar \([Export instructions](https://support.apple.com/guide/calendar/import-or-export-calendars-icl1023/mac)\)

File size  
You can import any number of valid `.ics` files\. However, the total size of all imported files for each calendar can't exceed 64KB\.  
To minimize the size of the `.ics` file, ensure that you are exporting only basic details about your calendar entries\. If necessary, reduce the length of the time period that you are exporting\.

Time zone  
In addition to a calendar name, a calendar provider, and at least one event, your exported `.ics` file should also indicate the time zone for the calendar\. If it does not, or there is a problem identifying the time zone, you will be prompted to specify one after you import the file\.

Recurring event limitation  
Your exported `.ics` file can include recurring events\. However, if one or more occurrences of a recurring event had been deleted in the source calendar, the import fails\.

**Topics**
+ [Importing events from third\-party calendar providers](change-calendar-import.md)
+ [Updating all events from a third\-party calendar provider](change-calendar-import-add-remove.md)
+ [Deleting all events imported from a third\-party calendar](change-calendar-delete-ics.md)