# Tagging Systems Manager resources<a name="tagging-resources"></a>

A tag is a label that you assign to an AWS resource\. Each tag consists of a *key* and a *value*, both of which you define\. 

Tags enable you to categorize your AWS resources in different ways, such as by purpose, owner, or environment\. For example, if you want to organize and manage your resources according to whether they're used for development or production, you might tag some of them with the key `Environment` and the value `Production`\. You can then perform various types of queries for resources tagged `"Key=Environment,Values=Production"`\. For example, you could define a set of tags for your account's managed instances that help you track or target instances by operating system and environment, such as your SUSE Linux Enterprise Server grouped as `development`, `staging`, and `production`\. You can also perform operations on resources by specifying this key\-value pair in your commands, such as running an update script on all instances in the group or reviewing the status of those instances\.

You can use the tags applied to your Systems Manager in various operations\. For example, you can target only managed instances that are tagged with a specified tag key\-value pair when you [run a command](run-command.md) or [assign targets to a maintenance window](sysman-maintenance-assign-targets.md)\. You can also [restrict access to your resources](security_iam_id-based-policy-examples.md) based on the tags applied to them\.

Going further, you can create *resource groups* by specifying the same tags for AWS resources of various types, not only the same type\. After that, you can use the Systems Manager Resource Groups capability to view information about which resources in a group are compliant and working correctly and which resources require action\. The information you view pertains to all types of AWS resources that can be added to a resource group, not only supported Systems Manager resource types\. For more information, see [Resource Groups in AWS Systems Manager](systems-manager-resource-groups.md) and [Viewing operations data for AWS Resource Groups](viewing-operations-data.md)\.

The remainder of this chapter describes how to add and remove tags from Systems Manager resources\.

**Topics**
+ [Taggable Systems Manager resources](taggable-resources.md)
+ [Tagging Systems Manager documents](tagging-documents.md)
+ [Tagging maintenance windows](tagging-maintenance-windows.md)
+ [Tagging managed instances](tagging-managed-instances.md)
+ [Tagging OpsItems](tagging-opsitems.md)
+ [Tagging Systems Manager parameters](tagging-parameters.md)
+ [Tagging patch baselines](tagging-patch-baselines.md)