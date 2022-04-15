# About predefined and custom patch baselines<a name="sysman-patch-baselines"></a>

Patch Manager, a capability of AWS Systems Manager, provides predefined patch baselines for each of the operating systems supported by Patch Manager\. You can use these baselines as they are currently configured \(you can't customize them\) or you can create your own custom patch baselines\. Custom patch baselines allows you greater control over which patches are approved or rejected for your environment\. Also, the predefined baselines assign a compliance level of `Unspecified` to all patches installed using those baselines\. For compliance values to be assigned, you can create a copy of a predefined baseline and specify the compliance values you want to assign to patches\. For more information, see [About custom baselines](#patch-manager-baselines-custom) and [Working with custom patch baselines \(console\)](sysman-patch-baseline-console.md)\.

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
| AWS\-CentOSDefaultPatchBaseline | CentOS and CentOS Stream | Approves all updates seven days after they become available, including nonsecurity updates\. | 
| AWS\-DebianDefaultPatchBaseline | Debian Server | Immediately approves all operating system security\-related patches that have a priority of "Required", "Important", "Standard," "Optional," or "Extra\." There is no wait before approval because reliable release dates aren't available in the repos\. | 
| AWS\-MacOSDefaultPatchBaseline | macOS | Approves all operating system patches that are classified as "Security"\. Also approves all packages with a current update\. | 
| AWS\-OracleLinuxDefaultPatchBaseline | Oracle Linux | Approves all operating system patches that are classified as "Security" and that have a severity level of "Important" or "Moderate"\. Patches are auto\-approved seven days after release\. Also approves all patches that are classified as "Bugfix" seven days after release\. | 
| AWS\-DefaultRaspbianPatchBaseline | Raspberry Pi OS | Immediately approves all operating system security\-related patches that have a priority of "Required", "Important", "Standard," "Optional," or "Extra\." There is no wait before approval because reliable release dates aren't available in the repos\. | 
|  `AWS-RedHatDefaultPatchBaseline`  |  Red Hat Enterprise Linux \(RHEL\)   |  Approves all operating system patches that are classified as "Security" and that have a severity level of "Critical" or "Important"\. Patches are auto\-approved seven days after release\. Also approves all patches that are classified as "Bugfix" seven days after release\.  | 
|  `AWS-RockyLinuxDefaultPatchBaseline`  |  Rocky Linux  |  Approves all operating system patches that are classified as "Security" and that have a severity level of "Critical" or "Important"\. Patches are auto\-approved seven days after release\. Also approves all patches that are classified as "Bugfix" seven days after release\.  | 
| AWS\-SuseDefaultPatchBaseline | SUSE Linux Enterprise Server \(SLES\) | Approves all operating system patches that are classified as "Security" and with a severity of "Critical" or "Important"\. Patches are auto\-approved seven days after release\.  | 
|  `AWS-UbuntuDefaultPatchBaseline`  |  Ubuntu Server  |  Immediately approves all operating system security\-related patches that have a priority of "Required", "Important", "Standard," "Optional," or "Extra\." There is no wait before approval because reliable release dates aren't available in the repos\.  | 
| AWS\-DefaultPatchBaseline |  Windows Server  |  Approves all Windows Server operating system patches that are classified as "CriticalUpdates" or "SecurityUpdates" and that have an MSRC severity of "Critical" or "Important"\. Patches are auto\-approved seven days after release\.  | 
| AWS\-WindowsPredefinedPatchBaseline\-OS |  Windows Server  |  Approves all Windows Server operating system patches that are classified as "CriticalUpdates" or "SecurityUpdates" and that have an MSRC severity of "Critical" or "Important"\. Patches are auto\-approved seven days after release\.  | 
| AWS\-WindowsPredefinedPatchBaseline\-OS\-Applications | Windows Server | For the Windows Server operating system, approves all patches that are classified as "CriticalUpdates" or "SecurityUpdates" and that have an MSRC severity of "Critical" or "Important"\. For applications released by Microsoft, approves all patches\. Patches for both OS and applications are auto\-approved seven days after release\. | 

## About custom baselines<a name="patch-manager-baselines-custom"></a>

If you create your own patch baseline, you can choose which patches to auto\-approve by using the following categories\.
+ Operating system: Windows, Amazon Linux, Ubuntu Server, and so on\.
+ Product name \(for operating systems\): For example, RHEL 6\.5, Amazon Linux 2014\.09, Windows Server 2012, Windows Server 2012 R2, and so on\.
+ Product name \(for applications released by Microsoft on Windows Server only\): For example, Word 2016, BizTalk Server, and so on\.
+ Classification: For example, critical updates, security updates, and so on\.
+ Severity: For example, critical, important, and so on\.

For each approval rule that you create, you can choose to specify an auto\-approval delay or specify a patch approval cutoff date\. 

**Note**  
Because it's not possible to reliably determine the release dates of update packages for Ubuntu Server, the auto\-approval options aren't supported for this operating system\.

An auto\-approval delay is the number of days to wait after the patch was released, before the patch is automatically approved for patching\. For example, if you create a rule using the `CriticalUpdates` classification and configure it for seven days auto\-approval delay, then a new critical patch released on July 7 is automatically approved on July 14\.

**Note**  
If a Linux repository doesn’t provide release date information for packages, Systems Manager uses the build time of the package as the auto\-approval delay for Amazon Linux, Amazon Linux 2, RHEL, and CentOS\. If the system isn't able to find the build time of the package, Systems Manager treats the auto\-approval delay as having a value of zero\.

When you specify an auto\-approval cutoff date, Patch Manager automatically applies all patches released on or before that date\. For example, if you specify July 7, 2020, as the cutoff date, no patches released on or after July 8, 2020, are installed automatically\.

You can also specify a compliance severity level\. If an approved patch is reported as missing, `Compliance Level` is the severity of the compliance violation\. 

By using multiple patch baselines with different auto\-approval delays or cutoff dates, you can deploy patches at different rates to different managed nodes\. For example, you can create separate patch baselines, auto\-approval delays, andcutoff dates for development and production environments\. This allows you to test patches in your development environment before they get deployed in your production environment\. 

Keep the following in mind when you create a patch baseline:
+ Patch Manager provides one predefined patch baseline for each supported operating system\. These predefined patch baselines are used as the default patch baselines for each operating system type unless you create your own patch baseline and designate it as the default for the corresponding operating system type\. 
**Note**  
For Windows Server, three predefined patch baselines are provided\. The patch baselines `AWS-DefaultPatchBaseline` and `AWS-WindowsPredefinedPatchBaseline-OS` support only operating system updates on the Windows operating system itself\. `AWS-DefaultPatchBaseline` is used as the default patch baseline for Windows Server managed nodes unless you specify a different patch baseline\. The configuration settings in these two patch baselines are the same\. The newer of the two, `AWS-WindowsPredefinedPatchBaseline-OS`, was created to distinguish it from the third predefined patch baseline for Windows Server\. That patch baseline, `AWS-WindowsPredefinedPatchBaseline-OS-Applications`, can be used to apply patches to both the Windows Server operating system and supported applications released by Microsoft\.
+ For on\-premises servers and virtual machines \(VMs\), Patch Manager attempts to use your custom default patch baseline\. If no custom default patch baseline exists, the system uses the predefined patch baseline for the corresponding operating system\.
+ If a patch is listed as both approved and rejected in the same patch baseline, the patch is rejected\.
+ A managed node can have only one patch baseline defined for it\.
+ The formats of package names you can add to lists of approved patches and rejected patches for a patch baseline depend on the type of operating system you're patching\.

  For information about accepted formats for lists of approved patches and rejected patches, see [About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)\.

For information about creating a patch baseline, see [Working with custom patch baselines \(console\)](sysman-patch-baseline-console.md) and [Walkthrough: Patch a server environment \(AWS CLI\)](sysman-patch-cliwalk.md)\.