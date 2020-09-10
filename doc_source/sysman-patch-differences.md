# Key differences between Linux and Windows patching<a name="sysman-patch-differences"></a>

This topic describes important differences between Linux and Windows patching\.

**Note**  
To patch Linux instances, your instances must be running SSM Agent version 2\.0\.834\.0 or later\.  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

**Difference 1: Patch evaluation**  
**Linux**  
For Linux patching, Systems Manager evaluates patch baseline rules and the list of approved and rejected patches on *each* managed instance\. Systems Manager must evaluate patching on each instance because the service retrieves the list of known patches and updates from the repositories that are configured on the instance\.
**Windows**  
Patch Manager uses different processes on Windows managed instances and Linux managed instances in order to evaluate which patches should be present\. For Windows patching, Systems Manager evaluates patch baseline rules and the list of approved and rejected patches *directly in the service*\. It can do this because Windows patches are pulled from a single repository \(Windows Update\)\.

**Difference 2: `Not Applicable` patches**  
Due to the large number of available packages for Linux operating systems, Systems Manager does not report details about patches in the *Not Applicable* state\. A `Not Applicable` patch is, for example, a patch for Apache software when the instance does not have Apache installed\. Systems Manager does report the number of `Not Applicable` patches in the summary, but if you call the [DescribeInstancePatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeInstancePatches.html) API for an instance, the returned data does not include patches with a state of `Not Applicable`\. This behavior is different from Windows\.

**Difference 3: SSM document support**  
The `AWS-ApplyPatchBaseline` SSM document doesn't support Linux instances\. For applying patch baselines to both Windows Server and Linux instances, the recommended SSM document is `AWS-RunPatchBaseline`\. For more information, see [About SSM documents for patching instances](patch-manager-ssm-documents.md) and [About the SSM document AWS\-RunPatchBaseline](patch-manager-about-aws-runpatchbaseline.md)\.

**Difference 4: Application patches**  
Patch Manager's primary focus is applying patches to operating systems\. However, you can also use Patch Manager to apply patches to some applications on your instances\.  
**Linux**  
On Linux operating systems, Patch Manager uses the configured repositories for updates, and does not differentiate between operating systems and application patches\. You can use Patch Manager to define which repositories to fetch updates from\. For more information, see [How to specify an alternative patch source repository \(Linux\)](patch-manager-how-it-works-alt-source-repository.md)\.
**Windows**  
On Windows Server instances, you can apply approval rules, as well as *Approved* and *Rejected* patch exceptions, for applications released by Microsoft, such as Microsoft Word 2011 and Microsoft Exchange Server 2016\. For more information, see [Working with custom patch baselines](sysman-patch-baseline-console.md)\.