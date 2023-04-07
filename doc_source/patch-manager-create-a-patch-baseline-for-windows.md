# Creating a custom patch baseline \(Windows\)<a name="patch-manager-create-a-patch-baseline-for-windows"></a>

Use the following procedure to create a custom patch baseline for Windows managed nodes in Patch Manager, a capability of AWS Systems Manager\. 

For information about creating a patch baseline for Linux managed nodes, see [Creating a custom patch baseline \(Linux\)](patch-manager-create-a-patch-baseline-for-linux.md)\. For information about creating a patch baseline for macOS managed nodes, see [Creating a custom patch baseline \(macOS\)](patch-manager-create-a-patch-baseline-for-macos.md)\.

For an example of creating a patch baseline that is limited to installing Windows Service Packs only, see [Tutorial: Create a patch baseline for installing Windows Service Packs \(console\)](patch-manager-windows-service-pack-patch-baseline-tutorial.md)\.

**To create a custom patch baseline \(Windows\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. Choose **Create patch baseline**\. 

   \-or\-

   If you are accessing Patch Manager for the first time in the current AWS Region, choose **View predefined patch baselines**, and then choose **Create patch baseline**\.

1. For **Name**, enter a name for your new patch baseline, for example, `MyWindowsPatchBaseline`\.

1. \(Optional\) For **Description**, enter a description for this patch baseline\.

1. For **Operating system**, choose `Windows`\.

1. If you want to begin using this patch baseline as the default for Windows as soon as you create it, select **Set this patch baseline as the default patch baseline for Windows Server instances** \.

   If you choose not to set this patch baseline for use now, you can do so later\. For information, see [Setting an existing patch baseline as the default \(console\)](patch-manager-default-patch-baseline.md)\.

1. In the **Approval rules for operating systems** section, use the fields to create one or more auto\-approval rules\.
   + **Product**: The version of the operating systems the approval rule applies to, such as `WindowsServer2012`\. The default selection is `All`\.
   + **Classification**: The type of patches the approval rule applies to, such as `CriticalUpdates`, `Drivers`, and `Tools`\. The default selection is `All`\. 
**Tip**  
You can include Windows Service Pack installations in your approval rules by including `ServicePacks` or by choosing `All` in your **Classification** list\. For an example, see [Tutorial: Create a patch baseline for installing Windows Service Packs \(console\)](patch-manager-windows-service-pack-patch-baseline-tutorial.md)\.
   + **Severity**: The severity value of patches the rule is to apply to, such as `Critical`\. The default selection is `All`\. 
   + **Auto\-approval**: The method for selecting patches for automatic approval\.
     + **Approve patches after a specified number of days**: The number of days for Patch Manager to wait after a patch is released or updated before a patch is automatically approved\. You can enter any integer from zero \(0\) to 360\. For most scenarios, we recommend waiting no more than 100 days\.
     + **Approve patches released up to a specific date**: The patch release date for which Patch Manager automatically applies all patches released or updated on or before that date\. For example, if you specify July 7, 2023, no patches released or last updated on or after July 8, 2023, are installed automatically\.
   + \(Optional\) **Compliance reporting**: The severity level you want to assign to patches approved by the baseline, such as `High`\.
**Note**  
If an approved patch is reported as missing, the option you choose in **Compliance reporting**, such as `Critical` or `Medium`, determines the severity of the compliance violation\.

1. \(Optional\) In the **Approval rules for applications** section, use the fields to create one or more auto\-approval rules\.
**Note**  
Instead of specifying approval rules, you can specify lists of approved and rejected patches as patch exceptions\. See steps 10 and 11\. 
   + **Product family**: The general Microsoft product family for which you want to specify a rule, such as `Office` or `Exchange Server`\.
   + **Product**: The version of the application the approval rule applies to, such as `Office 2016` or `Active Directory Rights Management Services Client 2.0 2016`\. The default selection is `All`\.
   + **Classification**: The type of patches the approval rule applies to, such as `CriticalUpdates`\. The default selection is `All`\. 
   + **Severity**: The severity value of patches the rule applies to, such as `Critical`\. The default selection is `All`\. 
   + **Auto\-approval**: The method for selecting patches for automatic approval\.
     + **Approve patches after a specified number of days**: The number of days for Patch Manager to wait after a patch is released or updated before a patch is automatically approved\. You can enter any integer from zero \(0\) to 360\. For most scenarios, we recommend waiting no more than 100 days\.
     + **Approve patches released up to a specific date**: The patch release date for which Patch Manager automatically applies all patches released or updated on or before that date\. For example, if you specify July 7, 2023, no patches released or last updated on or after July 8, 2023, are installed automatically\.
   + \(Optional\) **Compliance reporting**: The severity level you want to assign to patches approved by the baseline, such as `High`\.
**Note**  
If an approved patch is reported as missing, the option you choose in **Compliance reporting**, such as `Critical` or `Medium`, determines the severity of the compliance violation\.

1. \(Optional\) If you want to explicitly approve any patches instead of letting patches be selected according to approval rules, do the following in the **Patch exceptions** section:
   + For **Approved patches**, enter a comma\-separated list of the patches you want to approve\.
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)\.
   + \(Optional\) For **Approved patches compliance level**, assign a compliance level to the patches in the list\.

1. If you want to explicitly reject any patches that otherwise meet your approval rules, do the following in the **Patch exceptions** section:
   + For **Rejected patches**, enter a comma\-separated list of the patches you want to reject\.
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)\.
   + For **Rejected patches action**, select the action for Patch Manager to take on patches included in the **Rejected patches** list\.
     + **Allow as dependency**: A package in the **Rejected patches** list is installed only if it's a dependency of another package\. It's considered compliant with the patch baseline and its status is reported as *InstalledOther*\. This is the default action if no option is specified\.
     + **Block**: Packages in the **Rejected patches** list, and packages that include them as dependencies, aren't installed under any circumstances\. If a package was installed before it was added to the **Rejected patches** list, it's considered noncompliant with the patch baseline and its status is reported as *InstalledRejected*\.

1. \(Optional\) For **Manage tags**, apply one or more tag key name/value pairs to the patch baseline\.

   Tags are optional metadata that you assign to a resource\. Tags allow you to categorize a resource in different ways, such as by purpose, owner, or environment\. For example, you might want to tag a patch baseline to identify the severity level of patches it specifies, the operating system family it applies to, and the environment type\. In this case, you could specify tags similar to the following key name/value pairs:
   + `Key=PatchSeverity,Value=Critical`
   + `Key=OS,Value=RHEL`
   + `Key=Environment,Value=Production`

1. Choose **Create patch baseline**\.