# Creating a custom patch baseline \(macOS\)<a name="create-baseline-console-macos"></a>

Use the following procedure to create a custom patch baseline for macOS managed nodes in Patch Manager, a capability of AWS Systems Manager\. 

For information about creating a patch baseline for Windows Server managed nodes, see [Creating a custom patch baseline \(Windows\)](create-baseline-console-windows.md)\. For information about creating a patch baseline for Linux managed nodes, see [Creating a custom patch baseline \(Linux\)](create-baseline-console-linux.md)\. 

**To create a custom patch baseline for macOS managed nodes**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. Choose **Create patch baseline**\.

   \-or\-

   If you are accessing Patch Manager for the first time in the current AWS Region, choose **View predefined patch baselines**, and then choose **Create patch baseline**\.

1. For **Name**, enter a name for your new patch baseline, for example, `MymacOSPatchBaseline`\.

1. \(Optional\) For **Description**, enter a description for this patch baseline\.

1. For **Operating system**, choose macOS\.

1. If you want to begin using this patch baseline as the default for macOS as soon as you create it, check the box next to **Set this patch baseline as the default patch baseline for macOS instances**\.

   For information about setting an existing patch baseline as the default, see [Setting an existing patch baseline as the default \(console\)](set-default-patch-baseline.md)\.

1. In the **Approval rules for operating\-systems** section, use the fields to create one or more auto\-approval rules\.
   + **Product**: The version of the operating systems the approval rule applies to, such as `Mojave10.14.1` or `Catalina10.15.1`\. The default selection is `All`\.
   + **Classification**: The package manager or package managers that you want to apply packages during the patching process\. You can choose from the following:
     + softwareupdate
     + installer
     + brew
     + brew cask

     The default selection is `All`\. 
   + \(Optional\) **Compliance reporting**: The severity level you want to assign to patches approved by the baseline, such as `High`\.
**Note**  
If an approved patch is reported as missing, the option you choose in **Compliance reporting**, such as `Critical` or `Medium`, determines the severity of the compliance violation\.
   + **Include non\-security updates**: Select the check box to install nonsecurity operating system patches made available in the source repository, in addition to the security\-related patches\. 

   For more information about working with approval rules in a custom patch baseline, see [About custom baselines](sysman-patch-baselines.md#patch-manager-baselines-custom)\.

1. If you want to explicitly approve any patches in addition to those meeting your approval rules, do the following in the **Patch exceptions** section:
   + For **Approved patches**, enter a comma\-separated list of the patches you want to approve\.
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)\.
   + \(Optional\) For **Approved patches compliance level**, assign a compliance level to the patches in the list\.
   + If any approved patches you specify aren't related to security, select the **Approved patches include non\-security updates** check box for these patches to be installed on your macOS operating system as well\.

1. If you want to explicitly reject any patches that otherwise meet your approval rules, do the following in the **Patch exceptions** section:
   + For **Rejected patches**, enter a comma\-separated list of the patches you want to reject\.
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)\.
   + For **Rejected patches action**, select the action for Patch Manager to take on patches included in the **Rejected patches** list\.
     + **Allow as dependency**: A package in the **Rejected patches** list is installed only if it's a dependency of another package\. It's considered compliant with the patch baseline and its status is reported as *InstalledOther*\. This is the default action if no option is specified\.
     + **Block**: Packages in the **Rejected patches** list, and packages that include them as dependencies, aren't installed under any circumstances\. If a package was installed before it was added to the **Rejected patches** list, it's considered noncompliant with the patch baseline and its status is reported as *InstalledRejected*\.

1. \(Optional\) For **Manage tags**, apply one or more tag key name/value pairs to the patch baseline\.

   Tags are optional metadata that you assign to a resource\. Tags allow you to categorize a resource in different ways, such as by purpose, owner, or environment\. For example, you might want to tag a patch baseline to identify the severity level of patches it specifies, the package manager it applies to, and the environment type\. In this case, you could specify tags similar to the following key name/value pairs:
   + `Key=PatchSeverity,Value=Critical`
   + `Key=PackageManager,Value=softwareupdate`
   + `Key=Environment,Value=Production`

1. Choose **Create patch baseline**\.