# Create a Patching Configuration<a name="create-patching-configuration"></a>

A patching configuration defines a unique patching operation\. The configuration specifies the instances for patching, which patch baseline is to be applied, the schedule for patching, and the Maintenance Window that the configuration is to be associated with\. 

**Note**  
Most patching use cases benefit from patching instances on a schedule with a Maintenance Window, but you can also run a one\-time patching operation manually without a Maintenance Window\.

To minimize the impact on your server availability, we recommend that you configure a Maintenance Window to execute patching during times that won't interrupt your business operations\. For more information about Maintenance Windows, see [AWS Systems Manager Maintenance Windows](systems-manager-maintenance.md)\.

If you plan to add the patching configuration to a Maintenance Window, you must first configure roles and permissions for Maintenance Windows before beginning this procedure\. For more information, see [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)\. 

**To create a patching configuration**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. Choose **Configure patching**\.

1. In the **Instances to patch** section, choose one of the following:
   + **Enter instance tags**: Enter a tag key and optional tag value to specify the tagged instance to patch\. Click **Add** to include additional tagged instances\.
   + **Select a patch group**: Choose the name of an existing patch group that includes the instances you want to patch\.
**Note**  
The **Select a patch group** list displays only those patch groups that are attached to, or registered with, a patch baseline\. You can register a patch group with a patch baseline in one of two ways\. You can use the [register\-patch\-baseline\-for\-patch\-group](https://docs.aws.amazon.com/cli/latest/reference/ssm/register-patch-baseline-for-patch-group.html) CLI command, or you can view a patch baseline in the Systems Manager console and select **Modify patch groups** from the **Actions** menu\.  
Alternatively, to specify an existing patch group that is not registered with the patch baseline, choose **Enter instance tag**, type `Patch Group` as the tag key and the patch group's name as the tag value\.
   + **Select instances manually**: Select the check box next to the name of each instance you want to patch\.

1. In the **Patching schedule** section, choose one of the following:
   + **Select an existing Maintenance Window**: From the list, select a Maintenance Window you have already created, and then continue to step 7\. 
   + **Schedule in a new Maintenance Window**: Create a new Maintenance Window to associate with this patching configuration\.
   + **Skip scheduling and patch now**: Run a one\-time manual patching operation without a schedule or Maintenance Window\. Continue to step 7\.

1. If you chose **Schedule in a new Maintenance Window** in step 5, then under **How do you want to specify a patching schedule?**, do the following:
   + Under **How do you want to specify a Maintenance Window schedule?**, choose a schedule builder or expression option\.
   + Under **Maintenance Window run frequency**, specify how frequently the Maintenance Window runs\. If you are specifying a CRON/Rate expression, see [Reference: Cron and Rate Expressions for Systems Manager](reference-cron-and-rate-expressions.md) for more information\.
   + In the **Maintenance Window duration** box, specify the number of hours the Maintenance Window is permitted to run before timing out\.
   + In the **Maintenance Window name** box, enter a name to identify the Maintenance Window\.

1. In the **Patching operation** area, choose whether to scan instances for missing patches and apply them as needed, or to scan only and generate a list of missing patches\.

1. \(Optional\) In the **Additional settings** area, if any target instances you selected belong to a patch group, you can change the patch baseline that is associated with the patch group\. To do so, follow these steps:

   1. Choose the button beside the name of the associated patch baseline\.

   1. Choose **Change patch baseline registration**\.

   1. Choose the patch baselines you want to specify for this configuration by clearing and selecting check boxes beside the patch baseline names\.

   1. Choose **Close**\.
**Note**  
For any target instances you selected that are not part of a patch group, Patch Manager instead uses the default patch baseline for the operating system type of the instance\.

1. Choose **Configure patching**\.

If you created a new Maintenance Window for this patching configuration, you can add to it or make patching configuration changes in the **Maintenance Window** area of Systems Manager\. For more information, see [Updating or Deleting a Maintenance Window \(Console\)](sysman-maintenance-update.md)\.