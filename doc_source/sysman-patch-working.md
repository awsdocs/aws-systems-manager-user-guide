# Working with Patch Manager \(console\)<a name="sysman-patch-working"></a>

To use Patch Manager, a capability of AWS Systems Manager, complete the following tasks\. These tasks are described in more detail in this section\.

1. Verify that the AWS predefined patch baseline for each operating system type that you use meets your needs\. If it doesn't, create a patch baseline that defines a standard set of patches for that instance type and set it as the default instead\.

1. Organize instances into patch groups by using Amazon EC2 tags \(optional, but recommended\)\.

1. Schedule patching by using a maintenance window that defines which instances to patch and when to patch them\.

   \-or\-

   Patch or scan instances on demand whenever you need to\.

1. Monitor patching to verify compliance and investigate failures\.

**Related content**
+ To view an example of how to create a patch baseline, patch groups, and a maintenance window using the AWS Command Line Interface \(AWS CLI\), see [Walkthrough: Patch a server environment \(AWS CLI\)](sysman-patch-cliwalk.md)\.
+ For more information about maintenance windows, see [AWS Systems Manager Maintenance Windows](systems-manager-maintenance.md)\.
+ For information about monitoring patch compliance, see [About patch compliance](sysman-compliance-about.md#sysman-compliance-monitor-patch)\.

**Topics**
+ [Viewing patch Dashboard summaries \(console\)](view-patch-dashboard-summaries.md)
+ [Working with patch compliance reports](patch-compliance-reports.md)
+ [Patching instances on demand \(console\)](patch-on-demand.md)
+ [Working with patch baselines](patch-baselines.md)
+ [Viewing available patches \(console\)](viewing-available-patches.md)
+ [Creating a patching configuration \(console\)](create-patching-configuration.md)
+ [Working with patch groups](sysman-patch-group-tagging.md)
+ [Working with Patch Manager settings](patch-manager-settings.md)