# Key Differences Between Linux and Windows Patching<a name="sysman-patch-differences"></a>

The following table describes important differences between Linux and Windows patching\.

**Note**  
To patch Linux instances, your instances must be running SSM Agent version 2\.0\.834\.0 or later\.  
SSM Agent is updated whenever changes are made to Systems Manager and when new capabilities are added\. To ensure that your instances are always running the newest version of SSM Agent, we recommend that you update the agent automatically whenever a new version is available using either of the following methods\.  
Use a State Manager association\. For information, see the State Manager topic [Automatically Update SSM Agent \(CLI\)](sysman-state-cli.md)\.
Use a Maintenance Window\. For information, see the Maintenance Window topics [Automatically Update SSM Agent \(AWS CLI\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/mw-walkthrough-cli.html) and [Automatically Update SSM Agent \(Console\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/mw-walkthrough-console.html)\. 
If you prefer to update SSM Agent on your instances manually, you can subscribe to notifications that AWS publishes when a new version of the agent is released\. For information, see [Subscribing to SSM Agent Notifications](ssm-agent-subscribe-notifications.md)\. For information about using Run Command to manually update one or more instances with the latest version, see [Update SSM Agent by using Run Command](rc-console.md#rc-console-agentexample)\.


****  

| Difference | Details | 
| --- | --- | 
|  Patch evaluation  |  For Linux patching, Systems Manager evaluates patch baseline rules and the list of approved and rejected patches on *each* managed instance\. Systems Manager must evaluate patching on each instance because the service retrieves the list of known patches and updates from the repositories that are configured on the instance\. Patch Manager uses different processes on Windows managed instances and Linux managed instances to evaluate which patches should be present\. For Windows patching, Systems Manager evaluates patch baseline rules and the list of approved and rejected patches *directly in the service*\. It can do this because Windows patches are pulled from a single repository \(Windows Update\)\.  | 
|  `Not Applicable` patches  |  Due to the large number of available packages for Linux operating systems, Systems Manager does not report details about patches in the *Not Applicable* state\. A `Not Applicable` patch is, for example, a patch for Apache software when the instance does not have Apache installed\. Systems Manager does report the number of `Not Applicable` patches in the summary, but if you call the [DescribeInstancePatches](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeInstancePatches.html) API for an instance, the returned data does not include patches with a state of `Not Applicable`\. This behavior is different from Windows\.  | 
| SSM document support |  The `AWS-ApplyPatchBaseline` SSM document doesn't support Linux instances\. For applying patch baselines to both Windows and Linux instances, the recommended SSM document is `AWS-RunPatchBaseline`\. For more information, see [About SSM Documents for Patching Instances](patch-manager-ssm-documents.md) and [About the SSM Document AWS\-RunPatchBaseline](patch-manager-about-aws-runpatchbaseline.md)\.  | 