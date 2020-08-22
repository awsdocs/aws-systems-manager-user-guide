# About predefined and custom patch baselines<a name="sysman-patch-baselines"></a>

A patch baseline defines which patches are approved for installation on your instances\. You can specify approved or rejected patches one by one\. You can also create auto\-approval rules to specify that certain types of updates \(for example, critical updates\) should be automatically approved\. The rejected list overrides both the rules and the approve list\. 

To use a list of approved patches to install specific packages, you first remove all auto\-approval rules\. If you explicitly identify a patch as rejected, it will not be approved or installed, even if it matches all of the criteria in an auto\-approval rule\. Also, a patch is installed on an instance only if it applies to the software on the instance, even if the patch has otherwise been approved for the instance\.

Patch Manager provides predefined patch baselines for each of the operating systems supported by Patch Manager\. You can use these baselines as they are currently configured \(you can't customize them\) or you can create your own custom patch baselines\. Custom patch baselines allows you greater control over which patches are approved or rejected for your environment\. Also, the predefined baselines assign a compliance level of `Unspecified` to all patches installed using those baselines\. For compliance values to be assigned, you can create a copy of a predefined baseline and specify the compliance values you want to assign to patches\. For more information, see [About custom baselines](#patch-manager-baselines-custom) and [Working with custom patch baselines](sysman-patch-baseline-console.md)\.

**Topics**
+ [About predefined baselines](#patch-manager-baselines-pre-defined)
+ [About custom baselines](#patch-manager-baselines-custom)

## About predefined baselines<a name="patch-manager-baselines-pre-defined"></a>

The following table describes the predefined patch baselines provided with Patch Manager\.

For information about which versions of each operating system Patch Manager supports, see [Patch Manager prerequisites](patch-manager-prerequisites.md)\.


****  

| Name | Supported operating system | Details | 
| --- | --- | --- | 
|  `AWS-AmazonLinuxDefaultPatchBaseline`  |  Amazon Linux  |  Approves all operating system patches that are classified as "Security" and that have a severity level of "Critical" or "Important"\. Patches are auto\-approved seven days after release\. Also auto\-approves all patches with a classification of "Bugfix" seven days after release\.  | 
| AWS\-AmazonLinux2DefaultPatchBaseline | Amazon Linux 2 | Approves all operating system patches that are classified as "Security" and that have a severity level of "Critical" or "Important"\. Patches are auto\-approved seven days after release\. Also approves all patches with a classification of "Bugfix" seven days after release\. | 
| AWS\-CentOSDefaultPatchBaseline | CentOS | Approves all updates seven days after they become available, including nonsecurity updates\. | 
| AWS\-DebianDefaultPatchBaseline | Debian | Immediately approves all operating system security\-related patches that have a priority of "Required", "Important", "Standard," "Optional," or "Extra\." There is no wait before approval because reliable release dates are not available in the repos\. | 
| AWS\-OracleLinuxDefaultPatchBaseline | Oracle Linux | Approves all operating system patches that are classified as "Security" and that have a severity level of "Important" or "Moderate"\. Patches are auto\-approved seven days after release\. Also approves all patches that are classified as "Bugfix" seven days after release\. | 
|  `AWS-RedHatDefaultPatchBaseline`  |  Red Hat Enterprise Linux \(RHEL\)   |  Approves all operating system patches that are classified as "Security" and that have a severity level of "Critical" or "Important"\. Patches are auto\-approved seven days after release\. Also approves all patches that are classified as "Bugfix" seven days after release\.  | 
| AWS\-SuseDefaultPatchBaseline | SUSE Linux Enterprise Server \(SLES\) | Approves all operating system patches that are classified as "Security" and with a severity of "Critical" or "Important"\. Patches are auto\-approved seven days after release\.  | 
|  `AWS-UbuntuDefaultPatchBaseline`  |  Ubuntu Server  |  Immediately approves all operating system security\-related patches that have a priority of "Required", "Important", "Standard," "Optional," or "Extra\." There is no wait before approval because reliable release dates are not available in the repos\.  | 
| AWS\-DefaultPatchBaseline |  Windows Server  |  Approves all Windows Server operating system patches that are classified as "CriticalUpdates" or "SecurityUpdates" and that have an MSRC severity of "Critical" or "Important"\. Patches are auto\-approved seven days after release\.  | 
| AWS\-WindowsPredefinedPatchBaseline\-OS |  Windows Server  |  Approves all Windows Server operating system patches that are classified as "CriticalUpdates" or "SecurityUpdates" and that have an MSRC severity of "Critical" or "Important"\. Patches are auto\-approved seven days after release\.  | 
| AWS\-WindowsPredefinedPatchBaseline\-OS\-Applications | Windows Server | For the Windows Server operating system, approves all patches that are classified as "CriticalUpdates" or "SecurityUpdates" and that have an MSRC severity of "Critical" or "Important"\. For Microsoft applications, approves all patches\. Patches for both OS and applications are auto\-approved seven days after release\. | 

## About custom baselines<a name="patch-manager-baselines-custom"></a>

If you create your own patch baseline, you can choose which patches to auto\-approve by using the following categories\.
+ Operating system: Windows, Amazon Linux, Ubuntu Server, and so on\.
+ Product name \(for operating systems\): For example, RHEL 6\.5, Amazon Linux 2014\.09, Windows Server 2012, Windows Server 2012 R2, and so on\.
+ Product name \(for Microsoft applications on Windows Server only\): For example, Word 2016, BizTalk Server, and so on\.
+ Classification: For example, critical updates, security updates, and so on\.
+ Severity: For example, critical, important, and so on\.

For each approval rule that you create, you can choose to specify an auto\-approval delay or specify a patch approval cutoff date\. 

**Note**  
Because it's not possible to reliably determine the release dates of update packages for Ubuntu Server, the auto\-approval options are not supported for this operating system\.

An auto\-approval delay is the number of days to wait after the patch was released, before the patch is automatically approved for patching\. For example, if you create a rule using the `CriticalUpdates` classification and configure it for seven days auto\-approval delay, then a new critical patch released on July 7 is automatically approved on July 14\.

**Note**  
If a Linux repository doesn’t provide release date information for packages, Systems Manager uses the build time of the package as the auto\-approval delay for Amazon Linux, Amazon Linux 2, RHEL, and CentOS\. If the system isn't able to find the build time of the package, Systems Manager treats the auto\-approval delay as having a value of zero\.

When you specify an auto\-approval cutoff date, Patch Manager automatically applies all patches released on or before that date\. For example, if you specify July 7, 2020, as the cutoff date, no patches released on or after July 8, 2020, are installed automatically\.

You can also specify a compliance severity level\. If an approved patch is reported as missing, `Compliance Level` is the severity of the compliance violation\. 

By using multiple patch baselines with different auto\-approval delays or cutoff dates, you can deploy patches at different rates to different instances\. For example, you can create separate patch baselines, auto\-approval delays, andcutoff dates for development and production environments\. This enables you to test patches in your development environment before they get deployed in your production environment\. 

Keep the following in mind when you create a patch baseline:
+ Patch Manager provides one predefined patch baseline for each supported operating system\. These predefined patch baselines are used as the default patch baselines for each operating system type unless you create your own patch baseline and designate it as the default for the corresponding operating system type\. 
**Note**  
For Windows Server, two predefined patch baselines are provided\. The patch baseline `AWS-WindowsPredefinedPatchBaseline-OS` supports only operating system updates on the Windows operating system itself\. It is used as the default patch baseline for Windows Server instances unless you specify a different patch baseline\. The other predefined Windows patch baseline, `AWS-WindowsPredefinedPatchBaseline-OS-Applications`, can be used to apply patches to both the Windows Server operating system and supported Microsoft applications\. 
+ For on\-premises or other non\-EC2 instances, Patch Manager attempts to use your custom default patch baseline\. If no custom default patch baseline exists, the system uses the predefined patch baseline for the corresponding operating system\.
+ If a patch is listed as both approved and rejected in the same patch baseline, the patch is rejected\.
+ An instance can have only one patch baseline defined for it\.
+ The formats of package names you can add to lists of approved patches and rejected patches for a patch baseline depend on the type of operating system you are patching\.

  For information about accepted formats for lists of approved patches and rejected patches, see [About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)\.

For information about creating a patch baseline, see [Working with custom patch baselines](sysman-patch-baseline-console.md) and [Walkthrough: Patch a server environment \(AWS CLI\)](sysman-patch-cliwalk.md)\.