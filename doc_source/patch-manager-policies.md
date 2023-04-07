# Using Quick Setup patch policies<a name="patch-manager-policies"></a>

Beginning December 22, 2022, Patch Manager offers a new, recommended method to configure patching for your organization and AWS accounts through the use of *patch policies*\. 

A patch policy is a configuration you set up using Quick Setup, a capability of AWS Systems Manager\. Patch policies provide more extensive and more centralized control over your patching operations than is available with previous methods of configuring patching\. Patch policies can be used with [all operating systems supported by Patch Manager](patch-manager-prerequisites.md#pm-prereqs), including supported versions of Linux, macOS, and Windows Server\. For information about creating a patch policy, see [Automate organization\-wide patching using a Quick Setup patch policyAutomate organization\-wide patching](quick-setup-patch-manager.md)\.

## Major features of patch policies<a name="patch-policies-about-major-features"></a>

Instead of using other methods of patching your nodes, use a patch policy to take advantage of these major features:
+ **Single setup** – Setting up patching operations using a maintenance window or State Manager association can require multiple tasks in different parts of the Systems Manager console\. Using a patch policy, all your patching operations can be set up in a single wizard\.
+ **Multi\-account/Multi\-Region support** – Using a maintenance window, a State Manager association, or the **Patch now** feature in Patch Manager, you're limited to targeting managed nodes in a single AWS account\-AWS Region pair\. If you use multiple accounts and multiple Regions, your setup and maintenance tasks can require a great deal of time, because you must perform setup tasks in each account\-Region pair\. However, if you use AWS Organizations, you can set up one patch policy that applies to all your managed nodes in all AWS Regions in all your AWS accounts\. Or, if you choose, a patch policy can apply to only some organizational units \(OUs\) in the accounts and Regions you choose\. A patch policy can also apply to a single local account, if you choose\.
+ **Installation support at the organizational level** – The existing Host Management configuration option in Quick Setup provides support for a daily scan of your managed nodes for patch compliance\. However, this scan is done at a predetermined time and results in patch compliance information only\. No patch installations are performed\. Using a patch policy, you scan specify different schedules for scanning and installing\. You can also choose the frequency and time of these operations by using custom CRON or Rate expressions\. For example, you could scan for missing patches every day to provide you with regularly updated compliance information\. But, your installation schedule could be just once a week to avoid unwanted downtime\.
+ **Simplified patch baseline selection** – Patch policies still incorporate patch baselines, and there are no changes to the way patch baselines are configured\. However, when you create or update a patch policy, you can select the AWS managed or custom baseline you want to use for each operating system \(OS\) type in a single list\. It’s not necessary to specify the default baseline for each OS type in separate tasks\.

**Note**  
When patching operations based on a patch policy run, they use the `AWS-RunPatchBaseline` SSM document\. For more information, see [About the `AWS-RunPatchBaseline` SSM document](patch-manager-aws-runpatchbaseline.md)\.

**Related information**  
[Centrally deploy patching operations across your AWS Organization using Systems Manager Quick Setup](http://aws.amazon.com/blogs/mt/centrally-deploy-patching-operations-across-your-aws-organization-using-systems-manager-quick-setup/) \(AWS Cloud Operations and Migrations Blog\)

## Other differences with patch policies<a name="patch-policies-about-other-features"></a>

Here are some other differences to note when using patch policies instead of previous methods of configuring patching:
+ **No patch groups required** – In previous patching operations, you could tag multiple nodes to belong to a patch group, and then specify the patch baseline to use for that patch group\. If no patch group was defined, Patch Manager patched instances with the current default patch baseline for the OS type\. Using patch policies, it’s no longer necessary to set up and maintain patch groups\. 
+ **‘Configure patching’ page removed** – Before the release of patch policies, you could specify defaults for which nodes to patch, a patching schedule, and a patching operation on a **Configure patching** page\. This page has been removed from Patch Manager\. These options are now specified in patch policies\. 
+ **No ‘Patch now’ support** – The ability to patch nodes on demand is still limited to a single AWS account\-AWS Region pair at a time\. For information, see [Patching managed nodes on demand](patch-manager-patch-now-on-demand.md)\.
+ **Patch policies and compliance information** – When your managed nodes are scanned for compliance according to a patching policy configuration, compliance data is made available to you\. You can view and work with the data in the same way as with other methods of compliance scanning\. Although you can set up a patch policy for an entire organization or multiple organizational units, compliance information is reported individually for each AWS account\-AWS Region pair\. For more information, see [Working with patch compliance reports](patch-manager-compliance-reports.md)\.

## AWS Regions supported for patch policies<a name="patch-policies-supported-regions"></a>

Patch policy configurations in Quick Setup are currently supported in the following Regions:
+ US East \(Ohio\) \(us\-east\-2\)
+ US East \(N\. Virginia\) \(us\-east\-1\)
+ US West \(N\. California\) \(us\-west\-1\)
+ US West \(Oregon\) \(us\-west\-2\)
+ Asia Pacific \(Mumbai\) \(ap\-south\-1\)
+ Asia Pacific \(Seoul\) \(ap\-northeast\-2\)
+ Asia Pacific \(Singapore\) \(ap\-southeast\-1\)
+ Asia Pacific \(Sydney\) \(ap\-southeast\-2\)
+ Asia Pacific \(Tokyo\) \(ap\-northeast\-1\)
+ Canada \(Central\) \(ca\-central\-1\)
+ Europe \(Frankfurt\) \(eu\-central\-1\)
+ Europe \(Ireland\) \(eu\-west\-1\)
+ Europe \(London\) \(eu\-west\-2\)
+ Europe \(Paris\) \(eu\-west\-3\)
+ Europe \(Stockholm\) \(eu\-north\-1\)
+ South America \(São Paulo\) \(sa\-east\-1\)