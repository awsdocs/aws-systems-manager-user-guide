# Create a Maintenance Window for Patching<a name="sysman-patch-mw-console"></a>

**Important**  
You can continue to use this legacy topic to create a Maintenance Window for patching\. However, we recommend using the **Configure patching** page instead\. For more information, see [Create a Patching Configuration \(Console\)](create-patching-configuration.md)\.

To minimize the impact on your server availability, we recommend that you configure a Maintenance Window to execute patching during times that won't interrupt your business operations\. For more information about Maintenance Windows, see [AWS Systems Manager Maintenance Windows](systems-manager-maintenance.md)\.

You must configure roles and permissions for Maintenance Windows before beginning this procedure\. For more information, see [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)\. 

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To create a Maintenance Window for patching \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Maintenance Windows**\.

1. Choose **Create maintenance window**\.

1. For **Name**, enter a name that designates this as a Maintenance Window for patching critical and important updates\.

1. In the top of the **Schedule** section, choose the schedule options you want\.

1. For **Duration**, type the number of hours you want the Maintenance Window to be active\.

1. For **Stop initiating tasks**, type the number of hours before the Maintenance Window duration ends that you want the system to stop initiating new tasks\.

1. Choose **Create maintenance window**\.

1. In the Maintenance Windows list, choose the Maintenance Window you just created, and then choose **Actions**, **Register targets**\.

1. \(Optional\) In the **Maintenance window target details** section, provide a name, a description, and owner information \(your name or alias\) for this target\.

1. For **Targets**, choose **Specifying tags**\.

1. For **Tag**, enter a tag key and a tag value to identify the instances to register with the Maintenance Window\.

1. Choose **Register target**\. The system creates a Maintenance Window target\.

1. In the details page of the Maintenance Window you created, choose **Actions**, **Register run command task**\.

1. \(Optional\) For **Maintenance window task details**, provide a name and description for this task\.

1. For **Command document**, choose **AWS\-RunPatchBaseline**\.

1. For **Task priority**, choose a priority\. One is the highest priority\.

1. For **Targets**, under **Target by**, choose the Maintenance Window target you created earlier in this procedure\.

1. \(Optional\) For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by choosing Amazon EC2 tags, and you are not certain how many instances use the selected tags, then limit the number of instances that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Instances still processing the command might also send errors\.

1. For **Role**, enter the ARN of an IAM role to which the **AmazonSSMMaintenanceWindowRole** is attached\. For more information, see [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)\.

1. In the **Output options** section, if you want to save the command output to a file, select the **Write command output to an Amazon S3 bucket**\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\. 

1. In the **SNS Notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Configuring Amazon SNS Notifications for AWS Systems Manager](monitoring-sns-notifications.md)\.

1. For **Parameters**:
   + For **Operation**, choose **Scan** to scan for missing patches, or choose **Install** to scan for and install missing patches\.
**Note**  
The **Install** operation causes the instance to reboot \(if patches are installed\)\. The **Scan** operations does not cause a reboot\.
   + You don't need to enter anything in the **Snapshot Id** field\. This system automatically generates and provides this parameter\.
   + \(Optional\) For **Comment**, enter a tracking note or reminder about this command\.
   + For **Timeout \(seconds\)**, enter the number of seconds the system should wait for the operation to finish before it is considered unsuccessful\.

1. Choose **Register run command task**\.

**To create a Maintenance Window for patching \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Maintenance Windows**, and then choose **Create maintenance window**\.

1. For **Name**, enter a name that designates this as a Maintenance Window for patching critical and important updates\.

1. For **Specify schedule**, choose the schedule options you want\.

1. For **Duration**, enter the number of hours you want the Maintenance Window to be active\.

1. For **Stop initiating tasks**, enter the number of hours before the Maintenance Window duration ends that you want the system to stop initiating new tasks\.

1. Choose **Create maintenance window**\.

1. In this list of **Maintenance Windows**, select the Maintenance Window you just created, and then choose **Actions**, **Register targets**\.

1. \(Optional\) Near the top of the page, specify a name, description, and owner information \(your name or alias\) for this target\.

1. Next to **Select targets by**, choose **Specifying Tags**\.

1. Next to **Tag**, use the lists to choose a tag key and a tag value\.

1. Choose **Register targets**\. The system creates a Maintenance Window target\.

1. In the list of Maintenance Windows, choose the Maintenance Window you created with the procedure, and then choose **Actions**, **Register run command task**\.

1. In the **Command Document** section of the **Register run command task** page, choose **AWS\-RunPatchBaseline**\.

1. In the **Task Priority** section, specify a priority\. One is the highest priority\.

1. In the **Targets** section, choose **Select**, and then choose the Maintenance Window target you created earlier in this procedure\.

1. For **Role**, enter the ARN of a role which has the **AmazonSSMMaintenanceWindowRole** policy attached to it\. For more information, see [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)\.

1. For **Execute on**, choose either **Targets** or **Percent** to limit the number of instances where the system can simultaneously perform patching operations\.

1. For **Stop after**, specify the number of allowed errors before the system stops sending the patching task to other instances\.

1. For **Operation**, choose **Scan** to scan for missing patches, or choose **Install** to scan for and install missing patches\.
**Note**  
The **Install** operation causes the instance to reboot \(if patches are installed\)\. The **Scan** operations does not cause a reboot\.

1. You don't need to specify anything in the **Snapshot Id** field\. This system automatically generates and provides this parameter\.

1. In the **Advanced** section: 
   + If you want to write command output and results to an Amazon S3 bucket, choose **Write to S3**\. Type the bucket and prefix names in the boxes\.
   + If you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\. For more information about configuring Amazon SNS notifications for Run Command, see [Configuring Amazon SNS Notifications for AWS Systems Manager](monitoring-sns-notifications.md)\.

1. Choose **Register task**\.

After the Maintenance Window task completes, you can view patch compliance details in the Amazon EC2 console on the **Managed Instances** page\. In the filter bar, use the **AWS:PatchSummary** and **AWS:ComplianceItem** filters\. 

**Note**  
You can save your query by bookmarking the URL after you specify the filters\.

You can also drill down on a specific instance by choosing the instance in the **Managed Instances** page, and then choose the **Patch** tab\. You can also use the [DescribePatchGroupState](https://docs.aws.amazon.com/ssm/latest/APIReference/API_DescribePatchGroupState.html) and [DescribeInstancePatchStatesForPatchGroup](https://docs.aws.amazon.com/ssm/latest/APIReference/API_DescribeInstancePatchStatesForPatchGroup.html) APIs to view compliance details\. For information about patch compliance data, see [About Patch Compliance](sysman-compliance-about.md#sysman-compliance-monitor-patch)\.