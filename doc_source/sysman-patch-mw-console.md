# Creating a maintenance window for patching<a name="sysman-patch-mw-console"></a>

**Important**  
You can continue to use this legacy topic to create a maintenance window for patching\. However, we recommend using the **Configure patching** page instead\. For more information, see [Creating a patching configuration \(console\)](create-patching-configuration.md)\. And while many patching use cases benefit from patching instances on a schedule with a maintenance window, but you can also run a one\-time patching operation manually without a maintenance window\. For more information, see [Patching instances on demand](patch-on-demand.md)\.

To minimize the impact on your server availability, we recommend that you configure a maintenance window to run patching during times that won't interrupt your business operations\. For more information about maintenance windows, see [AWS Systems Manager Maintenance Windows](systems-manager-maintenance.md)\.

You must configure roles and permissions for Maintenance Windows before beginning this procedure\. For more information, see [Controlling access to maintenance windows](sysman-maintenance-permissions.md)\. 

**To create a maintenance window for patching**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Maintenance Windows**\.

1. Choose **Create maintenance window**\.

1. For **Name**, enter a name that designates this as a maintenance window for patching critical and important updates\.

1. For **Description**, enter a description\. 

1. Choose **Allow unregistered targets** if you want to allow a maintenance window task to run on managed instances, even if you have not registered those instances as targets\. If you choose this option, then you can choose the unregistered instances \(by instance ID\) when you register a task with the maintenance window\.

   If you don't choose this option, then you must choose previously\-registered targets when you register a task with the maintenance window\. 

1. In the top of the **Schedule** section, specify a schedule for the maintenance window by using one of the three scheduling options\.

   For information about building cron/rate expressions, see [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

1. For **Duration**, type the number of hours the maintenance window will run\. The value you specify determines the specific end time for the maintenance window based on the time it begins\. No maintenance window tasks are permitted to start after the resulting endtime minus the number of hours you specify for **Stop initiating tasks** in the next step\. 

   For example, if the maintenance window starts at 3 PM, the duration is three hours, and the **Stop initiating tasks** value is one hour, no maintenance window tasks can start after 5 PM\. 

1. For **Stop initiating tasks**, enter the number of hours before the end of the maintenance window that the system should stop scheduling new tasks to run\. 

1. \(Optional\) For **Start date \(optional\)**, specify a date and time, in ISO\-8601 Extended format, for when you want the maintenance window to become active\. This allows you to delay activation of the maintenance window until the specified future date\.

1. \(Optional\) For **End date \(optional\)**, specify a date and time, in ISO\-8601 Extended format, for when you want the maintenance window to become inactive\. This allows you to set a date and time in the future after which the maintenance window no longer runs\.

1. \(Optional\) For **Time zone \(optional\)**, specify the time zone to base scheduled maintenance window executions on, in Internet Assigned Numbers Authority \(IANA\) format\. For example: "America/Los\_Angeles", "etc/UTC", or "Asia/Seoul"\.

   For more information about valid formats, see the [Time Zone Database](https://www.iana.org/time-zones) on the IANA website\.

1. Choose **Create maintenance window**\.

1. In the maintenance windows list, choose the maintenance window you just created, and then choose **Actions**, **Register targets**\.

1. \(Optional\) In the **Maintenance window target details** section, provide a name, a description, and owner information \(your name or alias\) for this target\.

1. For **Targets**, choose **Specifying instance tags**\.

1. For **Instance tags**, enter a tag key and a tag value to identify the instances to register with the maintenance window, and then choose **Add**\.

1. Choose **Register target**\. The system creates a maintenance window target\.

1. In the details page of the maintenance window you created, choose **Actions**, **Register Run command task**\.

1. \(Optional\) For **Maintenance window task details**, provide a name and description for this task\.

1. For **Command document**, choose **AWS\-RunPatchBaseline**\.

1. For **Task priority**, choose a priority\. Zero \(`0`\) is the highest priority\.

1. For **Targets**, under **Target by**, choose the maintenance window target you created earlier in this procedure\.

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

1. In the **SNS notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.

1. For **Parameters**:
   + For **Operation**, choose **Scan** to scan for missing patches, or choose **Install** to scan for and install missing patches\.
   + You don't need to enter anything in the **Snapshot Id** field\. This system automatically generates and provides this parameter\.
   + You don't need to enter anything in the **Install Override List** field unless you want Patch Manager to use a different patch set than is specified for the patch baseline\. For information, see [Parameter name: `InstallOverrideList`](patch-manager-about-aws-runpatchbaseline.md#patch-manager-about-aws-runpatchbaseline-parameters-installoverridelist)\.
   + For **Reboot option**, specify whether you want instances to reboot if patches are installed during the `Install` operation, or if Patch Manager detects other patches that were installed since the last instance reboot\. For information, see [Parameter name: `RebootOption`](patch-manager-about-aws-runpatchbaseline.md#patch-manager-about-aws-runpatchbaseline-parameters-norebootoption)\.
   + \(Optional\) For **Comment**, enter a tracking note or reminder about this command\.
   + For **Timeout \(seconds\)**, enter the number of seconds the system should wait for the operation to finish before it is considered unsuccessful\.

1. Choose **Register run command task**\.

After the maintenance window task completes, you can view patch compliance details in the Systems Manager console on the **Managed Instances** page\. In the filter bar, use the **AWS:PatchSummary** and **AWS:PatchCompliance** filters\. 

**Note**  
You can save your query by bookmarking the URL after you specify the filters\.

You can also drill down on a specific instance by choosing the instance in the **Managed Instances** page, and then choosing the **Patch** tab\. You can also use the [DescribePatchGroupState](https://docs.aws.amazon.com/ssm/latest/APIReference/API_DescribePatchGroupState.html) and [DescribeInstancePatchStatesForPatchGroup](https://docs.aws.amazon.com/ssm/latest/APIReference/API_DescribeInstancePatchStatesForPatchGroup.html) APIs to view compliance details\. For information about patch compliance data, see [About patch compliance](sysman-compliance-about.md#sysman-compliance-monitor-patch)\.