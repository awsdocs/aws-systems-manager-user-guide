# Viewing patch compliance results<a name="viewing-patch-compliance-results"></a>

Use these procedures to view patch compliance information about your managed instances\.

This procedure applies to patch operations that use the `AWS-RunPatchBaseline` document\. For information about viewing patch compliance information for patch operations that use the `AWS-RunPatchBaselineAssociation` document, see [Identifying out\-of\-compliance instances](patch-compliance-identify.md)\.

**Note**  
The patch scan processes for Quick Setup and Explorer, AWS Systems Manager capabilities, use the `AWS-RunPatchBaselineAssociation` document\.

**Identify the patch solution for a specific CVE issue \(Linux\)**  
For many Linux\-based operating systems, patch compliance results indicate which Common Vulnerabilities and Exposure \(CVE\) bulletin issues are resolved by which patches\. This information can help you determine how urgently you need to install a missing or failed patch\.

CVE details are included for supported versions of the following operating system types:
+ Amazon Linux
+ Amazon Linux 2
+ Oracle Linux
+ Red Hat Enterprise Linux \(RHEL\)
+ SUSE Linux Enterprise Server \(SLES\)
+ CentOS

  By default, CentOS does not provide CVE information about updates\. You can, however, enable this support by using third\-party repositories such as the Extra Packages for Enterprise Linux \(EPEL\) repository published by Fedora\. For information, see [EPEL](https://fedoraproject.org/wiki/EPEL) on the Fedora Wiki\.

**Note**  
You can also add CVE IDs to your lists of approved or rejected patches in your patch baselines, as the situation and your patching goals warrant\.  
For information about working with approved and rejected patch lists, see the following topics:  
[Working with custom patch baselines](sysman-patch-baseline-console.md)
[About package name formats for approved and rejected patch lists](patch-manager-approved-rejected-package-name-formats.md)
[How patch baseline rules work on Linux\-based systems](patch-manager-how-it-works-linux-rules.md)
[How patches are installed](patch-manager-how-it-works-installation.md)

## Viewing patching compliance results \(console\)<a name="viewing-patch-compliance-results-console"></a>

Use the following procedures to view patch compliance results in the AWS Systems Manager console\. 

**Note**  
For information about generating patch compliance reports that are downloaded to an Amazon Simple Storage Service \(Amazon S3\) bucket, see [Generating \.csv patch compliance reports](patch-compliance-reports-to-s3.md)\.

**To view patch compliance results**

1. Do one of the following\.

   **Option 1** \(recommended\) – Navigate from Patch Manager, a capability of AWS Systems Manager:
   + In the navigation pane, choose **Patch Manager**\.

     \-or\-

     If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.
   + Choose the **Reporting** tab\.
   + For **Instance patching summary**, choose the ID of the instance for which you want to review patch compliance results\.
   + Choose **View details**\.

   **Option 2** – Navigate from Compliance, a capability of AWS Systems Manager:
   + In the navigation pane, choose **Compliance**\.

     \-or\-

     If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Compliance** in the navigation pane\.
   + For **Compliance resources summary**, choose a number in the column for the types of patch resources you want to review, such as **Non\-Compliant resources**\.
   + In the **Resource** list, choose the ID of the instance for which you want to review patch compliance results\.

   **Option 3** – Navigate from Fleet Manager, a capability of AWS Systems Manager\.
   + In the navigation pane, choose **Fleet Manager**\.

     \-or\-

     If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.
   + On the **Managed instances** tab, choose the ID of the instance for which you want to review patch compliance results\.

1. Choose the **Patch** tab\.

1. \(Optional\) In the Search box \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/search-icon.png)\), choose from the available filters\.

   For example, for Red Hat Enterprise Linux \(RHEL\), choose from the following:
   + Name
   + Classification
   + State
   + Severity

    For Windows Server, choose from the following:
   + KB
   + Classification
   + State
   + Severity

1. Choose one of the available values for the filter type you chose\. For example, if you chose **State**, now choose a compliance state such as **InstalledPendingReboot**, **Failed** or **Missing**\.

1. Depending on the compliance state of the instance, you can choose what action to take to remedy any noncompliant instances\.

   For example, you can choose to patch your noncompliant instances immediately\. For information about patching your instances on demand, see [Patching instances on demand](patch-on-demand.md)\.

   For information about patch compliance states, see [Understanding patch compliance state values](about-patch-compliance-states.md)\.

## Viewing patching compliance results \(AWS CLI\)<a name="viewing-patch-compliance-results-cli"></a>

**To view patch compliance results for a single instance**

Run the following command in the AWS Command Line Interface \(AWS CLI\) to view patch compliance results for a single instance\.

```
aws ssm describe-instance-patch-states --instance-id instance-id
```

Replace *instance\-id* with the ID of the managed instance for which you want to view results, in the format **i\-02573cafcfEXAMPLE** or **mi\-0282f7c436EXAMPLE**\.

The systems returns information like the following\.

```
{
    "InstancePatchStates": [
        {
            "InstanceId": "i-02573cafcfEXAMPLE",
            "PatchGroup": "mypatchgroup",
            "BaselineId": "pb-0c10e65780EXAMPLE",            
            "SnapshotId": "a3f5ff34-9bc4-4d2c-a665-4d1c1EXAMPLE",
            "CriticalNonCompliantCount": 2,
            "SecurityNonCompliantCount": 2,
            "OtherNonCompliantCount": 1,
            "InstalledCount": 123,
            "InstalledOtherCount": 334,
            "InstalledPendingRebootCount": 0,
            "InstalledRejectedCount": 0,
            "MissingCount": 1,
            "FailedCount": 2,
            "UnreportedNotApplicableCount": 11,
            "NotApplicableCount": 2063,
            "OperationStartTime": "2021-05-03T11:00:56-07:00",
            "OperationEndTime": "2021-05-03T11:01:09-07:00",
            "Operation": "Scan",
            "LastNoRebootInstallOperationTime": "2020-06-14T12:17:41-07:00",
            "RebootOption": "RebootIfNeeded"
        }
    ]
}
```

**To view a patch count summary for all EC2 instances in a Region**

The `describe-instance-patch-states` supports retrieving results for just one managed instance at a time\. However, using a custom script with the `describe-instance-patch-states` command, you can generate a more granular report\.

For example, if the [jq filter tool](https://stedolan.github.io/jq/download/) is installed on you local machine, you could run the following command to identify which of your EC2 instances in a particular AWS Region have a status of `InstalledPendingReboot`\.

```
aws ssm describe-instance-patch-states \
--instance-ids $(aws ec2 describe-instances --region region | jq '.Reservations[].Instances[] | .InstanceId' | tr '\n|"' ' ') \
--output text --query 'InstancePatchStates[*].{Instance:InstanceId, InstalledPendingRebootCount:InstalledPendingRebootCount}'
```

*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

For example:

```
aws ssm describe-instance-patch-states \
--instance-ids $(aws ec2 describe-instances --region us-east-2 | jq '.Reservations[].Instances[] | .InstanceId' | tr '\n|"' ' ') \
--output text --query 'InstancePatchStates[*].{Instance:InstanceId, InstalledPendingRebootCount:InstalledPendingRebootCount}'
```

The system returns information like the following\.

```
1       i-02573cafcfEXAMPLE
0       i-0471e04240EXAMPLE
3       i-07782c72faEXAMPLE
6       i-083b678d37EXAMPLE
0       i-03a530a2d4EXAMPLE
1       i-01f68df0d0EXAMPLE
0       i-0a39c0f214EXAMPLE
7       i-0903a5101eEXAMPLE
7       i-03823c2fedEXAMPLE
```

In addition to `InstalledPendingRebootCount`, the list of count types you can search for include the following:
+ `CriticalNonCompliantCount`
+ `SecurityNonCompliantCount`
+ `OtherNonCompliantCount`
+ `UnreportedNotApplicableCount `
+ `InstalledPendingRebootCount`
+ `FailedCount`
+ `NotApplicableCount`
+ `InstalledRejectedCount`
+ `InstalledOtherCount`
+ `MissingCount`
+ `InstalledCount`