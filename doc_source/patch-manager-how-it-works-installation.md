# How Patches Are Installed<a name="patch-manager-how-it-works-installation"></a>

Patch Manager uses the appropriate built\-in mechanism for an operating system type to install updates on an instance\. For example, on Windows, the Windows Update API is used, and on Amazon Linux the `yum` package manager is used\.

Choose from the following tabs to learn how Patch Manager installs patches on an operating system\.

------
#### [ Windows ]

When a patching operation is performed on a Windows instance, the instance requests a snapshot of the appropriate patch baseline from Systems Manager\. This snapshot contains the list of all updates available in the patch baseline that have been approved for deployment\. This list of updates is sent to the Windows Update API, which determines which of the updates are applicable to the instance and installs them as needed\. If any updates are installed, the instance is rebooted afterwards, as many times as necessary to complete all necessary patching\. The summary of the patching operation can be found in the output of the Run Command request\. Additional logs can be found on the instance in the `%PROGRAMDATA%\Amazon\PatchBaselineOperations\Logs` folder\.

Because the Windows Update API is used to download and install patches, all Group Policy settings for Windows Update are respected\. No Group Policy settings are required to use Patch Manager, but any settings that you have defined will be applied, such as to direct instances to a WSUS server\.

**Note**  
By default, Windows downloads all patches from Microsoft's Windows Update site because Patch Manager uses the Windows Update API to drive the download and installation of patches\. As a result, the instance must be able to reach the Microsoft Windows Update site or patching will fail\. Alternatively, you can configure a WSUS server to serve as a patch repository and configure your instances to target that WSUS server instead using Group Policies\.

------
#### [ Amazon Linux and Amazon Linux 2 ]

On Amazon Linux and Amazon Linux 2 instances, the patch installation workflow is as follows:

1. Apply [GlobalFilters](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-GlobalFilters) as specified in the patch baseline, keeping only the qualified packages for further processing\. 

1. Apply [ApprovalRules](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) as specified in the patch baseline\. Each approval rule can define a package as approved\.

1. Apply [ApprovedPatches](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovedPatches) as specified in the patch baseline\. The approved patches are approved for update even if they are discarded by [GlobalFilters](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-GlobalFilters) or if no approval rule specified in [ApprovalRules](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) grants it approval\.

1. Apply [RejectedPatches](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-RejectedPatches) as specified in the patch baseline\. The rejected patches are removed from the list of approved patches and will not be applied\.

1. If multiple versions of a patch are approved, the latest version is applied\.

1. The YUM update API is applied to approved patches\.

1. The instance is rebooted if any updates were installed\. 

**Note**  
The equivalent yum command for this workflow is:  

```
sudo yum update-minimal --security --bugfix 
```

------
#### [ RHEL ]

On Red Hat Enterprise Linux instances, the patch installation workflow is as follows:

1.  Apply [GlobalFilters](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-GlobalFilters) as specified in the patch baseline, keeping only the qualified packages for further processing\. 

1. Apply [ApprovalRules](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) as specified in the patch baseline\. Each approval rule can define a package as approved\.

1. Apply [ApprovedPatches](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovedPatches) as specified in the patch baseline\. The approved patches are approved for update even if they are discarded by [GlobalFilters](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-GlobalFilters) or if no approval rule specified in [ApprovalRules](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) grants it approval\.

1. Apply [RejectedPatches](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-RejectedPatches) as specified in the patch baseline\. The rejected patches are removed from the list of approved patches and will not be applied\.

1. If multiple versions of a patch are approved, the latest version is applied\.

1. The YUM update API is applied to approved patches\.

1. The instance is rebooted if any updates were installed\. 

**Note**  
The equivalent yum command for this workflow is:  

```
sudo yum update-minimal --security --bugfix 
```

------
#### [ Ubuntu ]

On Ubuntu Server instances, the patch installation workflow is as follows:

1.  Apply [GlobalFilters](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-GlobalFilters) as specified in the patch baseline, keeping only the qualified packages for further processing\. 

1. Apply [ApprovalRules](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) as specified in the patch baseline\. Each approval rule can define a package as approved\. In addition, an implicit rule is applied in order to select only packages with upgrades in security repos\. For each package, the candidate version of the package \(which is typically the latest version\) must be part of a security repo\.

1. Apply [ApprovedPatches](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovedPatches) as specified in the patch baseline\. The approved patches are approved for update even if they are discarded by [GlobalFilters](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-GlobalFilters) or if no approval rule specified in [ApprovalRules](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) grants it approval\.

1. Apply [RejectedPatches](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-RejectedPatches) as specified in the patch baseline\. The rejected patches are removed from the list of approved patches and will not be applied\.

1. The APT library is used to upgrade packages\.

1. The instance is rebooted if any updates were installed\. 

------
#### [ SLES ]

On SUSE Linux Enterprise Server \(SLES\) instances, the patch installation workflow is as follows:

1. Apply [GlobalFilters](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-GlobalFilters) as specified in the patch baseline, keeping only the qualified packages for further processing\. 

1. Apply [ApprovalRules](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) as specified in the patch baseline\. Each approval rule can define a package as approved\.

1. Apply [ApprovedPatches](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovedPatches) as specified in the patch baseline\. The approved patches are approved for update even if they are discarded by [GlobalFilters](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-GlobalFilters) or if no approval rule specified in [ApprovalRules](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) grants it approval\.

1. Apply [RejectedPatches](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-RejectedPatches) as specified in the patch baseline\. The rejected patches are removed from the list of approved patches and won't be applied\.

1. If multiple versions of a patch are approved, the latest version is applied\.

1. The Zypper update API is applied to approved patches\.

1. The instance is rebooted if any updates were installed\. 

------
#### [ CentOS ]

On CentOS instances, the patch installation workflow is as follows:

1.  Apply [GlobalFilters](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-GlobalFilters) as specified in the patch baseline, keeping only the qualified packages for further processing\. 

1. Apply [ApprovalRules](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) as specified in the patch baseline\. Each approval rule can define a package as approved\.

1. Apply [ApprovedPatches](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovedPatches) as specified in the patch baseline\. The approved patches are approved for update even if they are discarded by [GlobalFilters](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-GlobalFilters) or if no approval rule specified in [ApprovalRules](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-ApprovalRules) grants it approval\.

1. Apply [RejectedPatches](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#EC2-CreatePatchBaseline-request-RejectedPatches) as specified in the patch baseline\. The rejected patches are removed from the list of approved patches and will not be applied\.

1. If multiple versions of a patch are approved, the latest version is applied\.

1. The YUM update API is applied to approved patches\.

1. The instance is rebooted if any updates were installed\. 

------