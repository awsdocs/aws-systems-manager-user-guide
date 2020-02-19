# Working with Patch Manager \(Console\)<a name="sysman-patch-working"></a>

To use Patch Manager, complete the following tasks\. These tasks are described in more detail in this section\.

1. Verify that the AWS predefined patch baseline for each operating system type that you use meets your needs\. If it does not, create a patch baseline that defines a standard set of patches for that instance type and set it as the default instead\.

1. Organize instances into patch groups by using Amazon EC2 tags \(optional, but recommended\)\.

1. Schedule patching by using a maintenance window that defines which instances to patch and when to patch them\.

1. Monitor patching to verify compliance and investigate failures\.

**Related Content**
+ To view an example of how to create a patch baseline, patch groups, and a maintenance window using the AWS CLI, see [Tutorial: Patch a Server Environment \(Command Line\)](sysman-patch-cliwalk.md)\.
+ For more information about maintenance windows, see [AWS Systems Manager Maintenance Windows](systems-manager-maintenance.md)\.
+ For information about monitoring patch compliance, see [About Patch Compliance](sysman-compliance-about.md#sysman-compliance-monitor-patch)\.

**Topics**
+ [View AWS Predefined Patch Baselines](view-predefined-patch-baselines.md)
+ [Create a Custom Patch Baseline](sysman-patch-baseline-console.md)
+ [Set an Existing Patch Baseline as the Default](set-default-patch-baseline.md)
+ [Create a Patch Group](sysman-patch-group-tagging.md)
+ [Add a Patch Group to a Patch Baseline](sysman-patch-group-patchbaseline.md)
+ [Create a Maintenance Window for Patching](sysman-patch-mw-console.md)
+ [Create a Patching Configuration \(Console\)](create-patching-configuration.md)
+ [Update or Delete a Patch Baseline \(Console\)](patch-baseline-update-or-delete.md)