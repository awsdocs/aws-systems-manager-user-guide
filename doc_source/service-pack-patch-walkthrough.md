# Walkthrough: Create a patch baseline for installing Windows Service Packs \(console\)<a name="service-pack-patch-walkthrough"></a>

When you create a custom patch baseline, you can specify that all, some, or only one type of supported patch is installed\.

In patch baselines for Windows, you can select `ServicePacks` as the only **Classification** option in order to limit patching updates to Service Packs only\. Service Packs can be installed automatically by Patch Manager provided that the update is available in Windows Update or Windows Server Update Services \(WSUS\)\.

You can configure a patch baseline to control whether Service Packs for all Windows versions are installed, or just those for specific versions, such as Windows 7 or Windows Server 2016\. 

Use the following procedure to create a custom patch baseline to be used exclusively for installing all Service Packs on your Windows instances\. 

**To use Patch Manager to install Windows Service Packs \(console\) \(Windows\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. Choose **Create patch baseline**\. 

1. For **Name**, enter a name for your new patch baseline, for example, **MyWindowsServicePackPatchBaseline**\.

1. \(Optional\) For **Description**, enter a description for this patch baseline\.

1. For **Operating system**, choose `Windows`\.

1. If you want to begin using this patch baseline as the default for Windows as soon as you create it, select **Set this patch baseline as the default patch baseline for Windows Server instances** \.

   If you choose not to set this patch baseline for use now, you can do so later\. For information, see [ Setting an existing patch baseline as the default](set-default-patch-baseline.md)\.

1. In the **Approval rules for operating systems** section, use the fields to create one or more auto\-approval rules\.
   + **Product**: The operating system versions that the approval rule applies to, such as `WindowsServer2008`\. You can choose one, more than one, or all supported versions of Windows\. The default selection is `All`\.
   + **Classification**: Choose `ServicePacks`\. 
   + **Severity**: The severity value of patches the rule is to apply to\. To ensure that all Service Packs are included by the rule, choose `All`\. 
   + **Auto\-approval**: The method for selecting patches for automatic approval\.
     + **Approve patches after a specified number of days**: The number of days for Patch Manager to wait after a patch is released before a patch is automatically approved\. You can enter any integer from zero \(0\) to 100\.
     + **Approve patches released up to a specific date**: The patch release date for which Patch Manager automatically applies all patches released on or before that date\. For example, if you specify July 7, 2020, no patches released on or after July 8, 2020, are installed automatically\.
   + \(Optional\) **Compliance reporting**: The severity level you want to assign to Service Packs approved by the baseline, such as `High`\.
**Note**  
If an approved Service Pack is reported as missing, the option you choose in **Compliance reporting**, such as `Critical` or `Medium`, determines the severity of the compliance violation\.

1. \(Optional\) For **Manage tags**, apply one or more tag key name/value pairs to the patch baseline\.

   Tags are optional metadata that you assign to a resource\. Tags enable you to categorize a resource in different ways, such as by purpose, owner, or environment\. For this patch baseline dedicated to updating Service Packs, you could specify key\-value pairs such as the following:
   + `Key=OS,Value=Windows`
   + `Key=Classification,Value=ServicePacks`

1. Choose **Create patch baseline**\.