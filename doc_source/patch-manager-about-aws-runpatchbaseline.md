# About the SSM Document AWS\-RunPatchBaseline<a name="patch-manager-about-aws-runpatchbaseline"></a>

AWS Systems Manager supports an SSM document for Patch Manager, **AWS\-RunPatchBaseline**, which performs security patching operations on instances\. This document supports both Linux and Windows instances, so it can be reliably run on either type of instance when managed by Systems Manager\. The document will perform the appropriate actions for each platform\.

**Note**  
Patch Manager also supports the legacy SSM document **AWS\-ApplyPatchBaseline**\. However, this document supports patching on Windows instances only\. We encourage you to use **AWS\-RunPatchBaseline** instead because it supports patching on both Linux and Windows instances\. Version 2\.0\.834\.0 or later of SSM Agent is required in order to use the **AWS\-RunPatchBaseline** document\.

On Windows systems:  
On Windows instances, the **AWS\-RunPatchBaseline** document downloads and invokes a PowerShell module, which in turn downloads a snapshot of the patch baseline that applies to the instance\. This patch baseline snapshot is passed to the Windows Update API, which controls downloading and installing the approved patches as appropriate\.

On Linux systems:  
On Linux instances, the **AWS\-RunPatchBaseline** document invokes a Python module, which in turn downloads a snapshot of the patch baseline that applies to the instance\. This patch baseline snapshot uses the defined rules and lists of approved and blocked patches to drive the appropriate package manager for each instance type:   
+  Amazon Linux, Amazon Linux 2, and RHEL instances use YUM\. For YUM operations, Patch Manager requires Python 2\.6 or later\. 
+  Ubuntu Server instances use APT\. For APT operations, Patch Manager requires Python 3\. 
+ SUSE Linux Enterprise Server instances use Zypper\. For Zypper operations, Patch Manager requires Python 2\.6 or later\.

After all approved and applicable updates have been installed, with reboots performed as necessary, patch compliance information is generated on an instance and reported back to Patch Manager\. For information about viewing patch compliance data, see [About Patch Compliance](sysman-compliance-about.md#sysman-compliance-monitor-patch)\. 

## AWS\-RunPatchBaseline Parameters<a name="patch-manager-about-aws-runpatchbaseline-parameters"></a>

**AWS\-RunPatchBaseline** supports two parameters\. The `Operation` parameter is required\. `Snapshot-ID` is technically optional, but we recommend that you supply a custom value for it when you run **AWS\-RunPatchBaseline** outside of a Maintenance Window, and let Patch Manager supply the value automatically when the document is run as part of a Maintenance Window operation\.

**Topics**
+ [Parameter name: `Operation`](#patch-manager-about-aws-runpatchbaseline-parameters-operation)
+ [Parameter name: `Snapshot ID`](#patch-manager-about-aws-runpatchbaseline-parameters-snapshot-id)

### Parameter name: `Operation`<a name="patch-manager-about-aws-runpatchbaseline-parameters-operation"></a>

**Usage**: Required\.

**Options**: Scan \| Install\. 

Scan  
When you choose the Scan option, **AWS\-RunPatchBaseline** determines the patch compliance state of the instance and reports this information back to Patch Manager\. Scan does not prompt updates to be installed or instances to be rebooted\. Instead, the operation identifies where updates are missing that are approved and applicable to the instance\. 

Install  
When you choose the Install option, **AWS\-RunPatchBaseline** attempts to install the approved and applicable updates that are missing from the instance\. Patch compliance information generated as part of an Install operation does not list any missing updates, but might report updates that are in a failed state if the installation of the update did not succeed for any reason\. Whenever an update is installed on an instance, the instance is rebooted to ensure the update is both installed and active\.

### Parameter name: `Snapshot ID`<a name="patch-manager-about-aws-runpatchbaseline-parameters-snapshot-id"></a>

**Usage**: Optional\.

`Snapshot ID` is a unique ID \(GUID\) used by Patch Manager to ensure that a set of instances that are patched in a single operation all have the exact same set of approved patches\. Although the parameter is defined as optional, our best practice recommendation depends on whether or not you are running **AWS\-RunPatchBaseline** in a Maintenance Window, as described in the following table\.


**AWS\-RunPatchBaseline Best Practices**  

| Mode | Best Practice | Details | 
| --- | --- | --- | 
| Running AWS\-RunPatchBaseline inside a Maintenance Window | Do not supply a Snapshot ID\. Patch Manager will supply it for you\. |  If you use a Maintenance Window to run **AWS\-RunPatchBaseline**, you should not provide your own generated Snapshot ID\. In this scenario, Systems Manager provides a GUID value based on the Maintenance Window execution ID\. This ensures that a correct ID is used for all the invocations of **AWS\-RunPatchBaseline** in that Maintenance Window\.  If you do specify a value in this scenario, note that the snapshot of the patch baseline might not remain in place for more than 24 hours\. After that, a new snapshot will be generated even if you specify the same ID after the snapshot expires\.   | 
| Running AWS\-RunPatchBaseline outside of a Maintenance Window | Generate and specify a custom GUID value for the Snapshot ID\.¹ |  When you are not using a Maintenance Window to run **AWS\-RunPatchBaseline**, we recommend that you generate and specify a unique Snapshot ID for each patch baseline, particularly if you are running the **AWS\-RunPatchBaseline** document on multiple instances in the same operation\. If you do not specify an ID in this scenario, Systems Manager generates a different Snapshot ID for each instance the command is sent to\. This might result in varying sets of patches being specified among the instances\. For instance, say that you are running the **AWS\-RunPatchBaseline** document directly via Run Command and targeting a group of 50 instances\. Specifying a custom Snapshot ID results in the generation of a single baseline snapshot that is used to evaluate and patch all the instances, ensuring that they end up in a consistent state\.   | 
|  ¹ You can use any tool capable of generating a GUID to generate a value for the Snapshot ID parameter\. For example, in PowerShell, you can use the `New-Guid` cmdlet to generate a GUID in the format of `12345699-9405-4f69-bc5e-9315aEXAMPLE`\.  | 