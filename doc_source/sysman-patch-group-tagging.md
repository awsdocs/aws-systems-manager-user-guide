# Creating a patch group<a name="sysman-patch-group-tagging"></a>

To help you organize your patching efforts, we recommend that you add instances to patch groups by using tags\. Patch groups require use of the tag key **Patch Group**\. You can specify any tag value, but the tag key must be **Patch Group**\. For more information about patch groups, see [About patch groups](sysman-patch-patchgroups.md)\.

After you group your instances using tags, you add the patch group value to a patch baseline\. By registering the patch group with a patch baseline, you ensure that the correct patches are installed during the patching operation\. 

**Topics**
+ [Task 1: Add EC2 instances to a patch group using tags](#sysman-patch-group-tagging-ec2)
+ [Task 2: Add managed instances to a patch group using tags](#sysman-patch-group-tagging-managed)
+ [Task 3: Add a patch group to a patch baseline](#sysman-patch-group-patchbaseline)

## Task 1: Add EC2 instances to a patch group using tags<a name="sysman-patch-group-tagging-ec2"></a>

For EC2 instances, you can add tags by using the AWS Systems Manager console, the Amazon EC2 console, the AWS CLI command `create-tags`, or the API action `CreateTags`\.

**Note**  
When using the Amazon EC2 console and AWS CLI, it's possible to apply `Key = Patch Group` tags to instances that aren't yet configured for use with Systems Manager\. Ensure that SSM Agent is installed and running on instances that you want to manage using Systems Manager\. For more information, see [Working with SSM Agent](ssm-agent.md)\.

**To add EC2 instances to a patch group \(AWS Systems Manager console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Managed Instances**\.

1. In the **Managed instances** list, choose a the ID of a managed EC2 instance that you want to configure for patching\.

1. Select the **Tags** tab, then choose **Edit**\.

1. In the left column, type **Patch Group**\.

1. In the right column, enter a value that helps you understand which instances will be patched\.

1. Choose **Save**\.

1. Repeat this procedure to add other managed instances to the same patch group\.

**To add EC2 instances to a patch group \(Amazon EC2 console\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), and then choose **Instances** in the navigation pane\. 

1. In the list of instances, choose an instance that you want to configure for patching\.

1. In the **Actions** menu, choose **Instance Settings**, **Add/Edit Tags**\.

1. If the instance already has one or more tags applied, choose **Create Tag**\.

1. For **Key**, type **Patch Group**\.

1. For **Value**, enter a value that helps you understand which instances will be patched\.

1. Choose **Save**\.

1. Repeat this procedure to add other instances to the same patch group\.

## Task 2: Add managed instances to a patch group using tags<a name="sysman-patch-group-tagging-managed"></a>

For hybrid managed instances \(mi\-\*\), you can add tags by using the AWS Systems Manager console, the AWS CLI command `add-tags-to-resource`, or the API action `AddTagsToResource`\. You cannot add tags for hybrid managed instances using the Amazon EC2 console\.

**To add managed instances to a patch group \(AWS Systems Manager console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Managed Instances**\.

1. In the **Managed instances** list, choose a managed instance that you want to configure for patching\.

1. Choose **View details**\.

1. Select the **Tags** tab, then choose **Edit**\.

1. In the left column, type **Patch Group**\.

1. In the right column, enter a value that helps you understand which instances will be patched\.

1. Choose **Save**\.

1. Repeat this procedure to add other managed instances to the same patch group\.

## Task 3: Add a patch group to a patch baseline<a name="sysman-patch-group-patchbaseline"></a>

To associate a specific patch baseline with your instances, you must add the patch group value to the patch baseline\. By registering the patch group with a patch baseline, you can ensure that the correct patches are installed during a patching operation\. For more information about patch groups, see [About patch groups](sysman-patch-patchgroups.md)\.

**To add a patch group to a patch baseline \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

1. In the **Patch Baselines** list, choose the patch baseline you want to configure for your patch group\.

1. Choose **Actions**, then **Modify patch groups**\.

1. Enter the tag value you added to your managed instances in the previous section, then choose **Add**\.