# Create a Default Patch Baseline<a name="sysman-patch-baseline-console"></a>

Patch Manager includes a default patch baseline for each operating system supported by Patch Manager\. You can use these default patch baselines \(you can't customize them\), or you can create your own\. The following procedure describes how to view the default patch baselines to see if they meet your needs\. The procedure also describes how to create your own default patch baseline\. To learn more about patch baselines, see [Default and Custom Patch Baselines](sysman-patch-baselines.md)\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To create a default patch baseline \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. In the patch baselines list, choose the name of a patch baseline for the operating system you want to patch\.

1. Choose the **Approval rules** tab\. 

   If the auto\-approval rules are acceptable for your instances, then you can skip to the next procedure, [Create a Patch Group](sysman-patch-tagging-console.md)\. 

   \-or\-

   To create your own default patch baseline, in the navigation pane, choose **Patch Manager**, and then choose **Create patch baseline**\.

1. In the **Name** field, enter a name for your new patch baseline, for example, **RHEL\-Default**\.

1. \(Optional\) Enter a description for this patch baseline\.

1. In the **Operating system** list, choose an operating system, for example, Red Hat Enterprise Linux\.

1. In the **Approval rules** section, use the fields to create one or more auto\-approval rules\.
   + **Product**: The version of the operating systems the approval rule applies to, such as RedhatEnterpriseLinux7\.4\. The default selection is All\.
   + **Classification**: The type of patches the approval rule applies to, such as Security\. The default selection is All\. 
   + **Severity**: The severity value of patches the rule is to apply to, such as Critical\. The default selection is All\. 
   + **Auto approval delay**: The number of days to wait after a patch is released before a patch is automatically approved\. You can enter any integer from zero \(0\) to 100\.
   + \(Optional\) **Compliance level**: The severity level you want to assign to patches approved by the baseline, such as High\.
**Note**  
If an approved patch is reported as missing, the option you choose in **Compliance level**, such as Critical or Medium, determines the severity of the compliance violation\.
   + \(Linux only\) **Include non\-security updates**: Select the check box to install non\-security patches made available in the source repository, in addition to the security\-related patches\. 
**Note**  
For SUSE Linux Enterprise Server, it isn't necessary to select the check box because patches for security and non\-security issues are installed by default on SLES instances\. For more information, see the content for SLES in [How Security Patches Are Selected](patch-manager-how-it-works-selection.md)\.

   For more information about working with approval rules in a custom patch baseline, see [Custom Baselines](sysman-patch-baselines.md#patch-manager-baselines-custom)\.

1. In the **Patch exceptions** section, enter comma\-separated lists of patches you want to explicitly approve and reject for the baseline\. For approved patches, choose a corresponding compliance severity level\. 
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About Package Name Formats for Approved and Rejected Patch Lists](patch-manager-approved-rejected-package-name-formats.md)\.

   If any approved patches you specify aren't related to security, select the **Approved patches include non\-security updates** box for these patches to be installed as well\. Applies to Linux instances only\.

1. \(Optional\) For Linux instances only: If you want to specify alternative patch repositories for different versions of an operating system, such as *AmazonLinux2016\.03* and *AmazonLinux2017\.09*, do the following for each product in the **Patch sources** section:
   + In **Name**, enter a name to help you identify the source configuration\.
   + In **Product**, select the version of the operating systems the patch source repository is for, such as RedhatEnterpriseLinux7\.4\.
   + In **Configuration**, enter the value of the yum repository configuration to use\. For example:

     ```
     cachedir=/var/cache/yum/$basesearch
     $releasever
     keepcache=0
     debuglevel=2
     ```

     Choose **Add another source** to specify a source repository for each additional operating system version, up to a maximum of 20\.

     For more information about alternative source patch repositories, see [How to Specify an Alternative Patch Source Repository \(Linux\)](patch-manager-how-it-works-alt-source-repository.md)\.

1. Choose **Create patch baseline**\.

1. In the list of patch baselines, choose the baseline you want to set as the default\.

1. Choose **Actions**, and then choose **Set default patch baseline**\.

1. Verify details in the** Set default patch baseline** confirmation dialog, and then choose **Set default**\.

**To create a default patch baseline \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Services** in the navigation pane, and then choose **Patch Baselines**\. 

1. In the patch baselines list, choose a patch baseline for the operating system you want to patch\.
**Note**  
If the **Welcome to EC2 Systems Manager \- Patch Baselines** page appears, choose **Create Patch Baseline**\. When the **Create patch baseline** page appears, choose the back button in your browser to view the list of patch baselines\.

1. With a default baseline selected, choose the **Approval Rules** tab\. If the auto\-approval rules are acceptable for your instances, then you can skip to the next procedure, [Create a Patch Group](sysman-patch-tagging-console.md)\. 

1. To create your own default patch baseline, choose **Create Patch Baseline**\.

1. In the **Name** field, enter a name for your new patch baseline, for example, **RHEL\-Default**\.

1. \(Optional\) Enter a description for this patch baseline\.

1. In the **Operating System** field, choose an operating system, for example, RedhatEnterpriseLinux\.

1. In the **Approval Rules** section, use the fields to create one or more auto\-approval rules\.
**Note**  
If an approved patch is reported as missing, the option you choose in **Compliance level**, such as Critical or Medium, determines the severity of the compliance violation\.

1. \(Optional\) In the **Patch Exceptions** section, enter comma\-separated lists of patches you want to explicitly approve and reject for the baseline\. For approved patches, choose a corresponding compliance severity level\. 
**Note**  
For information about accepted formats for lists of approved patches and rejected patches, see [About Package Name Formats for Approved and Rejected Patch Lists](patch-manager-approved-rejected-package-name-formats.md)\.

1. Choose **Create Patch Baseline**, and then choose **Close**\.

1. In the list of patch baselines, choose the baseline you want to set as the default\.

1. Choose **Actions**, and then choose **Set Default Patch Baseline**\.

1. Verify details in the** Set Default Patch Baseline** confirmation dialog, and then choose **Set Default Patch Baseline**\.