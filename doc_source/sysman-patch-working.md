# Working with Patch Manager \(console\)<a name="sysman-patch-working"></a>

To use Patch Manager, complete the following tasks\. These tasks are described in more detail in this section\.

1. Verify that the AWS predefined patch baseline for each operating system type that you use meets your needs\. If it does not, create a patch baseline that defines a standard set of patches for that instance type and set it as the default instead\.

1. Organize instances into patch groups by using Amazon EC2 tags \(optional, but recommended\)\.

1. Schedule patching by using a maintenance window that defines which instances to patch and when to patch them\.

   \-or\-

   Patch or scan instances on demand whenever you need to\.

1. Monitor patching to verify compliance and investigate failures\.

**Related Content**
+ To view an example of how to create a patch baseline, patch groups, and a maintenance window using the AWS CLI, see [Walkthrough: Patch a server environment \(AWS CLI\)](sysman-patch-cliwalk.md)\.
+ For more information about maintenance windows, see [AWS Systems Manager Maintenance Windows](systems-manager-maintenance.md)\.
+ For information about monitoring patch compliance, see [About patch compliance](sysman-compliance-about.md#sysman-compliance-monitor-patch)\.

**Topics**
+ [Viewing AWS predefined patch baselines](view-predefined-patch-baselines.md)
+ [Working with custom patch baselines](sysman-patch-baseline-console.md)
+ [Setting an existing patch baseline as the default](set-default-patch-baseline.md)
+ [Creating a patching configuration \(console\)](create-patching-configuration.md)
+ [Creating a patch group](sysman-patch-group-tagging.md)
+ [Patching instances on demand](patch-on-demand.md)
+ [Creating a maintenance window for patching](sysman-patch-mw-console.md)