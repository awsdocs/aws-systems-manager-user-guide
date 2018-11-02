# Default and Custom Patch Baselines<a name="sysman-patch-baselines"></a>

A patch baseline defines which patches are approved for installation on your instances\. You can specify approved or rejected patches one by one\. You can also create auto\-approval rules to specify that certain types of updates \(for example, critical updates\) should be automatically approved\. The rejected list overrides both the rules and the approve list\. 

To use a list of approved patches to install specific packages, you first remove all auto\-approval rules\. If you explicitly identify a patch as rejected, it will not be approved or installed, even if it matches all of the criteria in an auto\-approval rule\. Also, a patch is installed on an instance only if it applies to the software on the instance, even if the patch has otherwise been approved for the instance\.

Patch Manager provides pre\-defined patch baselines for each of the operating systems supported by Patch Manager\. You can use these baselines as they are currently configured \(you can't customize them\) or you can create your own patch baselines if you want greater control over which patches are approved or rejected for your environment\. 

**Topics**
+ [Pre\-Defined Baselines](#patch-manager-baselines-pre-defined)
+ [Custom Baselines](#patch-manager-baselines-custom)

## Pre\-Defined Baselines<a name="patch-manager-baselines-pre-defined"></a>

The following table describes the pre\-defined patch baselines provided with Patch Manager\.


****  

| Name | Supported Products | Details | 
| --- | --- | --- | 
|  AWS\-DefaultPatchBaseline  |  Windows \(Windows Server 2008 – 2016\)  |  Approves all operating system patches with a classification of "CriticalUpdates" or "SecurityUpdates" and an MSRC severity of "Critical" or "Important" seven days after release\.  | 
|  AWS\-AmazonLinuxDefaultPatchBaseline  |  Amazon Linux \(2012\.03 – 2018\.03\) and Amazon Linux 2 2\-2\.0  |  Approves all operating system patches with a classification of "Security" and severity of "Critical" or "Important" seven days after release\. Also approves all patches with a classification of "Bugfix" seven days after release  | 
|  AWS\-UbuntuDefaultPatchBaseline  |  Ubuntu Server \(14\.04/16\.04\)  |  Immediately approves all operating system security\-related patches with a priority of "Required" or "Important"\. There is no wait before approval because reliable release dates are not available in the repos\.  | 
|  AWS\-RedHatDefaultPatchBaseline  |  Redhat Enterprise LinuxRed Hat Enterprise Linux \(6\.5, 6\.6, 6\.7, 6\.8, 6\.9, 7\.0, 7\.1, 7\.2, 7\.3\)   |  Approves all operating system patches with a classification of "Security" and severity of "Critical" or "Important" seven days after release\. Also approves all patches with a classification of "Bugfix" seven days after release\.  | 
| AWS\-SuseDefaultPatchBaseline | SUSE Linux Enterprise Server 12 | Approves all operating system patches with a classification of "Security" and a severity of "Critical" or "Important" seven days after release\.  | 
| AWS\-CentOSDefaultPatchBaseline | CentOS 6\.5 and later | Approves all updates 7 days after they become available \(including non\-security updates\)\. | 

## Custom Baselines<a name="patch-manager-baselines-custom"></a>

If you create your own patch baseline, you can choose which patches to auto\-approve by using the following categories\.
+ Operating system: Windows, Amazon Linux, Ubuntu Server, etc\.
+ Product name: For example, RHEL 6\.5, Amazon Linux 2014\.09, Windows Server 2012, Windows Server 2012 R2, etc\.
+ Classification: For example, critical updates, security updates, etc\.
+ Severity: For example, critical, important, etc\.

For each auto\-approval rule that you create, you can specify an auto\-approval delay\. This delay is the number of days to wait after the patch was released, before the patch is automatically approved for patching\. For example, if you create a rule using the Critical Updates classification and configure it for seven days auto\-approval delay, then a new critical patch released on January 7 will automatically be approved on January 14\.

**Note**  
If a Linux repository doesn’t provide release date information for packages, Systems Manager treats the Auto Approval Delay as having a value of zero\.

You can also specify a compliance severity level\. If an approved patch is reported as missing, `Compliance Level` is the severity of the compliance violation\. 

By using multiple patch baselines with different auto\-approval delays, you can deploy patches at different rates to different instances\. For example, you can create separate patch baselines and auto\-approval delays for development and production environments\. This enables you to test patches in your development environment before they get deployed in your production environment\. 

Keep the following in mind when you create a patch baseline:
+ Patch Manager provides a default patch baseline for each supported operating system\. You can instead create your own patch baseline and designate that as the default patch baseline for the corresponding operating system\.
+ For on\-premises or non\-Amazon EC2 instances, Patch Manager attempts to use your custom default patch baseline\. If no custom default patch baseline exists, the system uses the pre\-defined patch baseline for the corresponding operating system\.
+ If a patch is listed as both approved and rejected in the same patch baseline, the patch is rejected\.
+ An instance can have only one patch baseline defined for it\.
+ The formats of package names you can add to lists of approved patches and rejected patches for a patch baseline depend on the type of operating system you are patching\.

  For information about accepted formats for lists of approved patches and rejected patches, see [About Package Name Formats for Approved and Rejected Patch Lists](patch-manager-approved-rejected-package-name-formats.md)\.

For information about creating a patch baseline, see [Create a Default Patch Baseline](sysman-patch-baseline-console.md) and [Tutorial: Patch a Server Environment \(AWS CLI\)](sysman-patch-cliwalk.md)\.