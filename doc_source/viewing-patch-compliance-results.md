# Viewing patch compliance results \(console\)<a name="viewing-patch-compliance-results"></a>

Use these procedures to view patch compliance information about your managed nodes\.

This procedure applies to patch operations that use the `AWS-RunPatchBaseline` document\. For information about viewing patch compliance information for patch operations that use the `AWS-RunPatchBaselineAssociation` document, see [Identifying out\-of\-compliance managed nodes](patch-compliance-identify.md)\.

**Note**  
The patch scan processes for Quick Setup and Explorer, AWS Systems Manager capabilities, use the `AWS-RunPatchBaselineAssociation` document\.

**Identify the patch solution for a specific CVE issue \(Linux\)**  
For many Linux\-based operating systems, patch compliance results indicate which Common Vulnerabilities and Exposure \(CVE\) bulletin issues are resolved by which patches\. This information can help you determine how urgently you need to install a missing or failed patch\.

CVE details are included for supported versions of the following operating system types:
+ Amazon Linux
+ Amazon Linux 2
+ Amazon Linux 2022
+ Oracle Linux
+ Red Hat Enterprise Linux \(RHEL\)
+ Rocky Linux
+ SUSE Linux Enterprise Server \(SLES\)

**Note**  
By default, CentOS and CentOS Stream don't provide CVE information about updates\. You can, however, allow this support by using third\-party repositories such as the Extra Packages for Enterprise Linux \(EPEL\) repository published by Fedora\. For information, see [EPEL](https://fedoraproject.org/wiki/EPEL) on the Fedora Wiki\.

You can also add CVE IDs to your lists of approved or rejected patches in your patch baselines, as the situation and your patching goals warrant\.

For information about working with approved and rejected patch lists, see the following topics:
+ [Working with custom patch baselines \(console\)](sysman-patch-baseline-console.md)
+ [About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)
+ [How patch baseline rules work on Linux\-based systems](patch-manager-how-it-works-linux-rules.md)
+ [How patches are installed](patch-manager-how-it-works-installation.md)

**Note**  
In some cases, Microsoft releases patches for applications that might not specify explicitly an updated date and time\. In these cases, an updated date and time of `01/01/1970` is supplied by default\.

## Viewing patching compliance results \(console\)<a name="viewing-patch-compliance-results-console"></a>

Use the following procedures to view patch compliance results in the AWS Systems Manager console\. 

**Note**  
For information about generating patch compliance reports that are downloaded to an Amazon Simple Storage Service \(Amazon S3\) bucket, see [Generating \.csv patch compliance reports \(console\)](patch-compliance-reports-to-s3.md)\.

**To view patch compliance results**

1. Do one of the following\.

   **Option 1** \(recommended\) – Navigate from Patch Manager, a capability of AWS Systems Manager:
   + In the navigation pane, choose **Patch Manager**\.

     \-or\-

     If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.
   + Choose the **Compliance reporting** tab\.
   + For **Instance patching summary**, choose the ID of the managed node for which you want to review patch compliance results\.
   + Choose **View details**\.

   **Option 2** – Navigate from Compliance, a capability of AWS Systems Manager:
   + In the navigation pane, choose **Compliance**\.

     \-or\-

     If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Compliance** in the navigation pane\.
   + For **Compliance resources summary**, choose a number in the column for the types of patch resources you want to review, such as **Non\-Compliant resources**\.
   + In the **Resource** list, choose the ID of the managed node for which you want to review patch compliance results\.

   **Option 3** – Navigate from Fleet Manager, a capability of AWS Systems Manager\.
   + In the navigation pane, choose **Fleet Manager**\.

     \-or\-

     If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.
   + On the **Managed instances** tab, choose the ID of the managed node for which you want to review patch compliance results\.

1. Choose the **Patch** tab\.

1. \(Optional\) In the Search box \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/search-icon.png)\), choose from the available filters\.

   For example, for Red Hat Enterprise Linux \(RHEL\), choose from the following:
   + Name
   + Classification
   + State
   + Severity

    For Windows Server, choose from the following:
   + KB
   + Classification
   + State
   + Severity

1. Choose one of the available values for the filter type you chose\. For example, if you chose **State**, now choose a compliance state such as **InstalledPendingReboot**, **Failed** or **Missing**\.

1. Depending on the compliance state of the managed node, you can choose what action to take to remedy any noncompliant nodes\.

   For example, you can choose to patch your noncompliant managed nodes immediately\. For information about patching your managed nodes on demand, see [Patching managed nodes on demand \(console\)](patch-on-demand.md)\.

   For information about patch compliance states, see [Understanding patch compliance state values](about-patch-compliance-states.md)\.