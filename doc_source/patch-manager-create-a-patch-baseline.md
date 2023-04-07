# Working with patch baselines<a name="patch-manager-create-a-patch-baseline"></a>

A patch baseline in Patch Manager, a capability of AWS Systems Manager, defines which patches are approved for installation on your managed nodes\. You can specify approved or rejected patches one by one\. You can also create auto\-approval rules to specify that certain types of updates \(for example, critical updates\) should be automatically approved\. The rejected list overrides both the rules and the approve list\. To use a list of approved patches to install specific packages, you first remove all auto\-approval rules\. If you explicitly identify a patch as rejected, it won't be approved or installed, even if it matches all of the criteria in an auto\-approval rule\. Also, a patch is installed on a managed node only if it applies to the software on the node, even if the patch has otherwise been approved for the managed node\.

**Topics**
+ [Viewing AWS predefined patch baselines \(console\)](patch-manager-view-predefined-patch-baselines.md)
+ [Working with custom patch baselines \(console\)](patch-manager-manage-patch-baselines.md)
+ [Setting an existing patch baseline as the default \(console\)](patch-manager-default-patch-baseline.md)

**More info**  
+ [About patch baselines](patch-manager-patch-baselines.md)