# Create a Patch Group<a name="sysman-patch-tagging-console"></a>

To help you organize your patching efforts, we recommend that you add instances to patch groups by using Amazon EC2 tags\. Patch groups require use of the tag key **Patch Group**\. You can specify any value, but the tag key must be **Patch Group**\. For more information about patch groups, see [About Patch Groups](sysman-patch-patchgroups.md)\.

**To add instances to a patch group**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), and then choose **Instances** in the navigation pane\. 

1. In the list of instances, choose an instance that you want to configure for patching\.

1. From the **Actions** menu, choose **Instance Settings**, **Add/Edit Tags**\.

1. If the instance already has one or more tags applied, choose **Create Tag**\.

1. In the **Key** field, type **Patch Group**\.

1. In the **Value** field, enter a value that helps you understand which instances will be patched\.

1. Choose **Save**\.

1. Repeat this procedure to add other instances to the same patch group\.