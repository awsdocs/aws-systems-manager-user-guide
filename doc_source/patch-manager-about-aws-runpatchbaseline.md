# About the SSM Document AWS\-RunPatchBaseline<a name="patch-manager-about-aws-runpatchbaseline"></a>

AWS Systems Manager supports an SSM document for Patch Manager, **AWS\-RunPatchBaseline**, which performs patching operations on instances for both security related and other types of updates\. You can use the document AWS\-RunPatchBaseline to apply patches for both operating systems and applications\. \(On Windows Server, application support is limited to updates for Microsoft applications\.\)

This document supports both Linux and Windows instances, so it can be reliably run on either type of instance when managed by Systems Manager\. The document will perform the appropriate actions for each platform\.

**Note**  
Patch Manager also supports the legacy SSM document **AWS\-ApplyPatchBaseline**\. However, this document supports patching on Windows instances only\. We encourage you to use **AWS\-RunPatchBaseline** instead because it supports patching on both Linux and Windows instances\. Version 2\.0\.834\.0 or later of SSM Agent is required in order to use the **AWS\-RunPatchBaseline** document\.

------
#### [ Windows ]

On Windows instances, the **AWS\-RunPatchBaseline** document downloads and invokes a PowerShell module, which in turn downloads a snapshot of the patch baseline that applies to the instance\. This patch baseline snapshot is passed to the Windows Update API, which controls downloading and installing the approved patches as appropriate\.

------
#### [ Linux ]

On Linux instances, the **AWS\-RunPatchBaseline** document invokes a Python module, which in turn downloads a snapshot of the patch baseline that applies to the instance\. This patch baseline snapshot uses the defined rules and lists of approved and blocked patches to drive the appropriate package manager for each instance type: 
+ Amazon Linux, Amazon Linux 2, CentOS, and RHEL instances use YUM\. For YUM operations, Patch Manager requires Python 2\.6 or later\. 
+  Ubuntu Server instances use APT\. For APT operations, Patch Manager requires Python 3\. 
+ SUSE Linux Enterprise Server instances use Zypper\. For Zypper operations, Patch Manager requires Python 2\.6 or later\.

------

After all approved and applicable updates have been installed, with reboots performed as necessary, patch compliance information is generated on an instance and reported back to Patch Manager\. 

**Note**  
If the `RebootOption` parameter is set to `NoReboot` in the `AWS-RunPatchBaseline` document, the instance is not rebooted after Patch Manager runs\. For more information, see [Parameter name: `RebootOption`](#patch-manager-about-aws-runpatchbaseline-parameters-norebootoption)\.

For information about viewing patch compliance data, see [About Patch Compliance](sysman-compliance-about.md#sysman-compliance-monitor-patch)\. 

## AWS\-RunPatchBaseline Parameters<a name="patch-manager-about-aws-runpatchbaseline-parameters"></a>

**AWS\-RunPatchBaseline** supports four parameters\. The `Operation` parameter is required\. The `InstallOverrideList` and `RebootOption` parameters are optional\. `Snapshot-ID` is technically optional, but we recommend that you supply a custom value for it when you run **AWS\-RunPatchBaseline** outside of a maintenance window, and let Patch Manager supply the value automatically when the document is run as part of a maintenance window operation\.

**Topics**
+ [Parameter name: `Operation`](#patch-manager-about-aws-runpatchbaseline-parameters-operation)
+ [Parameter name: `Snapshot ID`](#patch-manager-about-aws-runpatchbaseline-parameters-snapshot-id)
+ [Parameter name: `InstallOverrideList`](#patch-manager-about-aws-runpatchbaseline-parameters-installoverridelist)
+ [Parameter name: `RebootOption`](#patch-manager-about-aws-runpatchbaseline-parameters-norebootoption)

### Parameter name: `Operation`<a name="patch-manager-about-aws-runpatchbaseline-parameters-operation"></a>

**Usage**: Required\.

**Options**: `Scan` \| `Install`\. 

Scan  
When you choose the `Scan` option, **AWS\-RunPatchBaseline** determines the patch compliance state of the instance and reports this information back to Patch Manager\. `Scan` does not prompt updates to be installed or instances to be rebooted\. Instead, the operation identifies where updates are missing that are approved and applicable to the instance\. 

Install  
When you choose the `Install` option, **AWS\-RunPatchBaseline** attempts to install the approved and applicable updates that are missing from the instance\. Patch compliance information generated as part of an `Install` operation does not list any missing updates, but might report updates that are in a failed state if the installation of the update did not succeed for any reason\. Whenever an update is installed on an instance, the instance is rebooted to ensure the update is both installed and active\. \(Exception: If the `RebootOption` parameter is set to `NoReboot` in the `AWS-RunPatchBaseline` document, the instance is not rebooted after Patch Manager runs\. For more information, see [Parameter name: `RebootOption`](#patch-manager-about-aws-runpatchbaseline-parameters-norebootoption)\.\)

### Parameter name: `Snapshot ID`<a name="patch-manager-about-aws-runpatchbaseline-parameters-snapshot-id"></a>

**Usage**: Optional\.

`Snapshot ID` is a unique ID \(GUID\) used by Patch Manager to ensure that a set of instances that are patched in a single operation all have the exact same set of approved patches\. Although the parameter is defined as optional, our best practice recommendation depends on whether or not you are running **AWS\-RunPatchBaseline** in a maintenance window, as described in the following table\.


**AWS\-RunPatchBaseline Best Practices**  

| Mode | Best Practice | Details | 
| --- | --- | --- | 
| Running AWS\-RunPatchBaseline inside a maintenance window | Do not supply a Snapshot ID\. Patch Manager will supply it for you\. |  If you use a maintenance window to run **AWS\-RunPatchBaseline**, you should not provide your own generated Snapshot ID\. In this scenario, Systems Manager provides a GUID value based on the maintenance window execution ID\. This ensures that a correct ID is used for all the invocations of **AWS\-RunPatchBaseline** in that maintenance window\.  If you do specify a value in this scenario, note that the snapshot of the patch baseline might not remain in place for more than 24 hours\. After that, a new snapshot will be generated even if you specify the same ID after the snapshot expires\.   | 
| Running AWS\-RunPatchBaseline outside of a maintenance window | Generate and specify a custom GUID value for the Snapshot ID\.¹ |  When you are not using a maintenance window to run **AWS\-RunPatchBaseline**, we recommend that you generate and specify a unique Snapshot ID for each patch baseline, particularly if you are running the **AWS\-RunPatchBaseline** document on multiple instances in the same operation\. If you do not specify an ID in this scenario, Systems Manager generates a different Snapshot ID for each instance the command is sent to\. This might result in varying sets of patches being specified among the instances\. For instance, say that you are running the **AWS\-RunPatchBaseline** document directly via Run Command and targeting a group of 50 instances\. Specifying a custom Snapshot ID results in the generation of a single baseline snapshot that is used to evaluate and patch all the instances, ensuring that they end up in a consistent state\.   | 
|  ¹ You can use any tool capable of generating a GUID to generate a value for the Snapshot ID parameter\. For example, in PowerShell, you can use the `New-Guid` cmdlet to generate a GUID in the format of `12345699-9405-4f69-bc5e-9315aEXAMPLE`\.  | 

### Parameter name: `InstallOverrideList`<a name="patch-manager-about-aws-runpatchbaseline-parameters-installoverridelist"></a>

**Usage**: Optional\.

`InstallOverrideList` lets you specify an https URL or an Amazon Simple Storage Service \(Amazon S3\) path\-style URL to a list of patches to be installed\. This patch installation list, which you maintain in YAML format, overrides the patches specified by the current default patch baseline\. This provides you with more granular control over which patches are installed on your instances\. 

Be aware that compliance reports reflect patch states according to what’s specified in the patch baseline, not what you specify in an `InstallOverrideList` list of patches\. In other words, Scan operations ignore the `InstallOverrideList` parameter\. This is to ensure that compliance reports consistently reflect patch states according to policy rather than what was approved for a specific patching operation\. 

**Valid URL formats**
+ **https URL format**:

  ```
  https://s3.amazonaws.com/my-patch-approval-lists-bucket/my-windows-override-list.yaml
  ```
+ **Amazon S3 path\-style URL**:

  ```
  s3://my-patch-approval-lists-bucket/my-windows-override-list.yaml
  ```

**Valid YAML content formats**

The formats you use to specify patches in your list depends on the operating system of your instance\. The general format, however, is as follows:

```
patches:
    - 
        id: '{patch-d}'
        title: '{patch-title}'
        {additional-fields}:{values}
```

Although you can provide additional fields in your YAML file, they are ignored during patch operations\.

In addition, we recommend verifying that the format of your YAML file is valid before adding or updating the list in your S3 bucket\. For more information about the YAML format, see [yaml\.org](http://www.yaml.org)\. For validation tool options, perform a web search for "yaml format validators"\.
+ Microsoft Windows

**id**  
The **id** field is required\. Use it to specify patches using Microsoft Knowledge Base IDs \(for example, KB2736693\) and Microsoft Security Bulletin IDs \(for example, MS17\-023\)\. 

  Any other fields you want to provide in a patch list for Windows are optional and are for your own informational use only\. You can use additional fields such as **title**, **classification**, **severity**, or anything else for providing more detailed information about the specified patches\.
+ Linux

**id**  
The **id** field is required\. Use it to specify patches using the package name and architecture\. For example: `'dhclient.x86_64'`\. You can use wildcards in id to indicate multiple packages\. For example: `'dhcp*'` and `'dhcp*1.*'`\.

**title**  
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
  + apt package version 1\.2\.25 is currently installed on your instance, but version 1\.2\.27 is now available\. 
  + You add apt\.amd64 version 1\.2\.27 to the patch list\. It depends on apt utils\.amd64 version 1\.2\.27, but apt\-utils\.amd64 version 1\.2\.25 is specified in the list\. 

  In this case, apt version 1\.2\.27 will be blocked from installation and reported as “Failed\-NonCompliant\.”

**Other fields**  
Any other fields you want to provide in a patch list for Linux are optional and are for your own informational use only\. You can use additional fields such as **classification**, **severity**, or anything else for providing more detailed information about the specified patches\.

**Sample patch lists**
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
+ **APT**

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

### Parameter name: `RebootOption`<a name="patch-manager-about-aws-runpatchbaseline-parameters-norebootoption"></a>

**Usage**: Optional\.

**Options**: `RebootIfNeeded` \| `NoReboot`\. 

RebootIfNeeded  
When you choose the `RebootIfNeeded` option, the instance is rebooted if Patch Manager installed new patches, or if it detected any patches with a status of `INSTALLED_PENDING_REBOOT` during the `Install` operation\. The `INSTALLED_PENDING_REBOOT` status can mean that the option `NoReboot` was selected the last time the `Install` operation was run\. \(Patches installed outside of Patch Manager are never given a status of `INSTALLED_PENDING_REBOOT`\.\)  
When you choose the `RebootIfNeeded` option, Patch Manager does not evaluate whether a reboot is *required* by the patch\. A reboot occurs whenever there are missing packages or packages with a status of `INSTALLED_PENDING_REBOOT`\.

NoReboot  
When you choose the `NoReboot` option, Patch Manager does not reboot an instance even if it installed patches during the `Install` operation\. This option is useful if you know that your instances don't require rebooting after patches are applied, or you have applications or processes running on an instance that should not be disrupted by a patching operation reboot\. It is also useful when you want more control over the timing of instance reboots, such as by using a maintenance window\.

**Patch installation tracking file**: To track patch installation, especially patches that have been installed since the last system reboot, Systems Manager maintains a file on the managed instance\.

**Important**  
Do not delete or modify the tracking file\. If this file is deleted or corrupted, the patch compliance report for the instance is inaccurate\. If this happens, reboot the instance and run a patch Scan operation to restore the file\.

This tracking file is stored in the following locations on your managed instances:
+ Linux operating systems: `/var/log/amazon/ssm/patch-configuration/patch-states-configuration.json`
+ Windows Server operating system: `C:\ProgramData\Amazon\PatchBaselineOperations\State\PatchStatesConfiguration.json`