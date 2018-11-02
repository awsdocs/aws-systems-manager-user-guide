# Create a Maintenance Window \(Console\)<a name="sysman-maintenance-create-mw"></a>

To create a Maintenance Window, do the following:
+ Create a window and define its schedule and duration\.
+ Assign targets for the window\.
+ Assign tasks to run during the window\.

After you complete these steps, the Maintenance Window runs according to the schedule you defined and runs the tasks on the targets you specified\. After a task is finished, Systems Manager logs the details of the execution\. 

You can run the following types of tasks on targets:
+ Commands by using Systems Manager Run Command
**Note**  
To create a Maintenance Window for patching operations, we recommend using the **Configure patching** page in Patch Manager instead of creating it in the **Maintenance Window** area of Systems Manager\.
+ Automation workflows by using Systems Manager Automation
+ Functions by using AWS Lambda
+ State machines by using AWS Step Functions
**Note**  
The AWS Systems Manager console does not currently support running Step Functions\. To register this type of task, you must use the AWS CLI\. For examples of how to create, configure, and update a Maintenance Window by using the AWS CLI, see the [Systems Manager Maintenance Window Tutorials \(AWS CLI\)](maintenance-windows-tutorials.md)\.

**To create a Maintenance Window**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

   \-or\-

   Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.
**Note**  
If you are using the Amazon EC2 console, some field names and locations may differ slightly\.

1. In the navigation pane, choose **Maintenance Windows**\. 

1. Choose **Create a Maintenance Window**\.

1. In the **Name** field, enter a descriptive name to help you identify this Maintenance Window as a test Maintenance Window\.

1. In the **Description** field, enter a description\.

1. Choose **Allow unregistered targets** if you want to allow a Maintenance Window task to run on managed instances, even if you have not registered those instances as targets\. If you choose this option, then you can choose the unregistered instances \(by instance ID\) when you register a task with the Maintenance Window\.

   If you don't choose this option, then you must choose previously\-registered targets when you register a task with the Maintenance Window\.

1. Specify a schedule for the Maintenance Window by using one of the three scheduling options\.

   For information about building cron/rate expressions, see [Reference: Cron and Rate Expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

1. In the **Duration** field, enter the number of hours the Maintenance Window should run\.

1. In the **Stop initiating tasks** field, enter the number of hours before the end of the Maintenance Window that the system should stop scheduling new tasks to run\.

1. \(Optional\) In **Start date \(optional\)**, specify a date and time, in ISO\-8601 Extended format, for when you want the Maintenance Window to become active\. This allows you to delay activation of the Maintenance Window until the specified future date\.

1. \(Optional\) In **End date \(optional\)**, specify a date and time, in ISO\-8601 Extended format, for when you want the Maintenance Window to become inactive\. This allows you to set a date and time in the future after which the Maintenance Window will no longer run\.

1. \(Optional\) In **Time zone \(optional\)**, specify the time zone to base scheduled Maintenance Window executions on, in Internet Assigned Numbers Authority \(IANA\) format\. For example: "America/Los\_Angeles", "etc/UTC", or "Asia/Seoul"\.

   For more information about valid formats, see the [Time Zone Database](https://www.iana.org/time-zones) on the IANA website\.

1. Choose **Create maintenance window**\. The system returns you to the Maintenance Window page\. The state of the Maintenance Window you just created is **Enabled**\.