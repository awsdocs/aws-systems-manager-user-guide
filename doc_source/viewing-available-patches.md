# Viewing available patches \(console\)<a name="viewing-available-patches"></a>

With Patch Manager, a capability of AWS Systems Manager, you can view all available patches for a specified operating system and, optionally, a specific operating system version\.

**Tip**  
To generate a list of available patches and save them to a file, you can use the [describe\-available\-patches](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-available-patches.html) command and specify your preferred [output](https://docs.aws.amazon.com/cli/latest/reference/cli-usage-output.html)\.

**To view available patches**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. Choose the **Patches** tab\.

1. For **Operating system**, choose the operating system for which you want to view available patches, such as `Windows` or `Amazon Linux`\.

1. \(Optional\) For **Product**, choose an OS version, such as `WindowsServer2019` or `AmazonLinux2018.03`\.

1. \(Optional\) To add or remove information columns for your results, choose the configure button \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/configure-button.png)\) at the top right of the **Patches** list\. \(By default, the **Patches** tab displays columns for only some of the available patch metadata\.\)

   For information about the types of metadata you can add to your view, see [Patch](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_Patch.html) in the *AWS Systems Manager API Reference*\.