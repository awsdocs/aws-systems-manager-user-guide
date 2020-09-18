# Walkthrough: Create a maintenance window to update SSM Agent \(console\)<a name="mw-walkthrough-console"></a>

The following walkthrough shows you how to use the AWS Systems Manager console to create an AWS Systems Manager maintenance window\. The walkthrough also describes how to register your managed instances as targets and register a Run Command task to update SSM Agent\.

**Before you begin**  
Before you complete the following procedure, you must either have administrator privileges on the instances you want to configure or you must have been granted the appropriate permissions in AWS Identity and Access Management \(IAM\)\. Additionally, verify that you have at least one running EC2 instance for Linux or Windows Server that is configured for Systems Manager\. For more information, see [Systems Manager prerequisites](systems-manager-prereqs.md)\.

**Topics**
+ [Step 1: Create the maintenance window \(console\)](#mw-walkthrough-console-create)
+ [Step 2: Register maintenance window targets \(console\)](#mw-walkthrough-console-register-target)
+ [Step 3: Register a Run Command task for the maintenance window to update SSM Agent \(console\)](#mw-walkthrough-console-register-task)

## Step 1: Create the maintenance window \(console\)<a name="mw-walkthrough-console-create"></a>

**To create a maintenance window \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\. 

1. Choose **Create maintenance window**\.

1. For **Name**, enter a descriptive name to help you identify this maintenance window as a test maintenance window\.

1. For **Description**, enter a description\.

1. Choose **Allow unregistered targets** if you want to allow a maintenance window task to run on managed instances, even if you have not registered those instances as targets\. If you choose this option, then you can choose the unregistered instances \(by instance ID\) when you register a task with the maintenance window\.

   If you don't choose this option, then you must choose previously\-registered targets when you register a task with the maintenance window\.

1. Specify a schedule for the maintenance window by using one of the three scheduling options\.

   For information about building cron/rate expressions, see [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

1. For **Duration**, enter the number of hours the maintenance window should run\.

1. For **Stop initiating tasks**, enter the number of hours before the end of the maintenance window that the system should stop scheduling new tasks to run\.

1. \(Optional\) For **Start date \(optional\)**, specify a date and time, in ISO\-8601 Extended format, for when you want the maintenance window to become active\. This allows you to delay activation of the maintenance window until the specified future date\.

1. \(Optional\) For **End date \(optional\)**, specify a date and time, in ISO\-8601 Extended format, for when you want the maintenance window to become inactive\. This allows you to set a date and time in the future after which the maintenance window no longer runs\.

1. \(Optional\) For **Time zone \(optional\)**, specify the time zone to base scheduled maintenance window executions on, in Internet Assigned Numbers Authority \(IANA\) format\. For example: "America/Los\_Angeles", "etc/UTC", or "Asia/Seoul"\.

   For more information about valid formats, see the [Time Zone Database](https://www.iana.org/time-zones) on the IANA website\.

1. Choose **Create maintenance window**\. The system returns you to the maintenance window page\. The maintenance window you just created is in the Enabled state\.

## Step 2: Register maintenance window targets \(console\)<a name="mw-walkthrough-console-register-target"></a>

Use the following procedure to register a target with the maintenance window you created in Step 1\. By registering a target, you specify which instances to update\.

**To assign targets to a maintenance window \(console\)**

1. In the list of maintenance windows, choose the maintenance window you just created\.

1. Choose **Actions**, and then choose **Register targets**\.

1. For **Target Name**, enter a name for the target\.

1. For **Description**, enter a description\.

1. \(Optional\) For **Owner information**, specify your name or work alias\. Owner information is included in any Amazon EventBridge event raised while running tasks for these targets in this maintenance window\.

   For information about using EventBridge to monitor Systems Manager events, see [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md)\.

1. In the **Select targets by** section, choose **Specifying Tags** to target instances by using Amazon EC2 tags that you previously assigned to the instances\. Choose **Manually Selecting Instances** to choose individual instances according to their instance IDs\.
**Note**  
If you don't see the instances that you want to target, verify that those instances are configured for Systems Manager\. For more information, see [Setting up AWS Systems Manager](systems-manager-setting-up.md)\.

1. Choose **Register target**\.

## Step 3: Register a Run Command task for the maintenance window to update SSM Agent \(console\)<a name="mw-walkthrough-console-register-task"></a>

Use the following procedure to register a Run Command task for the maintenance window you created in Step 1\. The Run Command task updates SSM Agent on the registered targets\.

**To assign tasks to a maintenance window \(console\)**

1. In the list of maintenance windows, choose the maintenance window you just created\.

1. Choose **Actions**, and then choose **Register Run Command task**\.

1. For **Name**, enter a name for the task, such as UpdateSSMAgent\.

1. For **Description**, enter a description\.

1. For **Document**, choose the SSM Command document `AWS-UpdateSSMAgent`\.
**Note**  
If the targets you registered in the preceding step are Windows Server 2012 R2 or earlier, you must use the `AWS-UpdateEC2Config` document\.

1. For **Task priority**, specify a priority for this task\. Zero \(`0`\) is the highest priority\. Tasks in a maintenance window are scheduled in priority order with tasks that have the same priority scheduled in parallel\.

1. In the **Targets** section, identify the instances on which you want to run this operation by specifying tags, selecting instances manually, or specifying a resource group\.
**Note**  
If you choose to select instances manually, and an instance you expect to see is not included in the list, see [Some of my instances are missing](troubleshooting-remote-commands.md#where-are-instances) for troubleshooting tips\.

1. \(Optional\) For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed instances or by specifying AWS resource groups, and you are not certain how many instances are targeted, then restrict the number of instances that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Instances still processing the command might also send errors\.

1. For ** IAM service role**, choose one of the following options to provide permissions for Systems Manager to run tasks on your target instances:
   +  ** Create and use a service\-linked role for Systems Manager **

     Service\-linked roles provide a secure way to delegate permissions to AWS services because only the linked service can assume a service\-linked role\. Additionally, AWS automatically defines and sets the permissions of service\-linked roles, depending on the actions that the linked service performs on your behalf\.
**Note**  
If a service\-linked role has already been created for your account, choose **Use the service\-linked role for Systems Manager**\.
   + **Use a custom service role**

     You can create a custom service role for maintenance window tasks if you want to use stricter permissions than those provided by the service\-linked role\. 

     If you need to create a custom service role, see one of the following topics:
     + [Control access to maintenance windows \(console\)](sysman-maintenance-perm-console.md)
     + [Control access to maintenance windows \(AWS CLI\)](sysman-maintenance-perm-cli.md)
     + [Control access to maintenance windows \(Tools for Windows PowerShell\)](sysman-maintenance-perm-ps.md)

   To help you decide whether to use a custom service role or the Systems Manager service\-linked role with a maintenance window task, see [Should I use a service\-linked role or a custom service role to run maintenance window tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Enable writing output to S3** box\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. In addition, if the specified S3 bucket is in a different AWS account, ensure that the instance profile associated with the instance has the necessary permissions to write to that bucket\.

   To stream the output to a CloudWatch Logs log group, select the **CloudWatch output** box\. Type the log group name in the box\.

1. In the **SNS notifications** section, you can optionally enable Systems Manager to send notifications about command statuses using Amazon SNS\. If you choose to enable this option, you need to specify the following:

   1. The IAM role to trigger Amazon SNS notifications\.

   1. The Amazon SNS topic to be used\.

   1. The specific event types about which you want to be notified\.

   1. The notification type that you want to receive when the status of a command changes\. For commands sent to multiple instances, choose **Invocation** to receive notification on an invocation \(per\-instance\) basis when the status of each invocation changes\.

1. In the **Input Parameters** section, you can optionally provide a specific version of SSM Agent to install, or you can allow SSM Agent service to be downgraded to an earlier version\. However, for this walkthrough we don't provide a version\. Therefore, SSM Agent is updated to the latest version\.

1. Choose **Register Run Command task**\.