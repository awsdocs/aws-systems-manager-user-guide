# About the `AWS-RunPatchBaselineWithHooks` SSM document<a name="patch-manager-about-aws-runpatchbaselinewithhooks"></a>

AWS Systems Manager supports `AWS-RunPatchBaselineWithHooks`, a Systems Manager document \(SSM document\) for Patch Manager, a capability of AWS Systems Manager\. This SSM document performs patching operations on instances for both security related and other types of updates\. 

`AWS-RunPatchBaselineWithHooks` differs from `AWS-RunPatchBaseline` in the following ways:
+ **A wrapper document** – `AWS-RunPatchBaselineWithHooks` is a wrapper for `AWS-RunPatchBaseline` and relies on `AWS-RunPatchBaseline` for some of its operations\.
+ **The `Install` operation** – `AWS-RunPatchBaselineWithHooks` supports lifecycle hooks that run at designated points during instance patching\. Because patch installations sometimes require instances to reboot, the patching operation is divided into two events, for a total of three hooks that support custom functionality\. The first hook is before the `Install with NoReboot` operation\. The second hook is after the `Install with NoReboot` operation\. The third hook is available after the reboot of the instance\.
+ **No custom patch list support** – `AWS-RunPatchBaselineWithHooks` doesn't support the `InstallOverrideList` parameter\.
+ **SSM Agent support** – `AWS-RunPatchBaselineWithHooks` requires that SSM Agent 3\.0\.502 or later be installed on the instance to patch\.

When the document is run, it uses the patch baseline currently specified as the "default" for an operating system type if no patch group is specified\. Otherwise, it uses the patch baselines that is associated with the patch group\. For information about patch groups, see [About patch groups](sysman-patch-patchgroups.md)\. 

You can use the document `AWS-RunPatchBaselineWithHooks` to apply patches for both operating systems and applications\. \(On Windows, application support is limited to updates for applications released by Microsoft\.\)

This document supports Linux, macOS, and Windows Server instances\. The document will perform the appropriate actions for each platform\.

------
#### [ Linux ]

On Linux instances, the `AWS-RunPatchBaselineWithHooks` document invokes a Python module, which in turn downloads a snapshot of the patch baseline that applies to the instance\. This patch baseline snapshot uses the defined rules and lists of approved and blocked patches to drive the appropriate package manager for each instance type: 
+ Amazon Linux, Amazon Linux 2, CentOS, Oracle Linux, and RHEL 7 instances use YUM\. For YUM operations, Patch Manager requires `Python 2.6` or later\. 
+ RHEL 8 instances use DNF\. For DNF operations, Patch Manager requires `Python 2` or `Python 3`\. \(Neither version is installed by default on RHEL 8\. You must install one or the other manually\.\)
+ Debian Server and Ubuntu Server instances use APT\. For APT operations, Patch Manager requires `Python 3`\. 
+ SUSE Linux Enterprise Server instances use Zypper\. For Zypper operations, Patch Manager requires `Python 2.6` or later\.

------
#### [ macOS ]

On macOS instances, the `AWS-RunPatchBaselineWithHooks` document invokes a Python module, which in turn downloads a snapshot of the patch baseline that applies to the instance\. Next, a Python subprocess invokes the CLI on the instance to retrieve the installation and update information for the specified package managers and to drive the appropriate package manager for each update package\.

------
#### [ Windows ]

On Windows Server instances, the `AWS-RunPatchBaselineWithHooks` document downloads and invokes a PowerShell module, which in turn downloads a snapshot of the patch baseline that applies to the instance\. This patch baseline snapshot contains a list of approved patches that is compiled by querying the patch baseline against a Windows Server Update Services \(WSUS\) server\. This list is passed to the Windows Update API, which controls downloading and installing the approved patches as appropriate\. 

------

Each snapshot is specific to an AWS account, patch group, operating system, and snapshot ID\. The snapshot is delivered through a presigned Amazon Simple Storage Service \(Amazon S3\) URL, which expires 24 hours after the snapshot is created\. After the URL expires, however, if you want to apply the same snapshot content to other instances, you can generate a new presigned Amazon S3 URL up to three days after the snapshot was created\. To do this, use the [get\-deployable\-patch\-snapshot\-for\-instance](https://docs.aws.amazon.com/cli/latest/reference/ssm/get-deployable-patch-snapshot-for-instance.html) command\. 

After all approved and applicable updates have been installed, with reboots performed as necessary, patch compliance information is generated on an instance and reported back to Patch Manager\. 

**Note**  
If the `RebootOption` parameter is set to `NoReboot` in the `AWS-RunPatchBaselineWithHooks` document, the instance isn't rebooted after Patch Manager runs\. For more information, see [Parameter name: `RebootOption`](patch-manager-about-aws-runpatchbaseline.md#patch-manager-about-aws-runpatchbaseline-parameters-norebootoption)\.

For information about viewing patch compliance data, see [About patch compliance](sysman-compliance-about.md#sysman-compliance-monitor-patch)\.

## `AWS-RunPatchBaselineWithHooks` operational steps<a name="patch-manager-about-aws-runpatchbaselinewithhooks-steps"></a>

When the `AWS-RunPatchBaselineWithHooks` runs, the following steps are performed:

1. **Scan** \- A `Scan` operation using `AWS-RunPatchBaseline` is run on the instance, and a compliance report is generated and uploaded\. 

1. **Verify local patch states** \- A script is run to determine what steps will be performed based on the selected operation and `Scan` result from Step 1\. 

   1. If the selected operation is `Scan`, the operation is marked complete\. The operation concludes\. 

   1. If the selected operation is `Install`, Patch Manager evaluates the `Scan` result from Step 1 to determine what to run next: 

      1. If no missing patches are detected, and no pending reboots required, the operation proceeds directly to the final step \(Step 8\), which includes a hook you have provided\. Any steps in between are skipped\. 

      1. If no missing patches are detected, but there are pending reboots required and the selected reboot option is `NoReboot`, the operation proceeds directly to the final step \(Step 8\), which includes a hook you have provided\. Any steps in between are skipped\. 

      1. Otherwise, the operation proceeds to the next step\.

1. **Pre\-patch hook operation** \- The SSM document you have provided for the first lifecycle hook, `PreInstallHookDocName`, is run on the instance\. 

1. **Install with NoReboot** \- An `Install` operation with the reboot option of `NoReboot` using `AWS-RunPatchBaseline` is run on the instance, and a compliance report is generated and uploaded\. 

1. **Post\-install hook operation** \- The SSM document you have provided for the second lifecycle hook, `PostInstallHookDocName`, is run on the instance\.

1. **Verify reboot** \- A script runs to determine whether a reboot is needed for the instance and what steps to run:

   1. If the selected reboot option is `NoReboot`, the operation proceeds directly to the final step \(Step 8\), which includes a hook you have provided\. Any steps in between are skipped\. 

   1. If the selected reboot option is `RebootIfNeeded`, Patch Manager checks whether there are any pending reboots required from the inventory collected in Step 4\. 

      1. If no patches requiring a reboot are found, the instance patching operation is complete, and the operation proceeds directly to the final step \(Step 8\), which includes a hook you have provided\. Any steps in between are skipped\. 

      1. If patches requiring a reboot are found, the operation continues to the next step\. 

1. **Reboot and report** \- An installation operation with the reboot option of `RebootIfNeeded` runs on the instance using `AWS-RunPatchBaseline`, and a compliance report is generated and uploaded\. 

1. **Post\-reboot hook operation** \- The SSM document you have provided for the third lifecycle hook, `OnExitHookDocName`, is run on the instance\. 

For a `Scan` operation, if Step 1 fails, the process of running the document stops and the step is reported as failed, although subsequent steps are reported as successful\. 

 For an `Install` operation, if any of the `aws:runDocument` steps fail during the operation, those steps are reported as failed, and the operation proceeds directly to the final step \(Step 8\), which includes a hook you have provided\. Any steps in between are skipped\. This step is reported as failed, the last step reports the status of its operation result, and all steps in between are reported as successful\.

## `AWS-RunPatchBaselineWithHooks` parameters<a name="patch-manager-about-aws-runpatchbaselinewithhooks-parameters"></a>

`AWS-RunPatchBaselineWithHooks` supports six parameters\. 

The `Operation` parameter is required\. 

The `RebootOption`, `PreInstallHookDocName`, `PostInstallHookDocName`, and `OnExitHookDocName` parameters are optional\. 

`Snapshot-ID` is technically optional, but we recommend that you supply a custom value for it when you run `AWS-RunPatchBaselineWithHooks` outside of a maintenance window\. Let Patch Manager supply the value automatically when the document is run as part of a maintenance window operation\.

**Topics**
+ [Parameter name: `Operation`](#patch-manager-about-aws-runpatchbaseline-parameters-operation)
+ [Parameter name: `Snapshot ID`](#patch-manager-about-aws-runpatchbaselinewithhook-parameters-snapshot-id)
+ [Parameter name: `RebootOption`](#patch-manager-about-aws-runpatchbaselinewithhooks-parameters-norebootoption)
+ [Parameter name: `PreInstallHookDocName`](#patch-manager-about-aws-runpatchbaselinewithhooks-parameters-preinstallhookdocname)
+ [Parameter name: `PostInstallHookDocName`](#patch-manager-about-aws-runpatchbaselinewithhooks-parameters-postinstallhookdocname)
+ [Parameter name: `OnExitHookDocName`](#patch-manager-about-aws-runpatchbaselinewithhooks-parameters-onexithookdocname)

### Parameter name: `Operation`<a name="patch-manager-about-aws-runpatchbaseline-parameters-operation"></a>

**Usage**: Required\.

**Options**: `Scan` \| `Install`\. 

Scan  
When you choose the `Scan` option, the systems uses the `AWS-RunPatchBaseline` document to determine the patch compliance state of the instance and reports this information back to Patch Manager\. `Scan` doesn't prompt updates to be installed or instances to be rebooted\. Instead, the operation identifies where updates are missing that are approved and applicable to the instance\. 

Install  
When you choose the `Install` option, `AWS-RunPatchBaselineWithHooks` attempts to install the approved and applicable updates that are missing from the instance\. Patch compliance information generated as part of an `Install` operation doesn't list any missing updates, but might report updates that are in a failed state if the installation of the update didn't succeed for any reason\. Whenever an update is installed on an instance, the instance is rebooted to ensure the update is both installed and active\. \(Exception: If the `RebootOption` parameter is set to `NoReboot` in the `AWS-RunPatchBaselineWithHooks` document, the instance isn't rebooted after Patch Manager runs\. For more information, see [Parameter name: `RebootOption`](#patch-manager-about-aws-runpatchbaselinewithhooks-parameters-norebootoption)\.\)  
If a patch specified by the baseline rules is installed *before* Patch Manager updates the instance, the system might not reboot as expected\. This can happen when a patch is installed manually by a user or installed automatically by another program, such as the `unattended-upgrades` package on Ubuntu Server\.

### Parameter name: `Snapshot ID`<a name="patch-manager-about-aws-runpatchbaselinewithhook-parameters-snapshot-id"></a>

**Usage**: Optional\.

`Snapshot ID` is a unique ID \(GUID\) used by Patch Manager to ensure that a set of instances that are patched in a single operation all have the exact same set of approved patches\. Although the parameter is defined as optional, our best practice recommendation depends on whether or not you're running `AWS-RunPatchBaselineWithHooks` in a maintenance window, as described in the following table\.


**`AWS-RunPatchBaselineWithHooks` best practices**  

| Mode | Best practice | Details | 
| --- | --- | --- | 
| Running AWS\-RunPatchBaselineWithHooks inside a maintenance window | Don't supply a Snapshot ID\. Patch Manager will supply it for you\. |  If you use a maintenance window to run `AWS-RunPatchBaselineWithHooks`, you shouldn't provide your own generated Snapshot ID\. In this scenario, Systems Manager provides a GUID value based on the maintenance window execution ID\. This ensures that a correct ID is used for all the invocations of `AWS-RunPatchBaselineWithHooks` in that maintenance window\.  If you do specify a value in this scenario, note that the snapshot of the patch baseline might not remain in place for more than three days\. After that, a new snapshot will be generated even if you specify the same ID after the snapshot expires\.   | 
| Running AWS\-RunPatchBaselineWithHooks outside of a maintenance window | Generate and specify a custom GUID value for the Snapshot ID\.¹ |  When you aren't using a maintenance window to run `AWS-RunPatchBaselineWithHooks`, we recommend that you generate and specify a unique Snapshot ID for each patch baseline, particularly if you're running the `AWS-RunPatchBaselineWithHooks` document on multiple instances in the same operation\. If you don't specify an ID in this scenario, Systems Manager generates a different Snapshot ID for each instance the command is sent to\. This might result in varying sets of patches being specified among the instances\. For instance, say that you're running the `AWS-RunPatchBaselineWithHooks` document directly through Run Command, a capability ofAWS Systems Manager, and targeting a group of 50 instances\. Specifying a custom Snapshot ID results in the generation of a single baseline snapshot that is used to evaluate and patch all the instances, ensuring that they end up in a consistent state\.   | 
|  ¹ You can use any tool capable of generating a GUID to generate a value for the Snapshot ID parameter\. For example, in PowerShell, you can use the `New-Guid` cmdlet to generate a GUID in the format of `12345699-9405-4f69-bc5e-9315aEXAMPLE`\.  | 

### Parameter name: `RebootOption`<a name="patch-manager-about-aws-runpatchbaselinewithhooks-parameters-norebootoption"></a>

**Usage**: Optional\.

**Options**: `RebootIfNeeded` \| `NoReboot`\. 

RebootIfNeeded  
When you choose the `RebootIfNeeded` option, the instance is rebooted if Patch Manager installed new patches, and it detected any patches with a status of `INSTALLED_PENDING_REBOOT` during the `Install` operation\. The `INSTALLED_PENDING_REBOOT` status can mean that the option `NoReboot` was selected the last time the `Install` operation was run\. \(Patches installed outside of Patch Manager are never given a status of `INSTALLED_PENDING_REBOOT`\.\)  
When you choose the `RebootIfNeeded` option, Patch Manager doesn't evaluate whether a reboot is *required* by the patch\. A reboot occurs whenever there are missing packages that require a reboot or packages with a status of `INSTALLED_PENDING_REBOOT`\.

NoReboot  
When you choose the `NoReboot` option, Patch Manager doesn't reboot an instance even if it installed patches during the `Install` operation\. This option is useful if you know that your instances don't require rebooting after patches are applied, or you have applications or processes running on an instance that shouldn't be disrupted by a patching operation reboot\. It's also useful when you want more control over the timing of instance reboots, such as by using a maintenance window\.  
If you choose the `NoReboot` option and a patch is installed, the patch is assigned a status of `InstalledPendingReboot`\. The instance itself, however, is marked as `Non-Compliant`\. After a reboot occurs and a `Scan` operation is run, the instance status is updated to `Compliant`\.

**Patch installation tracking file**: To track patch installation, especially patches that were installed since the last system reboot, Systems Manager maintains a file on the managed instance\.

**Important**  
Don't delete or modify the tracking file\. If this file is deleted or corrupted, the patch compliance report for the instance is inaccurate\. If this happens, reboot the instance and run a patch Scan operation to restore the file\.

This tracking file is stored in the following locations on your managed instances:
+ Linux operating systems: 
  + `/var/log/amazon/ssm/patch-configuration/patch-states-configuration.json`
  + `/var/log/amazon/ssm/patch-configuration/patch-inventory-from-last-operation.json`
+ Windows Server operating system:
  + `C:\ProgramData\Amazon\PatchBaselineOperations\State\PatchStatesConfiguration.json`
  + `C:\ProgramData\Amazon\PatchBaselineOperations\State\PatchInventoryFromLastOperation.json`

### Parameter name: `PreInstallHookDocName`<a name="patch-manager-about-aws-runpatchbaselinewithhooks-parameters-preinstallhookdocname"></a>

**Usage**: Optional\.

**Default**: `AWS-Noop`\. 

The value to provide for the `PreInstallHookDocName` parameter is the name or Amazon Resource Name \(ARN\) of an SSM document of your choice\. You can provide the name of an AWS managed document or the name or ARN of a custom SSM document that you have created or that has been shared with you\. \(For an SSM document that has been shared with you from a different AWS account, you must specify the full resource ARN, such as `aws:arn:ssm:us-east-2:123456789012:document/MySharedDocument`\.\)

The SSM document you specify is run before the `Install` operation and performs any actions supported by SSM Agent, such as a shell script to check application health check before patching is performed on the instance\. \(For a list of actions, see [Systems Manager Command document plugin reference](ssm-plugins.md)\)\. The default SSM document name is `AWS-Noop`, which doesn't perform any operation on the instance\. 

For information about creating a custom SSM document, see [Creating SSM documents](create-ssm-doc.md)\. 

### Parameter name: `PostInstallHookDocName`<a name="patch-manager-about-aws-runpatchbaselinewithhooks-parameters-postinstallhookdocname"></a>

**Usage**: Optional\.

**Default**: `AWS-Noop`\. 

The value to provide for the `PostInstallHookDocName` parameter is the name or Amazon Resource Name \(ARN\) of an SSM document of your choice\. You can provide the name of an AWS managed document or the name or ARN of a custom SSM document that you have created or that has been shared with you\. \(For an SSM document that has been shared with you from a different AWS account, you must specify the full resource ARN, such as `aws:arn:ssm:us-east-2:123456789012:document/MySharedDocument`\.\)

The SSM document you specify is run after the `Install with NoReboot` operation and performs any actions supported by SSM Agent, such as a shell script for installing third party updates before reboot\. \(For a list of actions, see [Systems Manager Command document plugin reference](ssm-plugins.md)\)\. The default SSM document name is `AWS-Noop`, which doesn't perform any operation on the instance\. 

For information about creating a custom SSM document, see [Creating SSM documents](create-ssm-doc.md)\. 

### Parameter name: `OnExitHookDocName`<a name="patch-manager-about-aws-runpatchbaselinewithhooks-parameters-onexithookdocname"></a>

**Usage**: Optional\.

**Default**: `AWS-Noop`\. 

The value to provide for the `OnExitHookDocName` parameter is the name or Amazon Resource Name \(ARN\) of an SSM document of your choice\. You can provide the name of an AWS managed document or the name or ARN of a custom SSM document that you have created or that has been shared with you\. \(For an SSM document that has been shared with you from a different AWS account, you must specify the full resource ARN, such as `aws:arn:ssm:us-east-2:123456789012:document/MySharedDocument`\.\)

The SSM document you specify is run after the instance reboot operation and performs any actions supported by SSM Agent, such as a shell script to verify instance health after the patching operation completes\. \(For a list of actions, see [Systems Manager Command document plugin reference](ssm-plugins.md)\)\. The default SSM document name is `AWS-Noop`, which doesn't perform any operation on the instance\. 

For information about creating a custom SSM document, see [Creating SSM documents](create-ssm-doc.md)\. 