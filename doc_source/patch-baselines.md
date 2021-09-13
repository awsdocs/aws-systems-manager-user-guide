# Working with patch baselines<a name="patch-baselines"></a>

A patch baseline in Patch Manager, a capability of AWS Systems Manager, defines which patches are approved for installation on your instances\. You can specify approved or rejected patches one by one\. You can also create auto\-approval rules to specify that certain types of updates \(for example, critical updates\) should be automatically approved\. The rejected list overrides both the rules and the approve list\. To use a list of approved patches to install specific packages, you first remove all auto\-approval rules\. If you explicitly identify a patch as rejected, it won't be approved or installed, even if it matches all of the criteria in an auto\-approval rule\. Also, a patch is installed on an instance only if it applies to the software on the instance, even if the patch has otherwise been approved for the instance\.

**Topics**
+ [Viewing AWS predefined patch baselines \(console\)](view-predefined-patch-baselines.md)
+ [Working with custom patch baselines \(console\)](sysman-patch-baseline-console.md)
+ [Setting an existing patch baseline as the default \(console\)](set-default-patch-baseline.md)

**Related content**

[About patch baselines](about-patch-baselines.md)