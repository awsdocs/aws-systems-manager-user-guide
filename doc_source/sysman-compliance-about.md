# Working with Configuration Compliance<a name="sysman-compliance-about"></a>

Configuration Compliance collects and reports data about the status of Patch Manager patching, State Manager associations, and custom compliance types\. This section includes details about each of these compliance types and how to view Systems Manager compliance data\. This section also includes information about how to view compliance history and change tracking\.

**Note**  
Systems Manager integrates with [Chef InSpec](https://www.chef.io/inspec/)\. InSpec is an open\-source, runtime framework that enables you to create human\-readable profiles on GitHub or Amazon S3\. Then you can use Systems Manager to run compliance scans and view compliant and noncompliant instances\. For more information, see [Using Chef InSpec profiles with Systems Manager Compliance](integration-chef-inspec.md)\.

**Topics**
+ [About patch compliance](#sysman-compliance-monitor-patch)
+ [About State Manager association compliance](#sysman-compliance-about-association)
+ [About custom compliance](#sysman-compliance-custom)
+ [Viewing current compliance data](#compliance-view-results)
+ [Viewing compliance configuration history and change tracking](#sysman-compliance-history)

## About patch compliance<a name="sysman-compliance-monitor-patch"></a>

After you use Systems Manager Patch Manager to install patches on your instances, compliance status information is immediately available to you in the console or in response to AWS CLI commands or corresponding Systems Manager API actions\.

For information about patch compliance status values, see [About patch compliance status values](about-patch-compliance-states.md)\.

## About State Manager association compliance<a name="sysman-compliance-about-association"></a>

After you create one or more State Manager associations, compliance status information is immediately available to you in the console or in response to AWS CLI commands or corresponding Systems Manager API actions\. For associations, Configuration Compliance shows statuses of `Compliant` or `Non-compliant` and the severity level assigned to the association, such as `Critical` or `Medium`\.

## About custom compliance<a name="sysman-compliance-custom"></a>

You can assign compliance metadata to a managed instance\. This metadata can then be aggregated with other compliance data for compliance reporting purposes\. For example, say that your business runs versions 2\.0, 3\.0, and 4\.0 of software X on your managed instances\. The company wants to standardize on version 4\.0, meaning that instances running versions 2\.0 and 3\.0 are non\-compliant\. You can use the [PutComplianceItems](https://docs.aws.amazon.com/ssm/latest/APIReference/API_PutComplianceItems.html) API action to explicitly note which managed instances are running older versions of software X\. Currently you can only assign compliance metadata by using the AWS CLI, AWS Tools for Windows PowerShell, or the SDKs\. The following CLI sample command assigns compliance metadata to a managed instance and specifies the compliance type in the required format `Custom:`\.

------
#### [ Linux ]

```
aws ssm put-compliance-items \
    --resource-id i-1234567890abcdef0 \
    --resource-type ManagedInstance \
    --compliance-type Custom:SoftwareXCheck \
    --execution-summary ExecutionTime=AnyStringToDenoteTimeOrDate \
    --items Id=Version2.0,Title=SoftwareXVersion,Severity=CRITICAL,Status=NON_COMPLIANT
```

------
#### [ Windows ]

```
aws ssm put-compliance-items ^
    --resource-id i-1234567890abcdef0 ^
    --resource-type ManagedInstance ^
    --compliance-type Custom:SoftwareXCheck ^
    --execution-summary ExecutionTime=AnyStringToDenoteTimeOrDate ^
    --items Id=Version2.0,Title=SoftwareXVersion,Severity=CRITICAL,Status=NON_COMPLIANT
```

------

Compliance managers can then view summaries or create reports about which instances are or aren't compliant\. You can assign a maximum of 10 different custom compliance types to an instance\.

For an example of how to create a custom compliance type and view compliance data, see [Configuration Compliance walkthrough \(AWS CLI\)](sysman-compliance-walk.md)\.

## Viewing current compliance data<a name="compliance-view-results"></a>

This section describes how to view compliance data in the AWS Systems Manager console and by using the AWS CLI\. For information about how to view patch and association compliance history and change tracking, see [Viewing compliance configuration history and change tracking](#sysman-compliance-history)\.

**Topics**
+ [Viewing current compliance data \(console\)](#compliance-view-results-console)
+ [Viewing current compliance data \(AWS CLI\)](#compliance-view-data-cli)

### Viewing current compliance data \(console\)<a name="compliance-view-results-console"></a>

Use the following procedure to view compliance data in the Systems Manager console\.

**To view current compliance reports in the Systems Manager console**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Compliance**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Compliance** in the navigation pane\.

1. In the **Details overview for resources** area, choose the ID of an instance\.

1. On the **Instance ID** details page, select the **Configuration compliance** tab to view its detailed configuration compliance report\.

**Note**  
For information about fixing compliance issues, see [Remediating compliance issues using EventBridge](sysman-compliance-fixing.md)\.

### Viewing current compliance data \(AWS CLI\)<a name="compliance-view-data-cli"></a>

You can view summaries of compliance data for patching, associations, and custom compliance types in the in the AWS CLI by using the following AWS CLI commands\. 

[list\-compliance\-summaries](https://docs.aws.amazon.com/cli/latest/reference/ssm/list-compliance-summaries.html)  
Returns a summary count of compliant and non\-compliant association statuses according to the filter you specify\. \(API: [ListComplianceSummaries](https://docs.aws.amazon.com/ssm/latest/APIReference/API_ListComplianceSummaries.html)\)

[list\-resource\-compliance\-summaries](https://docs.aws.amazon.com/cli/latest/reference/ssm/list-resource-compliance-summaries.html)  
Returns a resource\-level summary count\. The summary includes information about compliant and non\-compliant statuses and detailed compliance\-item severity counts, according to the filter criteria you specify\. \(API: [ListResourceComplianceSummaries](https://docs.aws.amazon.com/ssm/latest/APIReference/API_ListResourceComplianceSummaries.html)\)

You can view additional compliance data for patching by using the following AWS CLI commands\.

[describe\-patch\-group\-state](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-patch-group-state.html)  
Returns high\-level aggregated patch compliance state for a patch group\. \(API: [DescribePatchGroupState](https://docs.aws.amazon.com/ssm/latest/APIReference/API_DescribePatchGroupState.html)\)

[describe\-instance\-patch\-states\-for\-patch\-group](https://docs.aws.amazon.com/cli/latest/reference/ssm/describe-instance-patch-states-for-patch-group.html)  
Returns the high\-level patch state for the instances in the specified patch group\. \(API: [DescribeInstancePatchStatesForPatchGroup](https://docs.aws.amazon.com/ssm/latest/APIReference/API_DescribeInstancePatchStatesForPatchGroup.html)\)

**Note**  
For an illustration of how to configure patching and view patch compliance details by using the AWS CLI, see [Walkthrough: Patch a server environment \(AWS CLI\)](sysman-patch-cliwalk.md)\.

## Viewing compliance configuration history and change tracking<a name="sysman-compliance-history"></a>

Systems Manager Configuration Compliance displays *current* patching and association compliance data for your managed instances\. You can view patching and association compliance history and change tracking by using [AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/)\. AWS Config provides a detailed view of the configuration of AWS resources in your AWS account\. This includes how the resources are related to one another and how they were configured in the past so that you can see how the configurations and relationships change over time\. To view patching and association compliance history and change tracking, you must enable the following resources in AWS Config: 
+ SSM:PatchCompliance
+ SSM:AssociationCompliance

For information about how to choose and configure these specific resources in AWS Config, see [Selecting Which Resources AWS Config Records](https://docs.aws.amazon.com/config/latest/developerguide/select-resources.html) in the *AWS Config Developer Guide*\.

**Note**  
For information about AWS Config pricing, see [Pricing](https://aws.amazon.com/config/pricing/)\.