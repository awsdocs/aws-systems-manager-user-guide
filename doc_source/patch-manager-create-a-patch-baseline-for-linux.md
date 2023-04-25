# Creating a custom patch baseline \(Linux\)<a name="patch-manager-create-a-patch-baseline-for-linux"></a>

Use the following procedure to create a custom patch baseline for Linux managed nodes in Patch Manager, a capability of AWS Systems Manager\. 

For information about creating a patch baseline for macOS managed nodes, see [Creating a custom patch baseline \(macOS\)](patch-manager-create-a-patch-baseline-for-macos.md)\. For information about creating a patch baseline for Windows managed nodes, see [Creating a custom patch baseline \(Windows\)](patch-manager-create-a-patch-baseline-for-windows.md)\.

**To create a custom patch baseline for Linux managed nodes**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. Choose **Create patch baseline**\.

   \-or\-

   If you are accessing Patch Manager for the first time in the current AWS Region, choose **View predefined patch baselines**, and then choose **Create patch baseline**\.

1. For **Name**, enter a name for your new patch baseline, for example, `MyRHELPatchBaseline`\.

1. \(Optional\) For **Description**, enter a description for this patch baseline\.

1. For **Operating system**, choose an operating system, for example, `Red Hat Enterprise Linux`\.

1. If you want to begin using this patch baseline as the default for the selected operating system as soon as you create it, check the box next to **Set this patch baseline as the default patch baseline for *operating system name* instances**\.

   For information about setting an existing patch baseline as the default, see [Setting an existing patch baseline as the default \(console\)](patch-manager-default-patch-baseline.md)\.

1. In the **Approval rules for operating\-systems** section, use the fields to create one or more auto\-approval rules\.
   + **Product**: The version of the operating systems the approval rule applies to, such as `RedhatEnterpriseLinux7.4`\. The default selection is `All`\.
   + **Classification**: The type of patches the approval rule applies to, such as `Security` or `Enhancement`\. The default selection is `All`\. 
**Tip**  
You can configure a patch baseline to control whether minor version upgrades for Linux are installed, such as RHEL 7\.8\. Minor version upgrades can be installed automatically by Patch Manager provided that the update is available in the appropriate repository\.  
For Linux operating systems, minor version upgrades aren't classified consistently\. They can be classified as bug fixes or security updates, or not classified, even within the same kernel version\. Here are a few options for controlling whether a patch baseline installs them\.   
**Option 1**: The broadest approval rule to ensure minor version upgrades are installed when available is to specify **Classification** as `All` \(\*\) and choose the **Include nonsecurity updates** option\.
**Option 2**: To ensure patches for an operating system version are installed, you can use a wildcard \(\*\) to specify its kernel format in the **Patch exceptions** section of the baseline\. For example, the kernel format for RHEL 7\.\* is `kernel-3.10.0-*.el7.x86_64`\.  
Enter `kernel-3.10.0-*.el7.x86_64` in the **Approved patches** list in your patch baseline to ensure all patches, including minor version upgrades, are applied to your RHEL 7\.\* managed nodes\. \(If you know the exact package name of a minor version patch, you can enter that instead\.\)
**Option 3**: You can have the most control over which patches are applied to your managed nodes, including minor version upgrades, by using the [InstallOverrideList](patch-manager-aws-runpatchbaseline.md#patch-manager-aws-runpatchbaseline-parameters-installoverridelist) parameter in the `AWS-RunPatchBaseline` document\. For more information, see [About the `AWS-RunPatchBaseline` SSM document](patch-manager-aws-runpatchbaseline.md)\.
   + **Severity**: The severity value of patches the rule is to apply to, such as `Critical`\. The default selection is `All`\. 
   + **Auto\-approval**: The method for selecting patches for automatic approval\.
**Note**  
Because it's not possible to reliably determine the release dates of update packages for Ubuntu Server, the auto\-approval options aren't supported for this operating system\.
     + **Approve patches after a specified number of days**: The number of days for Patch Manager to wait after a patch is released or last updated before a patch is automatically approved\. You can enter any integer from zero \(0\) to 360\. For most scenarios, we recommend waiting no more than 100 days\.
     + **Approve patches released up to a specific date**: The patch release date for which Patch Manager automatically applies all patches released or updated on or before that date\. For example, if you specify July 7, 2023, no patches released or last updated on or after July 8, 2023, are installed automatically\.
   + \(Optional\) **Compliance reporting**: The severity level you want to assign to patches approved by the baseline, such as `Critical` or `High`\.
**Note**  
If you specify a compliance reporting level and the patch state of any approved patch is reported as `Missing`, then the patch baseline's overall reported compliance severity is the severity level you specified\.
   + **Include non\-security updates**: Select the check box to install nonsecurity Linux operating system patches made available in the source repository, in addition to the security\-related patches\. 
**Note**  
For SUSE Linux Enterprise Server, \(SLES\) it isn't necessary to select the check box because patches for security and nonsecurity issues are installed by default on SLES managed nodes\. For more information, see the content for SLES in [How security patches are selected](patch-manager-selecting-patches.md)\.

   For more information about working with approval rules in a custom patch baseline, see [About custom baselines](patch-manager-predefined-and-custom-patch-baselines.md#patch-manager-baselines-custom)\.

1. If you want to explicitly approve any patches in addition to those meeting your approval rules, do the following in the **Patch exceptions** section:
   + For **Approved patches**, enter a comma\-separated list of the patches you want to approve\.
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)\.
   + \(Optional\) For **Approved patches compliance level**, assign a compliance level to the patches in the list\.
   + If any approved patches you specify aren't related to security, select the **Approved patches include non\-security updates** check box for these patches to be installed on your Linux operating system as well\.

1. If you want to explicitly reject any patches that otherwise meet your approval rules, do the following in the **Patch exceptions** section:
   + For **Rejected patches**, enter a comma\-separated list of the patches you want to reject\.
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)\.
   + For **Rejected patches action**, select the action for Patch Manager to take on patches included in the **Rejected patches** list\.
     + **Allow as dependency**: A package in the **Rejected patches** list is installed only if it's a dependency of another package\. It's considered compliant with the patch baseline and its status is reported as *InstalledOther*\. This is the default action if no option is specified\.
     + **Block**: Packages in the **Rejected patches** list, and packages that include them as dependencies, aren't installed under any circumstances\. If a package was installed before it was added to the **Rejected patches** list, it's considered noncompliant with the patch baseline and its status is reported as *InstalledRejected*\.

1. \(Optional\) If you want to specify alternative patch repositories for different versions of an operating system, such as *AmazonLinux2016\.03* and *AmazonLinux2017\.09*, do the following for each product in the **Patch sources** section:
   + In **Name**, enter a name to help you identify the source configuration\.
   + In **Product**, select the version of the operating systems the patch source repository is for, such as `RedhatEnterpriseLinux7.4`\.
   + In **Configuration**, enter the value of the yum repository configuration to use in the following format:

     ```
     [main]
     name=MyCustomRepository
     baseurl=https://my-custom-repository
     enabled=1
     ```
**Tip**  
For information about other options available for your yum repository configuration, see [dnf\.conf\(5\)](https://man7.org/linux/man-pages/man5/dnf.conf.5.html)\.

     Choose **Add another source** to specify a source repository for each additional operating system version, up to a maximum of 20\.

     For more information about alternative source patch repositories, see [How to specify an alternative patch source repository \(Linux\)](patch-manager-alternative-source-repository.md)\.

1. \(Optional\) For **Manage tags**, apply one or more tag key name/value pairs to the patch baseline\.

   Tags are optional metadata that you assign to a resource\. Tags allow you to categorize a resource in different ways, such as by purpose, owner, or environment\. For example, you might want to tag a patch baseline to identify the severity level of patches it specifies, the operating system family it applies to, and the environment type\. In this case, you could specify tags similar to the following key name/value pairs:
   + `Key=PatchSeverity,Value=Critical`
   + `Key=OS,Value=RHEL`
   + `Key=Environment,Value=Production`

1. Choose **Create patch baseline**\.