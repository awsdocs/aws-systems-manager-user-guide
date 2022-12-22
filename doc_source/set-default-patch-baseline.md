# Setting an existing patch baseline as the default \(console\)<a name="set-default-patch-baseline"></a>

When you create a custom patch baseline in Patch Manager, a capability of AWS Systems Manager, you can set the baseline as the default for the associated operating system type as soon as you create it\. For information, see [Working with custom patch baselines \(console\)](sysman-patch-baseline-console.md)\.

You can also set an existing patch baseline as the default for an operating system type\.

**Important**  
Any default patch baseline selections you make here do not apply to patching operations that are based on a patch policy\. Patch policies use their own patch baseline specifications\. For more information about patch policies, see [Introducing patch policies](patch-policies-about.md)\.

**To set a patch baseline as the default**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. In the patch baselines list, choose the button of a patch baseline that isn't currently set as the default for an operating system type\.

   \-or\-

   If you are accessing Patch Manager for the first time in the current AWS Region, choose **View predefined patch baselines**, and then choose the button of a patch baseline that isn't currently set as the default for an operating system type\.
**Tip**  
The **Default baseline** column indicates which baselines are currently set as the defaults\.

1. In the **Actions** menu, choose **Set default patch baseline**\.

1. In the confirmation dialog box, choose **Set default**\.