# Working with Patch Manager \(console\)<a name="patch-manager-console"></a>

To use Patch Manager, a capability of AWS Systems Manager, complete the following tasks\. These tasks are described in more detail in this section\.

1. Verify that the AWS predefined patch baseline for each operating system type that you use meets your needs\. If it doesn't, create a patch baseline that defines a standard set of patches for that managed node type and set it as the default instead\.

1. Organize managed nodes into patch groups by using Amazon Elastic Compute Cloud \(Amazon EC2\) tags \(optional, but recommended\)\.

1. Do one of the following:
   + \(Recommended\) Configure a patch policy in Quick Setup, a capability of Systems Manager, that lets you install missing patches on a schedule for an entire organization, a subset of organizational units, or a single AWS account\. For more information, see [Automate organization\-wide patching using a Quick Setup patch policyAutomate organization\-wide patching](quick-setup-patch-manager.md)\.
   + Create a maintenance window that uses the Systems Manager document \(SSM document\) `AWS-RunPatchBaseline` in a Run Command task type\. For more information, see [Walkthrough: Creating a maintenance window for patching \(console\)](sysman-patch-mw-console.md)\.
   + Manually run `AWS-RunPatchBaseline` in a Run Command operation\. For more information, see [Running commands from the console](running-commands-console.md)\.
   + Manually patch nodes on demand using the **Patch now** feature\. For more information, see [Patching managed nodes on demand](patch-manager-patch-now-on-demand.md)\.

1. Monitor patching to verify compliance and investigate failures\.

**Topics**
+ [Creating a patch policy](patch-manager-create-a-patch-policy.md)
+ [Viewing patch Dashboard summaries \(console\)](patch-manager-view-dashboard-summaries.md)
+ [Working with patch compliance reports](patch-manager-compliance-reports.md)
+ [Patching managed nodes on demand](patch-manager-patch-now-on-demand.md)
+ [Working with patch baselines](patch-manager-create-a-patch-baseline.md)
+ [Viewing available patches \(console\)](patch-manager-view-available-patches.md)
+ [Working with patch groups](patch-manager-tag-a-patch-group.md)
+ [Working with Patch Manager settings](patch-manager-settings.md)