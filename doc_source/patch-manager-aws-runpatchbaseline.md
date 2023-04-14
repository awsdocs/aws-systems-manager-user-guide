# About the `AWS-RunPatchBaseline` SSM document<a name="patch-manager-aws-runpatchbaseline"></a>

AWS Systems Manager supports `AWS-RunPatchBaseline`, a Systems Manager document \(SSM document\) for Patch Manager, a capability of AWS Systems Manager\. This SSM document performs patching operations on managed nodes for both security related and other types of updates\. When the document is run, it uses the patch baseline specified as the "default" for an operating system type if no patch group is specified\. Otherwise, it uses the patch baseline that is associated with the patch group\. For information about patch groups, see [About patch groups](patch-manager-patch-groups.md)\. 

You can use the document `AWS-RunPatchBaseline` to apply patches for both operating systems and applications\. \(On Windows Server, application support is limited to updates for applications released by Microsoft\.\)

This document supports Linux, macOS, and Windows Server managed nodes\. The document will perform the appropriate actions for each platform\. 

**Note**  
Patch Manager also supports the legacy SSM document `AWS-ApplyPatchBaseline`\. However, this document supports patching on Windows managed nodes only\. We encourage you to use `AWS-RunPatchBaseline` instead because it supports patching on Linux, macOS, and Windows Server managed nodes\. Version 2\.0\.834\.0 or later of SSM Agent is required in order to use the `AWS-RunPatchBaseline` document\.

------
#### [ Windows ]

On Windows Server managed nodes, the `AWS-RunPatchBaseline` document downloads and invokes a PowerShell module, which in turn downloads a snapshot of the patch baseline that applies to the managed node\. This patch baseline snapshot contains a list of approved patches that is compiled by querying the patch baseline against a Windows Server Update Services \(WSUS\) server\. This list is passed to the Windows Update API, which controls downloading and installing the approved patches as appropriate\. 

------
#### [ Linux ]

On Linux managed nodes, the `AWS-RunPatchBaseline` document invokes a Python module, which in turn downloads a snapshot of the patch baseline that applies to the managed node\. This patch baseline snapshot uses the defined rules and lists of approved and blocked patches to drive the appropriate package manager for each node type: 
+ Amazon Linux, Amazon Linux 2, CentOS, Oracle Linux, and RHEL 7 managed nodes use YUM\. For YUM operations, Patch Manager requires `Python 2.6` or a later supported version \(2\.6 \- 3\.9\)\. 
+ RHEL 8 managed nodes use DNF\. For DNF operations, Patch Manager requires a supported version of `Python 2` or `Python 3` \(2\.6 \- 3\.9\)\. \(Neither version is installed by default on RHEL 8\. You must install one or the other manually\.\)
+ Debian Server, Raspberry Pi OS, and Ubuntu Server instances use APT\. For APT operations, Patch Manager requires a supported version of `Python 3` \(3\.0 \- 3\.9\)\.
+ SUSE Linux Enterprise Server managed nodes use Zypper\. For Zypper operations, Patch Manager requires `Python 2.6` or a later supported version \(2\.6 \- 3\.9\)\.

------
#### [ macOS ]

On macOS managed nodes, the `AWS-RunPatchBaseline` document invokes a Python module, which in turn downloads a snapshot of the patch baseline that applies to the managed node\. Next, a Python subprocess invokes the AWS Command Line Interface \(AWS CLI\) on the node to retrieve the installation and update information for the specified package managers and to drive the appropriate package manager for each update package\.

------

Each snapshot is specific to an AWS account, patch group, operating system, and snapshot ID\. The snapshot is delivered through a presigned Amazon Simple Storage Service \(Amazon S3\) URL, which expires 24 hours after the snapshot is created\. After the URL expires, however, if you want to apply the same snapshot content to other managed nodes, you can generate a new presigned Amazon S3 URL up to 3 days after the snapshot was created\. To do this, use the [get\-deployable\-patch\-snapshot\-for\-instance](https://docs.aws.amazon.com/cli/latest/reference/ssm/get-deployable-patch-snapshot-for-instance.html) command\. 

After all approved and applicable updates have been installed, with reboots performed as necessary, patch compliance information is generated on a managed node and reported back to Patch Manager\. 

**Note**  
If the `RebootOption` parameter is set to `NoReboot` in the `AWS-RunPatchBaseline` document, the managed node isn't rebooted after Patch Manager runs\. For more information, see [Parameter name: `RebootOption`](#patch-manager-aws-runpatchbaseline-parameters-norebootoption)\.

For information about viewing patch compliance data, see [About patch compliance](sysman-compliance-about.md#sysman-compliance-monitor-patch)\. 

## `AWS-RunPatchBaseline` parameters<a name="patch-manager-aws-runpatchbaseline-parameters"></a>

`AWS-RunPatchBaseline` supports five parameters\. The `Operation` parameter is required\. The `InstallOverrideList`, `BaselineOverride`, and `RebootOption` parameters are optional\. `Snapshot-ID` is technically optional, but we recommend that you supply a custom value for it when you run `AWS-RunPatchBaseline` outside of a maintenance window\. Patch Manager can supply the custom value automatically when the document is run as part of a maintenance window operation\.

**Topics**
+ [Parameter name: `Operation`](#patch-manager-aws-runpatchbaseline-parameters-operation)
+ [Parameter name: `AssociationId`](#patch-manager-aws-runpatchbaseline-parameters-association-id)
+ [Parameter name: `Snapshot ID`](#patch-manager-aws-runpatchbaseline-parameters-snapshot-id)
+ [Parameter name: `InstallOverrideList`](#patch-manager-aws-runpatchbaseline-parameters-installoverridelist)
+ [Parameter name: `RebootOption`](#patch-manager-aws-runpatchbaseline-parameters-norebootoption)
+ [Parameter name: `BaselineOverride`](#patch-manager-aws-runpatchbaseline-parameters-baselineoverride)

### Parameter name: `Operation`<a name="patch-manager-aws-runpatchbaseline-parameters-operation"></a>

**Usage**: Required\.

**Options**: `Scan` \| `Install`\. 

Scan  
When you choose the `Scan` option, `AWS-RunPatchBaseline` determines the patch compliance state of the managed node and reports this information back to Patch Manager\. `Scan` doesn't prompt updates to be installed or managed nodes to be rebooted\. Instead, the operation identifies where updates are missing that are approved and applicable to the node\. 

Install  
When you choose the `Install` option, `AWS-RunPatchBaseline` attempts to install the approved and applicable updates that are missing from the managed node\. Patch compliance information generated as part of an `Install` operation doesn't list any missing updates, but might report updates that are in a failed state if the installation of the update didn't succeed for any reason\. Whenever an update is installed on a managed node, the node is rebooted to ensure the update is both installed and active\. \(Exception: If the `RebootOption` parameter is set to `NoReboot` in the `AWS-RunPatchBaseline` document, the managed node isn't rebooted after Patch Manager runs\. For more information, see [Parameter name: `RebootOption`](#patch-manager-aws-runpatchbaseline-parameters-norebootoption)\.\)  
If a patch specified by the baseline rules is installed *before* Patch Manager updates the managed node, the system might not reboot as expected\. This can happen when a patch is installed manually by a user or installed automatically by another program, such as the `unattended-upgrades` package on Ubuntu Server\.

### Parameter name: `AssociationId`<a name="patch-manager-aws-runpatchbaseline-parameters-association-id"></a>

**Usage**: Optional\.

`AssociationId` is the ID of an existing association in State Manager, a capability of AWS Systems Manager\. It's used by Patch Manager to add compliance data to a specified association\. This association is related to a patching operation that's [set up in a patch policy in Quick Setup](quick-setup-patch-manager.md)\. 

**Note**  
With the `AWS-RunPatchBaseline`, if an `AssociationId` value is provided along with a patch policy baseline override, patching is done as a `PatchPolicy` operation and the `ExecutionType` value reported in `AWS:ComplianceItem` is also `PatchPolicy`\. If no `AssociationId` value is provided, patching is done as a `Command` operation and the `ExecutionType` value report in on the `AWS:ComplianceItem` submitted is also `Command`\. 

If you don't already have an association you want to use, you can create one by running [create\-association](https://docs.aws.amazon.com/cli/latest/reference/ssm/create-association.html) the command\. 

### Parameter name: `Snapshot ID`<a name="patch-manager-aws-runpatchbaseline-parameters-snapshot-id"></a>

**Usage**: Optional\.

`Snapshot ID` is a unique ID \(GUID\) used by Patch Manager to ensure that a set of managed nodes that are patched in a single operation all have the exact same set of approved patches\. Although the parameter is defined as optional, our best practice recommendation depends on whether or not you're running `AWS-RunPatchBaseline` in a maintenance window, as described in the following table\.


**`AWS-RunPatchBaseline` best practices**  

| Mode | Best practice | Details | 
| --- | --- | --- | 
| Running AWS\-RunPatchBaseline inside a maintenance window | Don't supply a Snapshot ID\. Patch Manager will supply it for you\. |  If you use a maintenance window to run `AWS-RunPatchBaseline`, you shouldn't provide your own generated Snapshot ID\. In this scenario, Systems Manager provides a GUID value based on the maintenance window execution ID\. This ensures that a correct ID is used for all the invocations of `AWS-RunPatchBaseline` in that maintenance window\.  If you do specify a value in this scenario, note that the snapshot of the patch baseline might not remain in place for more than 3 days\. After that, a new snapshot will be generated even if you specify the same ID after the snapshot expires\.   | 
| Running AWS\-RunPatchBaseline outside of a maintenance window | Generate and specify a custom GUID value for the Snapshot ID\.¹ |  When you aren't using a maintenance window to run `AWS-RunPatchBaseline`, we recommend that you generate and specify a unique Snapshot ID for each patch baseline, particularly if you're running the `AWS-RunPatchBaseline` document on multiple managed nodes in the same operation\. If you don't specify an ID in this scenario, Systems Manager generates a different Snapshot ID for each managed node the command is sent to\. This might result in varying sets of patches being specified among the managed nodes\. For instance, say that you're running the `AWS-RunPatchBaseline` document directly through Run Command, a capability of AWS Systems Manager, and targeting a group of 50 managed nodes\. Specifying a custom Snapshot ID results in the generation of a single baseline snapshot that is used to evaluate and patch all the nodes, ensuring that they end up in a consistent state\.   | 
|  ¹ You can use any tool capable of generating a GUID to generate a value for the Snapshot ID parameter\. For example, in PowerShell, you can use the `New-Guid` cmdlet to generate a GUID in the format of `12345699-9405-4f69-bc5e-9315aEXAMPLE`\.  | 

### Parameter name: `InstallOverrideList`<a name="patch-manager-aws-runpatchbaseline-parameters-installoverridelist"></a>

**Usage**: Optional\.

Using `InstallOverrideList`, you specify an https URL or an Amazon S3 path\-style URL to a list of patches to be installed\. This patch installation list, which you maintain in YAML format, overrides the patches specified by the current default patch baseline\. This provides you with more granular control over which patches are installed on your managed nodes\. 

Be aware that compliance reports reflect patch states according to what’s specified in the patch baseline, not what you specify in an `InstallOverrideList` list of patches\. In other words, Scan operations ignore the `InstallOverrideList` parameter\. This is to ensure that compliance reports consistently reflect patch states according to policy rather than what was approved for a specific patching operation\. 

For a description of how you might use the `InstallOverrideList` parameter to apply different types of patches to a target group, on different maintenance window schedules, while still using a single patch baseline, see [Sample scenario for using the InstallOverrideList parameter in `AWS-RunPatchBaseline` or `AWS-RunPatchBaselineAssociation`](patch-manager-override-lists.md)\.

**Valid URL formats**

**Note**  
If your file is stored in a publicly available bucket, you can specify either an https URL format or an Amazon S3 path\-style URL\. If your file is stored in a private bucket, you must specify an Amazon S3 path\-style URL\.
+ **https URL format**:

  ```
  https://s3.aws-api-domain/DOC-EXAMPLE-BUCKET/my-windows-override-list.yaml
  ```
+ **Amazon S3 path\-style URL**:

  ```
  s3://DOC-EXAMPLE-BUCKET/my-windows-override-list.yaml
  ```

**Valid YAML content formats**

The formats you use to specify patches in your list depends on the operating system of your managed node\. The general format, however, is as follows:

```
patches:
    - 
        id: '{patch-d}'
        title: '{patch-title}'
        {additional-fields}:{values}
```

Although you can provide additional fields in your YAML file, they're ignored during patch operations\.

In addition, we recommend verifying that the format of your YAML file is valid before adding or updating the list in your S3 bucket\. For more information about the YAML format, see [yaml\.org](http://www.yaml.org)\. For validation tool options, perform a web search for "yaml format validators"\.

------
#### [ Linux ]

**id**  
The **id** field is required\. Use it to specify patches using the package name and architecture\. For example: `'dhclient.x86_64'`\. You can use wildcards in id to indicate multiple packages\. For example: `'dhcp*'` and `'dhcp*1.*'`\.

**Title**  
The **title** field is optional, but on Linux systems it does provide additional filtering capabilities\. If you use **title**, it should contain the package version information in the one of the following formats:

YUM/SUSE Linux Enterprise Server \(SLES\):

```
{name}.{architecture}:{epoch}:{version}-{release}
```

APT

```
{name}.{architecture}:{version}
```

For Linux patch titles, you can use one or more wildcards in any position to expand the number of package matches\. For example: `'*32:9.8.2-0.*.rc1.57.amzn1'`\. 

For example: 
+ apt package version 1\.2\.25 is currently installed on your managed node, but version 1\.2\.27 is now available\. 
+ You add apt\.amd64 version 1\.2\.27 to the patch list\. It depends on apt utils\.amd64 version 1\.2\.27, but apt\-utils\.amd64 version 1\.2\.25 is specified in the list\. 

In this case, apt version 1\.2\.27 will be blocked from installation and reported as “Failed\-NonCompliant\.”

------
#### [ Windows ]

**id**  
The **id** field is required\. Use it to specify patches using Microsoft Knowledge Base IDs \(for example, KB2736693\) and Microsoft Security Bulletin IDs \(for example, MS17\-023\)\. 

Any other fields you want to provide in a patch list for Windows are optional and are for your own informational use only\. You can use additional fields such as **title**, **classification**, **severity**, or anything else for providing more detailed information about the specified patches\.

------
#### [ macOS ]

**id**  
The **id** field is required\. The value for the **id** field can be supplied using either a `{package-name}.{package-version}` format or a \{package\_name\} format\.

------

**Sample patch lists**
+ **Amazon Linux**

  ```
  patches:
      -
          id: 'kernel.x86_64'
      -
          id: 'bind*.x86_64'
          title: '32:9.8.2-0.62.rc1.57.amzn1'
      -
          id: 'glibc*'
      -
          id: 'dhclient*'
          title: '*12:4.1.1-53.P1.28.amzn1'
      -
          id: 'dhcp*'
          title: '*10:3.1.1-50.P1.26.amzn1'
  ```
+ **CentOS**

  ```
  patches:
      -
          id: 'kernel.x86_64'
      -
          id: 'bind*.x86_64'
          title: '32:9.8.2-0.62.rc1.57.amzn1'
      -
          id: 'glibc*'
      -
          id: 'dhclient*'
          title: '*12:4.1.1-53.P1.28.amzn1'
      -
          id: 'dhcp*'
          title: '*10:3.1.1-50.P1.26.amzn1'
  ```
+ **Debian Server**

  ```
  patches:
      -
          id: 'apparmor.amd64'
          title: '2.10.95-0ubuntu2.9'
      -
          id: 'cryptsetup.amd64'
          title: '*2:1.6.6-5ubuntu2.1'
      -
          id: 'cryptsetup-bin.*'
          title: '*2:1.6.6-5ubuntu2.1'
      -
          id: 'apt.amd64'
          title: '*1.2.27'
      -
          id: 'apt-utils.amd64'
          title: '*1.2.25'
  ```
+ **macOS**

  ```
  patches:
      -
          id: 'XProtectPlistConfigData'
      -
          id: 'MRTConfigData.1.61'
      -
          id: 'Command Line Tools for Xcode.11.5'
      -
          id: 'Gatekeeper Configuration Data'
  ```
+ **Oracle Linux**

  ```
  patches:
      -
          id: 'audit-libs.x86_64'
          title: '*2.8.5-4.el7'
      -
          id: 'curl.x86_64'
          title: '*.el7'
      -
          id: 'grub2.x86_64'
          title: 'grub2.x86_64:1:2.02-0.81.0.1.el7'
      -
          id: 'grub2.x86_64'
          title: 'grub2.x86_64:1:*-0.81.0.1.el7'
  ```
+ **Red Hat Enterprise Linux \(RHEL\)**

  ```
  patches:
      -
          id: 'NetworkManager.x86_64'
          title: '*1:1.10.2-14.el7_5'
      -
          id: 'NetworkManager-*.x86_64'
          title: '*1:1.10.2-14.el7_5'
      -
          id: 'audit.x86_64'
          title: '*0:2.8.1-3.el7'
      -
          id: 'dhclient.x86_64'
          title: '*.el7_5.1'
      -
          id: 'dhcp*.x86_64'
          title: '*12:5.2.5-68.el7'
  ```
+ **SUSE Linux Enterprise Server \(SLES\)**

  ```
  patches:
      -
          id: 'amazon-ssm-agent.x86_64'
      -
          id: 'binutils'
          title: '*0:2.26.1-9.12.1'
      -
          id: 'glibc*.x86_64'
          title: '*2.19*'
      -
          id: 'dhcp*'
          title: '0:4.3.3-9.1'
      -
          id: 'lib*'
  ```
+ **Ubuntu Server **

  ```
  patches:
      -
          id: 'apparmor.amd64'
          title: '2.10.95-0ubuntu2.9'
      -
          id: 'cryptsetup.amd64'
          title: '*2:1.6.6-5ubuntu2.1'
      -
          id: 'cryptsetup-bin.*'
          title: '*2:1.6.6-5ubuntu2.1'
      -
          id: 'apt.amd64'
          title: '*1.2.27'
      -
          id: 'apt-utils.amd64'
          title: '*1.2.25'
  ```
+ **Windows**

  ```
  patches:
      -
          id: 'KB4284819'
          title: '2018-06 Cumulative Update for Windows Server 2016 (1709) for x64-based Systems (KB4284819)'
      -
          id: 'KB4284833'
      -
          id: 'KB4284835'
          title: '2018-06 Cumulative Update for Windows Server 2016 (1803) for x64-based Systems (KB4284835)'
      -
          id: 'KB4284880'
      -
          id: 'KB4338814'
  ```

### Parameter name: `RebootOption`<a name="patch-manager-aws-runpatchbaseline-parameters-norebootoption"></a>

**Usage**: Optional\.

**Options**: `RebootIfNeeded` \| `NoReboot` 

**Default**: `RebootIfNeeded`

**Warning**  
The default option is `RebootIfNeeded`\. Be sure to select the correct option for your use case\. For example, if your managed nodes must reboot immediately to complete a configuration process, choose `RebootIfNeeded`\. Or, if you need to maintain managed node availability until a scheduled reboot time, choose `NoReboot`\.

**Important**  
We don’t recommend using Patch Manager for patching cluster instances in Amazon EMR \(previously called Amazon Elastic MapReduce\)\. In particular, don’t select the `RebootIfNeeded` option for the `RebootOption` parameter\. \(This option is available in the SSM Command documents for patching `AWS-RunPatchBaseline`, `AWS-RunPatchBaselineAssociation`, and `AWS-RunPatchBaselineWithHooks`\.\)  
The underlying commands for patching using Patch Manager use `yum` and `dnf` commands\. Therefore, the operations result in incompatibilities because of how packages are installed\. For information about the preferred methods for updating software on Amazon EMR clusters, see [Using the default AMI for Amazon EMR](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-default-ami.html) in the *Amazon EMR Management Guide*\.

RebootIfNeeded  
When you choose the `RebootIfNeeded` option, the managed node is rebooted in either of the following cases:   
+ Patch Manager installed one or more patches\. 

  Patch Manager doesn't evaluate whether a reboot is *required* by the patch\. The system is rebooted even if the patch doesn't require a reboot\.
+ Patch Manager detects one or more patches with a status of `INSTALLED_PENDING_REBOOT` during the `Install` operation\. 

  The `INSTALLED_PENDING_REBOOT` status can mean that the option `NoReboot` was selected the last time the `Install` operation was run\.
**Note**  
Patches installed outside of Patch Manager are never given a status of `INSTALLED_PENDING_REBOOT`\.
Rebooting managed nodes in these two cases ensures that updated packages are flushed from memory and keeps patching and rebooting behavior consistent across all operating systems\.

NoReboot  
When you choose the `NoReboot` option, Patch Manager doesn't reboot a managed node even if it installed patches during the `Install` operation\. This option is useful if you know that your managed nodes don't require rebooting after patches are applied, or you have applications or processes running on a node that shouldn't be disrupted by a patching operation reboot\. It's also useful when you want more control over the timing of managed node reboots, such as by using a maintenance window\.  
If you choose the `NoReboot` option and a patch is installed, the patch is assigned a status of `InstalledPendingReboot`\. The managed node itself, however, is marked as `Non-Compliant`\. After a reboot occurs and a `Scan` operation is run, the managed node status is updated to `Compliant`\.

**Patch installation tracking file**: To track patch installation, especially patches that were installed since the last system reboot, Systems Manager maintains a file on the managed node\.

**Important**  
Don't delete or modify the tracking file\. If this file is deleted or corrupted, the patch compliance report for the managed node is inaccurate\. If this happens, reboot the node and run a patch Scan operation to restore the file\.

This tracking file is stored in the following locations on your managed nodes:
+ Linux operating systems: 
  + `/var/log/amazon/ssm/patch-configuration/patch-states-configuration.json`
  + `/var/log/amazon/ssm/patch-configuration/patch-inventory-from-last-operation.json`
+ Windows Server operating system:
  + `C:\ProgramData\Amazon\PatchBaselineOperations\State\PatchStatesConfiguration.json`
  + `C:\ProgramData\Amazon\PatchBaselineOperations\State\PatchInventoryFromLastOperation.json`

### Parameter name: `BaselineOverride`<a name="patch-manager-aws-runpatchbaseline-parameters-baselineoverride"></a>

**Usage**: Optional\.

You can define patching preferences at runtime using the `BaselineOverride` parameter\. This baseline override is maintained as a JSON object in an S3 bucket\. It ensures patching operations use the provided baselines that match the host operating system instead of applying the rules from the default patch baseline

For more information about how to use the `BaselineOverride` parameter, see [Using the BaselineOverride parameter](patch-manager-baselineoverride-parameter.md)\.