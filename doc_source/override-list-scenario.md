# Sample scenario for using the InstallOverrideList parameter in AWS\-RunPatchBaseline<a name="override-list-scenario"></a>

You can use the `InstallOverrideList` parameter when you want to override the patches specified by the current default patch baseline\. The following example shows how to use this parameter to achieve the following:
+ Apply different sets of patches to a target group of instances\.
+ Apply these patch sets on different frequencies\.
+ Use the same patch baseline for both operations\.

Say that you want to install two different categories of patches on your Amazon Linux 2 instances\. You want to install these patches on different schedules using maintenance windows\. You want one maintenance window to run every week and install all `Security` patches\. You want another maintenance window to run once a month and install all available patches, or categories of patches other than `Security`\. 

However, only one patch baseline at a time can be defined as the default for an operating system\. This requirement helps avoid situations where one patch baseline approves a patch while another blocks it, which can lead to issues between conflicting versions\.

The following strategy lets you use the `InstallOverrideList` parameter to apply different types of patches to a target group, on different schedules, while still using the same patch baseline\.

1. In the default patch baseline, ensure that only `Security` updates are specified\.

1. Create a first maintenance window that runs `AWS-RunPatchBaseline` each week\. Do not specify an override list\.

1. Create an override list of the patches of all types that you want to apply on a monthly basis and store it in an Amazon S3 bucket\. A sample script that you can modify to help create the list of patches follows this procedure\. 

1. Create a second maintenance window that runs once a month\. But for the Run Command task you register for this maintenance window, specify the location of your override list\.

The result: Only `Security` patches, as defined in your default patch baseline, are installed each week\. All available patches, or whatever subset of patches you define, are installed each month\.

**Sample script for generating an override list**  
Download the following InstallOverrideList script and use it to generate an override list composed of all `Missing` patches reported from the most recent Scan operation\. You can let the script generate a list of all patches, or you can use its filter fields to create a subset list\. For example, you could specify that only patches of a certain classification or severity be added to the list\. Note that the script uses the `[DescribeInstanceInformation](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeInstanceInformation.html)` command to create the list\.

[Download InstallOverrideList sample script](samples/InstallOverrideList-Script.zip)