# Sample scenario for using the InstallOverrideList parameter in `AWS-RunPatchBaseline` or `AWS-RunPatchBaselineAssociation`<a name="patch-manager-override-lists"></a>

You can use the `InstallOverrideList` parameter when you want to override the patches specified by the current default patch baseline in Patch Manager, a capability of AWS Systems Manager\. This topic provides examples that show how to use this parameter to achieve the following:
+ Apply different sets of patches to a target group of managed nodes\.
+ Apply these patch sets on different frequencies\.
+ Use the same patch baseline for both operations\.

Say that you want to install two different categories of patches on your Amazon Linux 2 managed nodes\. You want to install these patches on different schedules using maintenance windows\. You want one maintenance window to run every week and install all `Security` patches\. You want another maintenance window to run once a month and install all available patches, or categories of patches other than `Security`\. 

However, only one patch baseline at a time can be defined as the default for an operating system\. This requirement helps avoid situations where one patch baseline approves a patch while another blocks it, which can lead to issues between conflicting versions\.

With the following strategy, you use the `InstallOverrideList` parameter to apply different types of patches to a target group, on different schedules, while still using the same patch baseline:

1. In the default patch baseline, ensure that only `Security` updates are specified\.

1. Create a maintenance window that runs `AWS-RunPatchBaseline` or `AWS-RunPatchBaselineAssociation` each week\. Don't specify an override list\.

1. Create an override list of the patches of all types that you want to apply on a monthly basis and store it in an Amazon Simple Storage Service \(Amazon S3\) bucket\. 

1. Create a second maintenance window that runs once a month\. However, for the Run Command task you register for this maintenance window, specify the location of your override list\.

The result: Only `Security` patches, as defined in your default patch baseline, are installed each week\. All available patches, or whatever subset of patches you define, are installed each month\.

For more information and sample lists, see [Parameter name: `InstallOverrideList`](patch-manager-aws-runpatchbaseline.md#patch-manager-aws-runpatchbaseline-parameters-installoverridelist)\.