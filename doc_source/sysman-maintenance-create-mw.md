# Create a maintenance window \(console\)<a name="sysman-maintenance-create-mw"></a>

In this procedure, you create a maintenance window and specify its basic options, such as name, schedule, and duration\. In later steps, you choose the targets, or resources, that it updates and the tasks that run during the maintenance window execution\.

**Note**  
For an explanation of how the various schedule\-related options for maintenance windows relate to one another, see [Reference: Maintenance window scheduling and active period options](maintenance-windows-schedule-options.md)\.  
For more information about working with the `--schedule` option, see [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

**To create a maintenance window \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\. 

1. Choose **Create a maintenance window**\.

1. For **Name**, enter a descriptive name to help you identify this maintenance window as a test maintenance window\.

1. For **Description**, enter a description\.

1. Choose **Allow unregistered targets** if you want to allow a maintenance window task to run on managed instances, even if you have not registered those instances as targets\. If you choose this option, then you can choose the unregistered instances \(by instance ID\) when you register a task with the maintenance window\.

   If you don't choose this option, then you must choose previously\-registered targets when you register a task with the maintenance window\.

1. Specify a schedule for the maintenance window by using one of the three scheduling options\.

   For information about building cron/rate expressions, see [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

1. For **Duration**, enter the number of hours the maintenance window will run\. The value you specify determines the specific end time for the maintenance window based on the time it begins\. No maintenance window tasks are permitted to start after the resulting endtime minus the number of hours you specify for **Stop initiating tasks** in the next step\.

   For example, if the maintenance window starts at 3 PM, the duration is three hours, and the **Stop initiating tasks** value is one hour, no maintenance window tasks can start after 5 PM\.

1. For **Stop initiating tasks**, enter the number of hours before the end of the maintenance window that the system should stop scheduling new tasks to run\.

1. \(Optional\) For **Start date \(optional\)**, specify a date and time, in ISO\-8601 Extended format, for when you want the maintenance window to become active\. This allows you to delay activation of the maintenance window until the specified future date\.

1. \(Optional\) For **End date \(optional\)**, specify a date and time, in ISO\-8601 Extended format, for when you want the maintenance window to become inactive\. This allows you to set a date and time in the future after which the maintenance window no longer runs\.

1. \(Optional\) For **Time zone \(optional\)**, specify the time zone to base scheduled maintenance window executions on, in Internet Assigned Numbers Authority \(IANA\) format\. For example: "America/Los\_Angeles", "etc/UTC", or "Asia/Seoul"\.

   For more information about valid formats, see the [Time Zone Database](https://www.iana.org/time-zones) on the IANA website\.

1. \(Optional\) In the **Manage tags** area, apply one or more tag key name/value pairs to the maintenance window\.

   Tags are optional metadata that you assign to a resource\. Tags enable you to categorize a resource in different ways, such as by purpose, owner, or environment\. For example, you might want to tag a maintenance window to identify the type of tasks it runs, the types of targets, and the environment it runs in\. In this case, you could specify the following key name/value pairs:
   + `Key=TaskType,Value=AgentUpdate`
   + `Key=OS,Value=Windows`
   + `Key=Environment,Value=Production`

1. Choose **Create maintenance window**\. The system returns you to the maintenance window page\. The state of the maintenance window you just created is **Enabled**\.