# How patches are installed<a name="patch-manager-how-it-works-installation"></a>

Patch Manager, a capability of AWS Systems Manager, uses the appropriate built\-in mechanism for an operating system type to install updates on a managed node\. For example, on Windows Server, the Windows Update API is used, and on Amazon Linux the `yum` package manager is used\.

Choose from the following tabs to learn how Patch Manager installs patches on an operating system\.

------
#### [ Amazon Linux and Amazon Linux 2 ]

On Amazon Linux and Amazon Linux 2 managed nodes, the patch installation workflow is as follows:

1. If a list of patches is specified using an https URL or an Amazon Simple Storage Service \(Amazon S3\) path\-style URL using the `InstallOverrideList` parameter for the `AWS-RunPatchBaseline` or `AWS-RunPatchBaselineAssociation` documents, the listed patches are installed and steps 2\-7 are skipped\.

1. Apply [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters) as specified in the patch baseline, keeping only the qualified packages for further processing\. 

1. Apply [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) as specified in the patch baseline\. Each approval rule can define a package as approved\.

   Approval rules, however, are also subject to whether the **Include nonsecurity updates** check box was selected when creating or last updating a patch baseline\.

   If nonsecurity updates are excluded, an implicit rule is applied in order to select only packages with upgrades in security repos\. For each package, the candidate version of the package \(which is typically the latest version\) must be part of a security repo\. 

   If nonsecurity updates are included, patches from other repositories are considered as well\.

1. Apply [ApprovedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovedPatches) as specified in the patch baseline\. The approved patches are approved for update even if they're discarded by [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters) or if no approval rule specified in [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) grants it approval\.

1. Apply [RejectedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-RejectedPatches) as specified in the patch baseline\. The rejected patches are removed from the list of approved patches and won't be applied\.

1. If multiple versions of a patch are approved, the latest version is applied\.

1. The YUM update API is applied to approved patches as follows:
   + For predefined default patch baselines provided by AWS, and for custom patch baselines where the **Approved patches include non\-security updates** check box is *not* selected, only patches specified in `updateinfo.xml` are applied \(security updates only\)\.

     The equivalent yum command for this workflow is:

     ```
     sudo yum update-minimal --sec-severity=critical,important --bugfix -y
     ```
   + For custom patch baselines where the **Approved patches include non\-security updates** check box *is* selected, both patches in `updateinfo.xml` and those not in `updateinfo.xml` are applied \(security and nonsecurity updates\)\.

     The equivalent yum command for this workflow is:

     ```
     sudo yum update --security --bugfix
     ```

1. The managed node is rebooted if any updates were installed\. \(Exception: If the `RebootOption` parameter is set to `NoReboot` in the `AWS-RunPatchBaseline` document, the managed node isn't rebooted after Patch Manager runs\. For more information, see [Parameter name: `RebootOption`](patch-manager-about-aws-runpatchbaseline.md#patch-manager-about-aws-runpatchbaseline-parameters-norebootoption)\.\)

------
#### [ CentOS ]

On CentOS managed nodes, the patch installation workflow is as follows:

1. If a list of patches is specified using an https URL or an Amazon Simple Storage Service \(Amazon S3\) path\-style URL using the `InstallOverrideList` parameter for the `AWS-RunPatchBaseline` or `AWS-RunPatchBaselineAssociation` documents, the listed patches are installed and steps 2\-7 are skipped\.

   Apply [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters) as specified in the patch baseline, keeping only the qualified packages for further processing\. 

1. Apply [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) as specified in the patch baseline\. Each approval rule can define a package as approved\.

   Approval rules, however, are also subject to whether the **Include nonsecurity updates** check box was selected when creating or last updating a patch baseline\.

   If nonsecurity updates are excluded, an implicit rule is applied in order to select only packages with upgrades in security repos\. For each package, the candidate version of the package \(which is typically the latest version\) must be part of a security repo\. 

   If nonsecurity updates are included, patches from other repositories are considered as well\.

1. Apply [ApprovedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovedPatches) as specified in the patch baseline\. The approved patches are approved for update even if they're discarded by [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters) or if no approval rule specified in [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) grants it approval\.

1. Apply [RejectedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-RejectedPatches) as specified in the patch baseline\. The rejected patches are removed from the list of approved patches and won't be applied\.

1. If multiple versions of a patch are approved, the latest version is applied\.

1. The YUM update API \(on CentOS 6\.x and 7\.x versions\) or the DNF update \(on CentOS 8\) is applied to approved patches\.

1. The managed node is rebooted if any updates were installed\. \(Exception: If the `RebootOption` parameter is set to `NoReboot` in the `AWS-RunPatchBaseline` document, the managed node isn't rebooted after Patch Manager runs\. For more information, see [Parameter name: `RebootOption`](patch-manager-about-aws-runpatchbaseline.md#patch-manager-about-aws-runpatchbaseline-parameters-norebootoption)\.\)

------
#### [ Debian Server and Raspberry Pi OS ]

On Debian Server and Raspberry Pi OS \(formerly Raspbian\) instances, the patch installation workflow is as follows:

1. If a list of patches is specified using an https URL or an Amazon Simple Storage Service \(Amazon S3\) path\-style URL using the `InstallOverrideList` parameter for the `AWS-RunPatchBaseline` or `AWS-RunPatchBaselineAssociation` documents, the listed patches are installed and steps 2\-7 are skipped\.

1. If an update is available for `python3-apt` \(a Python library interface to `libapt`\), it is upgraded to the latest version\. \(This nonsecurity package is upgraded even if you did not select the **Include nonsecurity updates** option\.\)
**Important**  
On Debian Server 8 only: Because some Debian Server 8\.\* managed nodes refer to an obsolete package repository \(`jessie-backports`\), Patch Manager performs the following additional steps to ensure that patching operations succeed:  
On your managed node, the reference to the `jessie-backports` repository is commented out from the source location list \(`/etc/apt/sources.list.d/jessie-backports`\)\. As a result, no attempt is made to download patches from that location\.
A Stretch security update signing key is imported\. This key provides the necessary permissions for the update and install operations on Debian Server 8\.\* distributions\.
The `apt-get` operation is run at this point to ensure that the latest version of `python3-apt` is installed before the patching process begins\.
After the installation process is complete, the reference to the `jessie-backports` repository is restored and the signing key is removed from the apt sources keyring\. This is done to leave the system configuration as it was before the patching operation\. 
The next time Patch Manager updates the system, the same process is repeated\.

1. Apply [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters) as specified in the patch baseline, keeping only the qualified packages for further processing\. 

1. Apply [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) as specified in the patch baseline\. Each approval rule can define a package as approved\. 
**Note**  
Because it isn't possible to reliably determine the release dates of update packages for Debian Server, the auto\-approval options aren't supported for this operating system\.

   Approval rules, however, are also subject to whether the **Include nonsecurity updates** check box was selected when creating or last updating a patch baseline\.

   If nonsecurity updates are excluded, an implicit rule is applied in order to select only packages with upgrades in security repos\. For each package, the candidate version of the package \(which is typically the latest version\) must be part of a security repo\. 

   If nonsecurity updates are included, patches from other repositories are considered as well\.
**Note**  
For Debian Server and Raspberry Pi OS, patch candidate versions are limited to patches included in `debian-security`\.

1. Apply [ApprovedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovedPatches) as specified in the patch baseline\. The approved patches are approved for update even if they're discarded by [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters) or if no approval rule specified in [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) grants it approval\.

1. Apply [RejectedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-RejectedPatches) as specified in the patch baseline\. The rejected patches are removed from the list of approved patches and won't be applied\.

1. The APT library is used to upgrade packages\.

1. The managed node is rebooted if any updates were installed\. \(Exception: If the `RebootOption` parameter is set to `NoReboot` in the `AWS-RunPatchBaseline` document, the managed node isn't rebooted after Patch Manager runs\. For more information, see [Parameter name: `RebootOption`](patch-manager-about-aws-runpatchbaseline.md#patch-manager-about-aws-runpatchbaseline-parameters-norebootoption)\.\)

------
#### [ macOS ]

On macOS managed nodes, the patch installation workflow is as follows:

1. The `/Library/Receipts/InstallHistory.plist` property list is a record of software that has been installed and upgraded using the `softwareupdate` and `installer` package managers\. Using the `pkgutil` command line tool \(for `installer`\) and the `softwareupdate` package manager, CLI commands are run to parse this list\. 

   For `installer`, the response to the CLI commands includes `package name`, `version`, `volume`, `location`, and `install-time` details, but only the `package name` and `version` are used by Patch Manager\.

   For `softwareupdate`, the response to the CLI commands includes the package name \(`display name`\), `version`, and `date`, but only the package name and version are used by Patch Manager\.

   For Brew and Brew Cask, Homebrew doesn't support its commands running under the root user\. As a result, Patch Manager queries for and runs Homebrew commands as either the owner of the Homebrew directory or as a valid user belonging to the Homebrew directory’s owner group\. The commands are similar to `softwareupdate` and `installer` and are run through a Python subprocess to gather package data, and the output is parsed to identify package names and versions\.

1. Apply [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters) as specified in the patch baseline, keeping only the qualified packages for further processing\. 

1. Apply [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) as specified in the patch baseline\. Each approval rule can define a package as approved\.

1. Apply [ApprovedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovedPatches) as specified in the patch baseline\. The approved patches are approved for update even if they're discarded by [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters) or if no approval rule specified in [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) grants it approval\.

1. Apply [RejectedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-RejectedPatches) as specified in the patch baseline\. The rejected patches are removed from the list of approved patches and won't be applied\.

1. If multiple versions of a patch are approved, the latest version is applied\.

1. Invokes the appropriate package CLI on the managed node to process approved patches as follows:
**Note**  
`installer` lacks the functionality to check for and install updates\. Therefore, for `installer`, Patch Manager only reports which packages are installed\. As a result, `installer` packages are never reported as `Missing`\.
   + For predefined default patch baselines provided by AWS, and for custom patch baselines where the **Approved patches include non\-security updates** check box is *not* selected, only security updates are applied\.
   + For custom patch baselines where the **Approved patches include non\-security updates** check box *is* selected, both security and nonsecurity updates are applied\.

1. The managed node is rebooted if any updates were installed\. \(Exception: If the `RebootOption` parameter is set to `NoReboot` in the `AWS-RunPatchBaseline` document, the managed node isn't rebooted after Patch Manager runs\. For more information, see [Parameter name: `RebootOption`](patch-manager-about-aws-runpatchbaseline.md#patch-manager-about-aws-runpatchbaseline-parameters-norebootoption)\.\)

------
#### [ Oracle Linux ]

On Oracle Linux managed nodes, the patch installation workflow is as follows:

1. If a list of patches is specified using an https URL or an Amazon Simple Storage Service \(Amazon S3\) path\-style URL using the `InstallOverrideList` parameter for the `AWS-RunPatchBaseline` or `AWS-RunPatchBaselineAssociation` documents, the listed patches are installed and steps 2\-7 are skipped\.

1. Apply [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters) as specified in the patch baseline, keeping only the qualified packages for further processing\. 

1. Apply [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) as specified in the patch baseline\. Each approval rule can define a package as approved\.

   Approval rules, however, are also subject to whether the **Include nonsecurity updates** check box was selected when creating or last updating a patch baseline\.

   If nonsecurity updates are excluded, an implicit rule is applied in order to select only packages with upgrades in security repos\. For each package, the candidate version of the package \(which is typically the latest version\) must be part of a security repo\. 

   If nonsecurity updates are included, patches from other repositories are considered as well\.

1. Apply [ApprovedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovedPatches) as specified in the patch baseline\. The approved patches are approved for update even if they're discarded by [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters) or if no approval rule specified in [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) grants it approval\.

1. Apply [RejectedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-RejectedPatches) as specified in the patch baseline\. The rejected patches are removed from the list of approved patches and won't be applied\.

1. If multiple versions of a patch are approved, the latest version is applied\.

1. On version 7 managed nodes, the YUM update API is applied to approved patches as follows:
   + For predefined default patch baselines provided by AWS, and for custom patch baselines where the **Approved patches include non\-security updates** check box is *not* selected, only patches specified in `updateinfo.xml` are applied \(security updates only\)\.

     The equivalent yum command for this workflow is:

     ```
     sudo yum update-minimal --sec-severity=important,moderate --bugfix -y
     ```
   + For custom patch baselines where the **Approved patches include non\-security updates** check box *is* selected, both patches in `updateinfo.xml` and those not in `updateinfo.xml` are applied \(security and nonsecurity updates\)\.

     The equivalent yum command for this workflow is:

     ```
     sudo yum update --security --bugfix -y
     ```

     On version 8 managed nodes, the Dnf update API is applied to approved patches as follows:
     + For predefined default patch baselines provided by AWS, and for custom patch baselines where the **Approved patches include non\-security updates** check box is *not* selected, only patches specified in `updateinfo.xml` are applied \(security updates only\)\.

       The equivalent yum command for this workflow is:

       ```
       sudo dnf upgrade-minimal --security --sec-severity Moderate --sec-severity Important
       ```
     + For custom patch baselines where the **Approved patches include non\-security updates** check box *is* selected, both patches in `updateinfo.xml` and those not in `updateinfo.xml` are applied \(security and nonsecurity updates\)\.

       The equivalent yum command for this workflow is:

       ```
       sudo dnf upgrade --security --bugfix
       ```

1. The managed node is rebooted if any updates were installed\. \(Exception: If the `RebootOption` parameter is set to `NoReboot` in the `AWS-RunPatchBaseline` document, the managed node isn't rebooted after Patch Manager runs\. For more information, see [Parameter name: `RebootOption`](patch-manager-about-aws-runpatchbaseline.md#patch-manager-about-aws-runpatchbaseline-parameters-norebootoption)\.\)

------
#### [ RHEL, CentOS Stream, and Rocky Linux  ]

On Red Hat Enterprise Linux, CentOS Stream, and Rocky Linux managed nodes, the patch installation workflow is as follows:

1. If a list of patches is specified using an https URL or an Amazon Simple Storage Service \(Amazon S3\) path\-style URL using the `InstallOverrideList` parameter for the `AWS-RunPatchBaseline` or `AWS-RunPatchBaselineAssociation` documents, the listed patches are installed and steps 2\-7 are skipped\.

1. Apply [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters) as specified in the patch baseline, keeping only the qualified packages for further processing\. 

1. Apply [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) as specified in the patch baseline\. Each approval rule can define a package as approved\.

   Approval rules, however, are also subject to whether the **Include nonsecurity updates** check box was selected when creating or last updating a patch baseline\.

   If nonsecurity updates are excluded, an implicit rule is applied in order to select only packages with upgrades in security repos\. For each package, the candidate version of the package \(which is typically the latest version\) must be part of a security repo\. 

   If nonsecurity updates are included, patches from other repositories are considered as well\.

1. Apply [ApprovedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovedPatches) as specified in the patch baseline\. The approved patches are approved for update even if they're discarded by [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters) or if no approval rule specified in [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) grants it approval\.

1. Apply [RejectedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-RejectedPatches) as specified in the patch baseline\. The rejected patches are removed from the list of approved patches and won't be applied\.

1. If multiple versions of a patch are approved, the latest version is applied\.

1. The YUM update API \(on RHEL 7\) or the DNF update API \(on RHEL 8, CentOS Stream, and Rocky Linux\) is applied to approved patches as follows:
   + For predefined default patch baselines provided by AWS, and for custom patch baselines where the **Approved patches include non\-security updates** check box is *not* selected, only patches specified in `updateinfo.xml` are applied \(security updates only\)\.

     For RHEL 7, the equivalent yum command for this workflow is:

     ```
     sudo yum update-minimal --sec-severity=critical,important --bugfix -y
     ```

     For RHEL 8, CentOS Stream, and Rocky Linux , the equivalent dnf commands for this workflow are:

     ```
     sudo dnf update-minimal --sec-severity=Critical --bugfix -y ; \
     sudo dnf update-minimal --sec-severity=Important --bugfix -y
     ```
   + For custom patch baselines where the **Approved patches include non\-security updates** check box *is* selected, both patches in `updateinfo.xml` and those not in `updateinfo.xml` are applied \(security and nonsecurity updates\)\.

     For RHEL 7, the equivalent yum command for this workflow is:

     ```
     sudo yum update --security --bugfix -y
     ```

     For RHEL 8, CentOS Stream, and Rocky Linux, the equivalent dnf command for this workflow is:

     ```
     sudo dnf update --security --bugfix -y
     ```

1. The managed node is rebooted if any updates were installed\. \(Exception: If the `RebootOption` parameter is set to `NoReboot` in the `AWS-RunPatchBaseline` document, the managed node isn't rebooted after Patch Manager runs\. For more information, see [Parameter name: `RebootOption`](patch-manager-about-aws-runpatchbaseline.md#patch-manager-about-aws-runpatchbaseline-parameters-norebootoption)\.\)

------
#### [ SLES ]

On SUSE Linux Enterprise Server \(SLES\) managed nodes, the patch installation workflow is as follows:

1. If a list of patches is specified using an https URL or an Amazon Simple Storage Service \(Amazon S3\) path\-style URL using the `InstallOverrideList` parameter for the `AWS-RunPatchBaseline` or `AWS-RunPatchBaselineAssociation` documents, the listed patches are installed and steps 2\-7 are skipped\.

1. Apply [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters) as specified in the patch baseline, keeping only the qualified packages for further processing\. 

1. Apply [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) as specified in the patch baseline\. Each approval rule can define a package as approved\.

   Approval rules, however, are also subject to whether the **Include nonsecurity updates** check box was selected when creating or last updating a patch baseline\.

   If nonsecurity updates are excluded, an implicit rule is applied in order to select only packages with upgrades in security repos\. For each package, the candidate version of the package \(which is typically the latest version\) must be part of a security repo\. 

   If nonsecurity updates are included, patches from other repositories are considered as well\.

1. Apply [ApprovedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovedPatches) as specified in the patch baseline\. The approved patches are approved for update even if they're discarded by [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters) or if no approval rule specified in [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) grants it approval\.

1. Apply [RejectedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-RejectedPatches) as specified in the patch baseline\. The rejected patches are removed from the list of approved patches and won't be applied\.

1. If multiple versions of a patch are approved, the latest version is applied\.

1. The Zypper update API is applied to approved patches\.

1. The managed node is rebooted if any updates were installed\. \(Exception: If the `RebootOption` parameter is set to `NoReboot` in the `AWS-RunPatchBaseline` document, the managed node isn't rebooted after Patch Manager runs\. For more information, see [Parameter name: `RebootOption`](patch-manager-about-aws-runpatchbaseline.md#patch-manager-about-aws-runpatchbaseline-parameters-norebootoption)\.\)

------
#### [ Ubuntu Server ]

On Ubuntu Server managed nodes, the patch installation workflow is as follows:

1. If a list of patches is specified using an https URL or an Amazon Simple Storage Service \(Amazon S3\) path\-style URL using the `InstallOverrideList` parameter for the `AWS-RunPatchBaseline` or `AWS-RunPatchBaselineAssociation` documents, the listed patches are installed and steps 2\-7 are skipped\.

1. If an update is available for `python3-apt` \(a Python library interface to `libapt`\), it is upgraded to the latest version\. \(This nonsecurity package is upgraded even if you did not select the **Include nonsecurity updates** option\.\)

1. Apply [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters) as specified in the patch baseline, keeping only the qualified packages for further processing\. 

1. Apply [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) as specified in the patch baseline\. Each approval rule can define a package as approved\. 
**Note**  
Because it's not possible to reliably determine the release dates of update packages for Ubuntu Server, the auto\-approval options aren't supported for this operating system\.

   Approval rules, however, are also subject to whether the **Include nonsecurity updates** check box was selected when creating or last updating a patch baseline\.

   If nonsecurity updates are excluded, an implicit rule is applied in order to select only packages with upgrades in security repos\. For each package, the candidate version of the package \(which is typically the latest version\) must be part of a security repo\. 

   If nonsecurity updates are included, patches from other repositories are considered as well\.

   Approval rules, however, are also subject to whether the **Include nonsecurity updates** check box was selected when creating or last updating a patch baseline\.
**Note**  
For Ubuntu Server, patch candidate versions are limited to patches included in `trusty-security` \(Ubuntu Server 14\.04 LTS\), `xenial-security` \(Ubuntu Server 16\.04 LTS\), `bionic-security` \(Ubuntu Server 18\.04 LTS\), `focal-security` \(Ubuntu Server 20\.04 LTS\), or `groovy-gorilla` \(Ubuntu Server 20\.10 STR\)\.

1. Apply [ApprovedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovedPatches) as specified in the patch baseline\. The approved patches are approved for update even if they're discarded by [GlobalFilters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-GlobalFilters) or if no approval rule specified in [ApprovalRules](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) grants it approval\.

1. Apply [RejectedPatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-RejectedPatches) as specified in the patch baseline\. The rejected patches are removed from the list of approved patches and won't be applied\.

1. The APT library is used to upgrade packages\.

1. The managed node is rebooted if any updates were installed\. \(Exception: If the `RebootOption` parameter is set to `NoReboot` in the `AWS-RunPatchBaseline` document, the managed node isn't rebooted after Patch Manager runs\. For more information, see [Parameter name: `RebootOption`](patch-manager-about-aws-runpatchbaseline.md#patch-manager-about-aws-runpatchbaseline-parameters-norebootoption)\.\)

------
#### [ Windows ]

When a patching operation is performed on a Windows Server managed node, the node requests a snapshot of the appropriate patch baseline from Systems Manager\. This snapshot contains the list of all updates available in the patch baseline that were approved for deployment\. This list of updates is sent to the Windows Update API, which determines which of the updates are applicable to the managed node and installs them as needed\. If any updates are installed, the managed node is rebooted afterwards, as many times as necessary to complete all necessary patching\. \(Exception: If the `RebootOption` parameter is set to `NoReboot` in the `AWS-RunPatchBaseline` document, the managed node isn't rebooted after Patch Manager runs\. For more information, see [Parameter name: `RebootOption`](patch-manager-about-aws-runpatchbaseline.md#patch-manager-about-aws-runpatchbaseline-parameters-norebootoption)\.\) The summary of the patching operation can be found in the output of the Run Command request\. Additional logs can be found on the managed node in the `%PROGRAMDATA%\Amazon\PatchBaselineOperations\Logs` folder\.

Because the Windows Update API is used to download and install patches, all Group Policy settings for Windows Update are respected\. No Group Policy settings are required to use Patch Manager, but any settings that you have defined will be applied, such as to direct managed nodes to a Windows Server Update Services \(WSUS\) server\.

**Note**  
By default, Windows downloads all patches from Microsoft's Windows Update site because Patch Manager uses the Windows Update API to drive the download and installation of patches\. As a result, the managed node must be able to reach the Microsoft Windows Update site or patching will fail\. Alternatively, you can configure a WSUS server to serve as a patch repository and configure your managed nodes to target that WSUS server using Group Policies\.

------