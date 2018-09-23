# Working with Patch Manager \(Console\)<a name="sysman-patch-working"></a>

To use Patch Manager, complete the following tasks\. These tasks are described in more detail in this section\.

1. Verify that the default patch baselines meet your needs, or create patch baselines that define a standard set of patches for your instances\.

1. Organize instances into patch groups by using Amazon EC2 tags \(optional, but recommended\)\.

1. Schedule patching by using a Maintenance Window that defines which instances to patch and when to patch them\.

1. Monitor patching to verify compliance and investigate failures\.

**Related Content**
+ To view an example of how to create a patch baseline, patch groups, and a Maintenance Window using the AWS CLI, see [Tutorial: Patch a Server Environment \(AWS CLI\)](sysman-patch-cliwalk.md)\.
+ For more information about Maintenance Windows, see [AWS Systems Manager Maintenance Windows](systems-manager-maintenance.md)\.
+ For information about monitoring patch compliance, see [About Patch Compliance](sysman-compliance-about.md#sysman-compliance-monitor-patch)\.

**Topics**
+ [Create a Default Patch Baseline](sysman-patch-baseline-console.md)
+ [Create a Patch Group](sysman-patch-tagging-console.md)
+ [Create a Maintenance Window for Patching](sysman-patch-mw-console.md)
+ [Create a Patching Configuration](create-patching-configuration.md)