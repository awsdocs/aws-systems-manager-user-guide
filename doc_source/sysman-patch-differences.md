# Important Differences Between Windows and Linux Patching<a name="sysman-patch-differences"></a>

The following table describes important differences between Windows and Linux patching\.

**Note**  
To patch Linux instances, your instances must be running SSM Agent version 2\.0\.834\.0 or later\. For information about updating the agent, see the section titled *Example: Update the SSM Agent* in [Running Commands from the Console](rc-console.md)\.  
The **AWS\-ApplyPatchBaseline ** SSM document doesn't support Linux instances\. For applying patch baselines to both Windows and Linux instances, the recommended SSM document is **AWS\-RunPatchBaseline**\. For more information, see [Overview of SSM Documents for Patching Instances](patch-manager-ssm-documents.md) and [About the SSM Document AWS\-RunPatchBaseline](patch-manager-about-aws-runpatchbaseline.md)\.


****  

| Difference | Details | 
| --- | --- | 
|  Patch evaluation  |  Patch Manager uses a different process to evaluate which patches should be present on Windows managed instances versus Linux managed instance\. For Windows patching, Systems Manager evaluates patch baseline rules and the list of approved and rejected patches *directly in the service*\. It can do this because Windows patches are pulled from a single repository \(Windows Update\)\. For Linux patching, Systems Manager evaluates patch baseline rules and the list of approved and rejected patches on *each* managed instance\. Systems Manager must evaluate patching on each instance because the service retrieves the list of known patches and updates from the repositories that are configured on the instance\.  | 
|  *Not Applicable* patches  |  Due to the large number of available packages for Linux operating systems, Systems Manager does not report details about patches in the *Not Applicable* state\. A Not Applicable patch is, for example, a patch for Apache software when the instance does not have Apache installed\. Systems Manager does report the number of Not Applicable patches in the summary, but if you call the [DescribeInstancePatches](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeInstancePatches.html) API for an instance, the returned data does not include patches with a state of Not Applicable\. This behavior is different from Windows\.  | 