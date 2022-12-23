# Patching out\-of\-compliance managed nodes<a name="patch-compliance-remediate"></a>

Many of the same AWS Systems Manager tools and processes you can use to check managed nodes for patch compliance can be used to bring nodes into compliance with the patch rules that currently apply to them\. To bring managed nodes into patch compliance, Patch Manager, a capability of AWS Systems Manager, must run a `Scan and install` operation\. \(If your goal is only to identify out\-of\-compliance managed nodes and not remediate them, run a `Scan` operation instead\. For more information, see [Identifying out\-of\-compliance managed nodes](patch-compliance-identify.md)\.\)

**Install patches using Systems Manager**  
You can choose from several tools to run a `Scan and install` operation:
+ \(Recommended\) Configure a patch policy in Quick Setup, a capability of Systems Manager, that lets you install missing patches on a schedule for an entire organization, a subset of organizational units, or a single AWS account\. For more information, see [Automate organization\-wide patching using a Quick Setup patch policyAutomate organization\-wide patching](quick-setup-patch-manager.md)\.
+ Create a maintenance window that uses the Systems Manager document \(SSM document\) `AWS-RunPatchBaseline` in a Run Command task type\. For information, see [Walkthrough: Creating a maintenance window for patching \(console\)](sysman-patch-mw-console.md)\.
+ Manually run `AWS-RunPatchBaseline` in a Run Command operation\. For information, see [Running commands from the console](running-commands-console.md)\.
+ Install patches on demand using the **Patch now** option\. For information, see [Patching managed nodes on demand](patch-on-demand.md)\.