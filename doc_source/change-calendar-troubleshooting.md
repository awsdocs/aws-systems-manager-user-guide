# Troubleshooting Change Calendar<a name="change-calendar-troubleshooting"></a>

Use the following information to help you troubleshoot problems with Change Calendar, a capability of AWS Systems Manager\.

**Topics**
+ ['Calendar import failed' error](#change-manager-troubleshooting-1)

## 'Calendar import failed' error<a name="change-manager-troubleshooting-1"></a>

**Problem**: When importing an iCalendar \(`.ics`\) file, the system reports that the calendar import failed\.
+ **Solution 1** – Ensure that you are importing a file that was exported from a supported third\-party calendar provider, which include the following:
  + Google Calendar \([Export instructions](https://support.google.com/calendar/answer/37111)\)
  + Microsoft Outlook \([Export instructions](https://support.microsoft.com/en-us/office/export-an-outlook-calendar-to-google-calendar-662fa3bb-0794-4b18-add8-9968b665f4e6)\)
  + iCloud Calendar \([Export instructions](https://support.apple.com/guide/calendar/import-or-export-calendars-icl1023/mac)\)
+ **Solution 2** – If your source calendar contains any recurring events, ensure that no individual occurrences of the event have been canceled or deleted\. Currently, Change Calendar doesn't support importing recurring events with individual cancellations\. To resolve the issue, remove the recurring event from the source calendar, re\-export the calendar and re\-import it into Change Calendar, and then add the recurring event using the Change Calendar interface\. For information, see [Creating a Change Calendar event](change-calendar-create-event.md)\.
+ **Solution 3** – Ensure that your source calendar contains at least one event\. Uploads of `.ics` files that don't contain events don't succeed\.
+ **Solution 4** – If the system reports that the import failed because the `.ics` is too large, ensure that you are exporting only basic details about your calendar entries\. If necessary, reduce the length of the time period that you export\.
+ **Solution 5** – If Change Calendar is unable to determine the time zone of your exported calendar when you attempt to import it from the **Events** tab, you might receive this message: "Calendar import failed\. Change Calendar couldn't locate a valid time zone\. You can import the calendar from the Edit menu\." In this case, choose **Actions, Edit**, and then try importing the file from the **Edit calendar** page\.
+ **Solution 6** – Don't edit the `.ics` file before import\. Attempting to modify the contents of the file can corrupt the calendar data\. If you have modified the file before attempting the import, export the calendar from the source calendar again, and then re\-attempt the upload\.