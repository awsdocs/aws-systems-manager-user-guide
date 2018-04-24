# Verify Default Patch Baselines or Create a Custom Patch Baseline<a name="sysman-patch-baselines"></a>

A patch baseline defines which patches are approved for installation on your instances\. You can specify approved or rejected patches one by one\. You can also create auto\-approval rules to specify that certain types of updates \(for example, critical updates\) should be automatically approved\. The rejected list overrides both the rules and the approve list\. 

To use a list of approved patches to install specific packages, you first remove all auto\-approval rules\. If you explicitly identify a patch as rejected, it will not be approved or installed, even if it matches all of the criteria in an auto\-approval rule\. Also, a patch is installed on an instance only if it applies to the software on the instance, even if the patch has otherwise been approved for the instance\.

**Topics**
+ [Pre\-Defined vs\. Custom Baselines](patch-manager-baselines.md)
+ [Important Differences Between Windows and Linux Patching](sysman-patch-differences.md)
+ [Package Name Formats for Approved and Rejected Patch Lists](patch-manager-approved-rejected-package-name-formats.md)