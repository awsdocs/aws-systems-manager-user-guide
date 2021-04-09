# Viewing patch compliance results<a name="viewing-patch-compliance-results"></a>

Use these procedures to view patch compliance information about your managed instances\.

This procedure applies to patch operations that use the `AWS-RunPatchBaseline` document\. For information about viewing patch compliance information for patch operations that use the `AWS-RunPatchBaselineAssociation` document, see [Identifying out\-of\-compliance instances](patch-compliance-identify.md)\.

**Note**  
The AWS Systems Manager Quick Setup and AWS Systems Manager Explorer patch scan processes use the `AWS-RunPatchBaselineAssociation` document\.

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

Use the following procedure to view patch compliance results in the AWS Systems Manager console\. 

**To view patch compliance results**

1. Do one of the following\.

   **Option 1** – Navigate from **Compliance**:
   + In the navigation pane, choose **Compliance**\.

     \-or\-

     If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Compliance** in the navigation pane\.
   + For **Compliance resources summary**, choose a number in the column for the types of patch resources you want to review, such as **Non\-Compliant resources**\.
   + In the **Resource** list, choose the ID of the instance for which you want to review patch compliance results\.

   **Option 2** – Navigate from **Managed Instances**\.
   + In the navigation pane, choose **Managed Instances**\.

     \-or\-

     If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Managed Instances**\.
   + On the **Managed Instances** tab, choose the ID of the instance for which you want to review patch compliance results\.

1. Choose the **Patch** tab\.

1. \(Optional\) In the Search box \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/search-icon.png)\), choose from the following filters:
   + Classification
   + KB
   + Severity
   + State

1. Choose one of the available values for the filter type you chose\. For example, if you chose **State**, now choose a compliance state such as **Failed** or **Missing**\.

1. Depending on the compliance state of the instance, you can choose what action to take to remedy any noncompliant instances\.

   For example, you can choose to patch your noncompliant instances immediately\. For information about patching your instances on demand, see [Patching instances on demand](patch-on-demand.md)\.

   For information about patch compliance states, see [Understanding patch compliance state values](about-patch-compliance-states.md)\.

## Viewing patching compliance results \(AWS CLI\)<a name="viewing-patch-compliance-results-cli"></a>

**To view patch compliance results for a single instance**

Run the following command in the AWS CLI to view patch compliance results for a single instance\.

```
aws ssm describe-instance-patch-states --instance-id instance-id
```

Replace *instance\-id* with the ID of the managed instance for which you want to view results, in the format **i\-02573cafcfEXAMPLE** or **mi\-0282f7c436EXAMPLE**\.

The systems returns information like the following\.

```
{
    "InstancePatchStates": [
        {
            "UnreportedNotApplicableCount": 0,
            "OperationStartTime": 1614103280.047,
            "BaselineId": "pb-020d361a05EXAMPLE",
            "InstalledPendingRebootCount": 0,
            "FailedCount": 1,
            "InstanceId": "mi-0282f7c436EXAMPLE",
            "OwnerInformation": "",
            "NotApplicableCount": 1913,
            "RebootOption": "RebootIfNeeded",
            "OperationEndTime": 1614103633.089,
            "PatchGroup": "",
            "InstalledRejectedCount": 0,
            "InstalledOtherCount": 4,
            "MissingCount": 0,
            "SnapshotId": "dc42fa2f-8490-4854-ad54-8fc61EXAMPLE",
            "Operation": "Scan",
            "InstalledCount": 17
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
+ `UnreportedNotApplicableCount `
+ `InstalledPendingRebootCount`
+ `FailedCount`
+ `NotApplicableCount`
+ `InstalledRejectedCount`
+ `InstalledOtherCount`
+ `MissingCount`
+ `InstalledCount`