# Create a Patch Group<a name="sysman-patch-group-tagging"></a>

To help you organize your patching efforts, we recommend that you add instances to patch groups by using tags\. Patch groups require use of the tag key **Patch Group**\. You can specify any value, but the tag key must be **Patch Group**\. For more information about patch groups, see [About Patch Groups](sysman-patch-patchgroups.md)\.

After you group your instances using tags, you must add the patch group value to a patch baseline\. By registering the patch group with a patch baseline, you ensure that the correct patches are installed during the patching operation\. For more information, see the next procedure [Add a Patch Group to a Patch Baseline](sysman-patch-group-patchbaseline.md)\.

## Add EC2 Instances to a Patch Group Using Tags<a name="sysman-patch-group-tagging-ec2"></a>

For EC2 instances, you can add tags by using the AWS Systems Manager console, the Amazon EC2 console, the AWS CLI command `create-tags`, or the API action `CreateTags`\.

**To add EC2 instances to a patch group \(AWS Systems Manager console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Managed Instances**\.

1. In the **Managed instances** list, choose a managed EC2 instance that you want to configure for patching\.

1. Choose **View details**\.

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

**To add EC2 instances to a patch group \(AWS CLI\)**

1. Download the latest version of the [AWS CLI](https://aws.amazon.com/cli) to your local machine\.

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in AWS Systems Manager, or you must have been granted the appropriate permission in AWS Identity and Access Management \(IAM\)\.

   ```
   aws configure
   ```

   The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

1. Run the following command to add the `Patch Group` tag to an EC2 instance\.

   ```
   aws ec2 create-tags --resources "i-1234567890abcdef0" --tags "Key=Patch Group,Value=GroupValue"
   ```

## Add Managed Instances to a Patch Group Using Tags<a name="sysman-patch-group-tagging-managed"></a>

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

**To add managed instances to a patch group \(AWS CLI\)**

1. Download the latest version of the [AWS CLI](https://aws.amazon.com/cli) to your local machine\.

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in AWS Systems Manager, or you must have been granted the appropriate permission in AWS Identity and Access Management \(IAM\)\.

   ```
   aws configure
   ```

   The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

1. Run the following command to add the `Patch Group` tag to a managed instance\.

   ```
   aws ssm add-tags-to-resource --resource-type "ManagedInstance" --resource-id "mi-0123456789abcdefg" --tags "Key=Patch Group,Value=GroupValue"
   ```