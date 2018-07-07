# Walkthrough: Patch a Server Environment \(Console\)<a name="sysman-patch-consolewalk"></a>

The following walkthrough describes how to patch a server environment in the Systems Manager console by using a default patch baseline, patch groups, and a Maintenance Window\. To learn more about the processes described in this walkthrough, see [Working with Patch Manager](sysman-patch-working.md)\.

**Before You Begin**

Install or update the SSM Agent on your instances\. To patch Linux instances, your instances must be running SSM Agent version 2\.0\.834\.0 or later\. For information about updating the agent, see the section titled *Example: Update the SSM Agent* in [Running Commands from the Console](rc-console.md)\.

In addition, the following walkthrough runs patching during a Maintenance Window\. You must configure roles and permissions for Maintenance Windows before you begin\. For more information, see [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)\. 

**Topics**
+ [Create a Default Patch Baseline](#sysman-patch-baseline-console)
+ [Add Instances to a Patch Group](#sysman-patch-tagging-console)
+ [Create a Maintenance Window for Patching](#sysman-patch-mw-console)

## Create a Default Patch Baseline<a name="sysman-patch-baseline-console"></a>

Patch Manager includes a default patch baseline for each operating system supported by Patch Manager\. You can use these default patch baselines \(you can't customize them\), or you can create your own\. The following procedure describes how to view the default patch baselines to see if they meet your needs\. The procedure also describes how to create your own default patch baseline\. To learn more about patch baselines, see [Verify Default Patch Baselines or Create a Custom Patch Baseline](sysman-patch-baselines.md)\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To create a default patch baseline \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. In the patch baselines list, choose the name of a patch baseline for the operating system you want to patch\.

1. Choose the **Approval rules** tab\. 

   If the auto\-approval rules are acceptable for your instances, then you can skip to the next procedure, [Add Instances to a Patch Group](#sysman-patch-tagging-console)\. 

   \-or\-

   To create your own default patch baseline, in the navigation pane, choose **Patch Manager**, and then choose **Create patch baseline**\.

1. In the **Name** field, type a name for your new patch baseline, for example, **RHEL\-Default**\.

1. \(Optional\) Type a description for this patch baseline\.

1. In the **Operating system** list, choose an operating system, for example, Red Hat Enterprise Linux\.

1. In the **Approval rules** section, use the fields to create one or more auto\-approval rules\.
   + **Product**: The version of the operating systems the approval rule applies to, such as RedhatEnterpriseLinux7\.4\. The default selection is All\.
   + **Classification**: The type of patches the approval rule applies to, such as Security\. The default selection is All\. 
   + **Severity**: The severity value of patches the rule is to apply to, such as Critical\. The default selection is All\. 
   + **Auto approval delay**: The number of days to wait after a patch is released before a patch is automatically approved\. You can enter any integer from zero \(0\) to 100\.
   + \(Optional\) **Compliance level**: The severity level you want to assign to patches approved by the baseline, such as High\.
**Note**  
If an approved patch is reported as missing, the option you choose in **Compliance level**, such as Critical or Medium, determines the severity of the compliance violation\.
   + \(Linux only\) **Include non\-security updates**: Select the check box to install non\-security patches made available in the source repository, in addition to the security\-related patches\. 
**Note**  
For SUSE Linux Enterprise Server, it isn't necessary to select the check box because patches for security and non\-security issues are installed by default on SLES instances\. For more information, see the content for SLES in [How Security Patches Are Selected](patch-manager-how-it-works-selection.md)\.

   For more information about working with approval rules in a custom patch baseline, see [Custom Baselines](patch-manager-baselines.md#patch-manager-baselines-custom)\.

1. In the **Patch exceptions** section, enter comma\-separated lists of patches you want to explicitly approve and reject for the baseline\. For approved patches, choose a corresponding compliance severity level\. 
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [Package Name Formats for Approved and Rejected Patch Lists](patch-manager-approved-rejected-package-name-formats.md)\.

   If any approved patches you specify aren't related to security, select the **Approved patches include non\-security updates** box for these patches to be installed as well\. Applies to Linux instances only\.

1. \(Optional\) For Linux instances only: If you want to specify alternative patch repositories for different versions of an operating system, such as *AmazonLinux2016\.03* and *AmazonLinux2017\.09*, do the following for each product in the **Patch sources** section:
   + In **Name**, type a name to help you identify the source configuration\.
   + In **Product**, select the version of the operating systems the patch source repository is for, such as RedhatEnterpriseLinux7\.4\.
   + In **Configuration**, enter the value of the yum repository configuration to use\. For example:

     ```
     cachedir=/var/cache/yum/$basesearch
     $releasever
     keepcache=0
     debuglevel=2
     ```

     Choose **Add another source** to specify a source repository for each additional operating system version, up to a maximum of 20\.

     For more information about alternative source patch repositories, see [How to Specify an Alternative Patch Source Repository \(Linux\)](patch-manager-how-it-works-alt-source-repository.md)\.

1. Choose **Create patch baseline**\.

1. In the list of patch baselines, choose the baseline you want to set as the default\.

1. Choose **Actions**, and then choose **Set default patch baseline**\.

1. Verify details in the** Set default patch baseline** confirmation dialog, and then choose **Set default**\.

**To create a default patch baseline \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Services** in the navigation pane, and then choose **Patch Baselines**\. 

1. In the patch baselines list, choose a patch baseline for the operating system you want to patch\.
**Note**  
If the **Welcome to EC2 Systems Manager \- Patch Baselines** page appears, choose **Create Patch Baseline**\. When the **Create patch baseline** page appears, choose the back button in your browser to view the list of patch baselines\.

1. With a default baseline selected, choose the **Approval Rules** tab\. If the auto\-approval rules are acceptable for your instances, then you can skip to the next procedure, [Add Instances to a Patch Group](#sysman-patch-tagging-console)\. 

1. To create your own default patch baseline, choose **Create Patch Baseline**\.

1. In the **Name** field, type a name for your new patch baseline, for example, **RHEL\-Default**\.

1. \(Optional\) Type a description for this patch baseline\.

1. In the **Operating System** field, choose an operating system, for example, RedhatEnterpriseLinux\.

1. In the **Approval Rules** section, use the fields to create one or more auto\-approval rules\.
**Note**  
If an approved patch is reported as missing, the option you choose in **Compliance level**, such as Critical or Medium, determines the severity of the compliance violation\.

1. \(Optional\) In the **Patch Exceptions** section, enter comma\-separated lists of patches you want to explicitly approve and reject for the baseline\. For approved patches, choose a corresponding compliance severity level\. 
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [Package Name Formats for Approved and Rejected Patch Lists](patch-manager-approved-rejected-package-name-formats.md)\.

1. Choose **Create Patch Baseline**, and then choose **Close**\.

1. In the list of patch baselines, choose the baseline you want to set as the default\.

1. Choose **Actions**, and then choose **Set Default Patch Baseline**\.

1. Verify details in the** Set Default Patch Baseline** confirmation dialog, and then choose **Set Default Patch Baseline**\.

## Add Instances to a Patch Group<a name="sysman-patch-tagging-console"></a>

To help you organize your patching efforts, we recommend that you add instances to patch groups by using Amazon EC2 tags\. Patch groups require use of the tag key **Patch Group**\. You can specify any value, but the tag key must be **Patch Group**\. For more information about patch groups, see [Organize Instances into Patch Groups](sysman-patch-patchgroups.md)\.

**To add instances to a patch group**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), and then choose **Instances** in the navigation pane\. 

1. In the list of instances, choose an instance that you want to configure for patching\.

1. From the **Actions** menu, choose **Instance Settings**, **Add/Edit Tags**\.

1. If the instance already has one or more tags applied, choose **Create Tag**\.

1. In the **Key** field, type **Patch Group**\.

1. In the **Value** field, type a value that helps you understand which instances will be patched\.

1. Choose **Save**\.

1. Repeat this procedure to add other instances to the same patch group\.

## Create a Maintenance Window for Patching<a name="sysman-patch-mw-console"></a>

To minimize the impact on your server availability, we recommend that you configure a Maintenance Window to execute patching during times that won't interrupt your business operations\. For more information about Maintenance Windows, see [AWS Systems Manager Maintenance Windows](systems-manager-maintenance.md)\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To create a Maintenance Window for patching \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Maintenance Windows**\.

1. Choose **Create maintenance window**\.

1. In the **Name** field, type a name that designates this as a Maintenance Window for patching critical and important updates\.

1. In the top of the **Schedule** section, choose the schedule options you want\.

1. In the **Duration** field, type the number of hours you want the Maintenance Window to be active\.

1. In the **Stop initiating tasks** field, type the number of hours before the Maintenance Window duration ends that you want the system to stop initiating new tasks\.

1. Choose **Create maintenance window**\.

1. In the Maintenance Windows list, choose the Maintenance Window you just created, and then choose **Actions**, **Register targets**\.

1. \(Optional\) In the **Maintenance window target details** section, provide a name, a description, and owner information \(your name or alias\) for this target\.

1. In the **Targets** section, choose **Specifying tags**\.

1. Under **Tag**, enter a tag key and a tag value to identify the instances to register with the Maintenance Window\.

1. Choose **Register target**\. The system creates a Maintenance Window target\.

1. In the details page of the Maintenance Window you created, choose **Actions**, **Register run command task**\.

1. \(Optional\) In the **Maintenance window task details** section, provide a name and description for this task\.

1. In the **Command document** list, choose **AWS\-RunPatchBaseline**\.

1. In the **Task priority** list, choose a priority\. One is the highest priority\.

1. In the **Targets** section, under **Target by**, choose choose the Maintenance Window target you created earlier in this procedure\.

1. \(Optional\) In **Rate control**:
   + In **Concurrency**, specify either a number or a percentage of instances on which to run the command at the same time\.
**Note**  
If you selected targets by choosing Amazon EC2 tags, and you are not certain how many instances use the selected tags, then limit the number of instances that can run the document at the same time by specifying a percentage\.
   + In **Error threshold**, specify when to stop running the command on other instances after it fails on either a number or a percentage of instances\. For example, if you specify 3 errors, then Systems Manager stops sending the command when the 4th error is received\. Instances still processing the command might also send errors\.

1. In the **Role** section, enter the ARN of a IAM role to which the **AmazonSSMMaintenanceWindowRole** is attached\. For more information, see [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)\.

1. In the **Output options** section, if you want to save the command output to a file, select the **Write command output to an Amazon S3 bucket**\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\. 

1. In the **SNS Notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Configuring Amazon SNS Notifications for Run Command](rc-sns-notifications.md)\.

1. In the **Parameters** section:
   + In the **Operation** list, choose **Scan** to scan for missing patches, or choose **Install** to scan for and install missing patches\.
**Note**  
The **Install** operation causes the instance to reboot \(if patches are installed\)\. The **Scan** operations does not cause a reboot\.
   + You don't need to specify anything in the **Snapshot Id** field\. This system automatically generates and provides this parameter\.
   + \(Optional\) In the **Comment** box, enter a tracking note or reminder about this command\.
   + In the Timeout \(seconds\) box, enter the number of seconds the system should wait for the operation to finish before it is considered unsuccessful\.

1. Choose **Register run command task**\.

**To create a Maintenance Window for patching \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Maintenance Windows**, and then choose **Create maintenance window**\.

1. In the **Name** field, type a name that designates this as a Maintenance Window for patching critical and important updates\.

1. In the **Specify schedule** area, choose the schedule options you want\.

1. In the **Duration** field, type the number of hours you want the Maintenance Window to be active\.

1. In the **Stop initiating tasks** field, type the number of hours before the Maintenance Window duration ends that you want the system to stop initiating new tasks\.

1. Choose **Create maintenance window**\.

1. In the Maintenance Window list, choose the Maintenance Window you just created, and then choose **Actions**, **Register targets**\.

1. \(Optional\) Near the top of the page, specify a name, description, and owner information \(your name or alias\) for this target\.

1. Next to **Select targets by**, choose **Specifying Tags**\.

1. Next to **Tag**, use the lists to choose a tag key and a tag value\.

1. Choose **Register targets**\. The system creates a Maintenance Window target\.

1. In the Maintenance Window list, choose the Maintenance Window you created with the procedure, and then choose **Actions**, **Register run command task**\.

1. In the **Command Document** section of the **Register run command task** page, choose **AWS\-RunPatchBaseline**\.

1. In the **Task Priority** section, specify a priority\. One is the highest priority\.

1. In the **Targets** section, choose **Select**, and then choose the Maintenance Window target you created earlier in this procedure\.

1. In the **Role** field, enter the ARN of a role which has the **AmazonSSMMaintenanceWindowRole** policy attached to it\. For more information, see [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)\.

1. In the **Execute on** field, choose either **Targets** or **Percent** to limit the number of instances where the system can simultaneously perform patching operations\.

1. In the **Stop after** field, specify the number of allowed errors before the system stops sending the patching task to other instances\.

1. In the **Operation** list, choose **Scan** to scan for missing patches, or choose **Install** to scan for and install missing patches\.
**Note**  
The **Install** operation causes the instance to reboot \(if patches are installed\)\. The **Scan** operations does not cause a reboot\.

1. You don't need to specify anything in the **Snapshot Id** field\. This system automatically generates and provides this parameter\.

1. In the **Advanced** section: 
   + If you want to write command output and results to an Amazon S3 bucket, choose **Write to S3**\. Type the bucket and prefix names in the boxes\.
   + If you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\. For more information about configuring Amazon SNS notifications for Run Command, see [Configuring Amazon SNS Notifications for Run Command](rc-sns-notifications.md)\.

1. Choose **Register task**\.

After the Maintenance Window task completes, you can view patch compliance details in the Amazon EC2 console on the **Managed Instances** page\. In the filter bar, use the **AWS:PatchSummary** and **AWS:ComplianceItem** filters\. 

**Note**  
You can save your query by bookmarking the URL after you specify the filters\.

You can also drill down on a specific instance by choosing the instance in the **Managed Instances** page, and then choose the **Patch** tab\. You can also use the [DescribePatchGroupState](http://docs.aws.amazon.com/ssm/latest/APIReference/API_DescribePatchGroupState.html) and [DescribeInstancePatchStatesForPatchGroup](http://docs.aws.amazon.com/ssm/latest/APIReference/API_DescribeInstancePatchStatesForPatchGroup.html) APIs to view compliance details\. For information about patch compliance data, see [About Patch Compliance](sysman-compliance-about.md#sysman-compliance-monitor-patch)\.