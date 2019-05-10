# Create a Maintenance Window \(Console\)<a name="sysman-maintenance-create-mw"></a>

In this procedure, you create a Maintenance Window and specify its basic options, such as name, schedule, and duration\. In later steps, you choose the targets, or resources, that it updates and the tasks that run during the Maintenance Window execution\.

**Note**  
For an explanation of how the various schedule\-related options for Maintenance Windows relate to one another, see [Reference: Maintenance Window Scheduling and Active Period Options](reference-maintenance-windows-schedule-options.md)\.

After a task runs

**To create a Maintenance Window \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

   \-or\-

   Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.
**Note**  
If you are using the Amazon EC2 console, some field names and locations may differ slightly\.

1. In the navigation pane, choose **Maintenance Windows**\. 

1. Choose **Create a Maintenance Window**\.

1. For **Name**, enter a descriptive name to help you identify this Maintenance Window as a test Maintenance Window\.

1. For **Description**, enter a description\.

1. Choose **Allow unregistered targets** if you want to allow a Maintenance Window task to run on managed instances, even if you have not registered those instances as targets\. If you choose this option, then you can choose the unregistered instances \(by instance ID\) when you register a task with the Maintenance Window\.

   If you don't choose this option, then you must choose previously\-registered targets when you register a task with the Maintenance Window\.

1. Specify a schedule for the Maintenance Window by using one of the three scheduling options\.

   For information about building cron/rate expressions, see [Reference: Cron and Rate Expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

1. For **Duration**, enter the number of hours the Maintenance Window should run\.

1. For **Stop initiating tasks**, enter the number of hours before the end of the Maintenance Window that the system should stop scheduling new tasks to run\.

1. \(Optional\) For **Start date \(optional\)**, specify a date and time, in ISO\-8601 Extended format, for when you want the Maintenance Window to become active\. This allows you to delay activation of the Maintenance Window until the specified future date\.

1. \(Optional\) For **End date \(optional\)**, specify a date and time, in ISO\-8601 Extended format, for when you want the Maintenance Window to become inactive\. This allows you to set a date and time in the future after which the Maintenance Window no longer runs\.

1. \(Optional\) For **Time zone \(optional\)**, specify the time zone to base scheduled Maintenance Window executions on, in Internet Assigned Numbers Authority \(IANA\) format\. For example: "America/Los\_Angeles", "etc/UTC", or "Asia/Seoul"\.

   For more information about valid formats, see the [Time Zone Database](https://www.iana.org/time-zones) on the IANA website\.

1. \(Optional\) In the **Manage tags** area, apply one or more tag key name/value pairs to the Maintenance Window\.

   Tags are optional metadata that you assign to a resource\. Tags enable you to categorize a resource in different ways, such as by purpose, owner, or environment\. For example, you might want to tag a Maintenance Window to identify the type of tasks it runs, the types of targets, and the environment it runs in\. In this case, you could specify the following key name/value pairs:
   + `Key=TaskType,Value=AgentUpdate`
   + `Key=OS,Value=Windows`
   + `Key=Environment,Value=Production`

1. Choose **Create maintenance window**\. The system returns you to the Maintenance Window page\. The state of the Maintenance Window you just created is **Enabled**\.