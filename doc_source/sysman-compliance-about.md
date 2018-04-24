# About Configuration Compliance<a name="sysman-compliance-about"></a>

This section includes information about the different types of information, *compliance types*, that you can view by using Configuration Compliance\. Configuration Compliance currently supports Patch Manager patching data, State Manager associations, and custom compliance types\.

**Topics**
+ [About Instance Compliance](#sysman-compliance-instance-about)
+ [About Patch Compliance](#sysman-compliance-monitor-patch)
+ [About Association Compliance](#sysman-compliance-about-association)
+ [About Custom Compliance](#sysman-compliance-custom)

## About Instance Compliance<a name="sysman-compliance-instance-about"></a>

Systems Manager now integrates with [Chef InSpec](https://www.chef.io/inspec/)\. InSpec is an open\-source, runtime framework that enables you to create human\-readable profiles on GitHub or Amazon S3\. Then you can use Systems Manager to run compliance scans and view compliant and noncompliant instances\. For more information, see [Using Chef InSpec Profiles with Systems Manager Compliance](integration-chef-inspec.md)\.

## About Patch Compliance<a name="sysman-compliance-monitor-patch"></a>

After you use Patch Manager to install patches on your instances, compliance status information is immediately available to you in the console or in the responses to a set of AWS CLI commands or corresponding Systems Manager API actions\.

**Note**  
If you want to assign a specific patch compliance status to an instance, you can use the [put\-compliance\-items](http://docs.aws.amazon.com/cli/latest/reference/ssm/put-compliance-items.html) CLI command or the [PutComplianceItems](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutComplianceItems.html) API action\. Assigning compliance status is not supported in the Amazon EC2 console\.

For each patch, one of the following compliance status values is reported: 
+ **Installed**: Either the patch was already installed, or Patch Manager installed it when the **AWS\-RunPatchBaseline** document was run on the instance\.
+ **Installed\_Other**: The patch is not in the baseline, but it is installed on the instance\. An individual might have installed it manually\.
+ **Missing**: The patch is approved in the baseline, but it's not installed on the instance\. If you configure the **AWS\-RunPatchBaseline** document task to scan \(instead of install\) the system reports this status for patches that were located during the scan, but have not been installed\.
+ **Not\_Applicable**: The patch is approved in the baseline, but the service or feature that uses the patch is not installed on the instance\. For example, a patch for a web server service would show Not\_Applicable if it was approved in the baseline, but the web service is not installed on the instance\.
+ **Failed**: The patch is approved in the baseline, but it could not be installed\. To troubleshoot this situation, review the command output for information that might help you understand the problem\. 

### View Patch Compliance Reports<a name="compliance-view-results"></a>

You can use the console or the CLI to view patch compliance report data\.

**Note**  
For information about fixing compliance issues, see [Remediating Compliance Issues](sysman-compliance-fixing.md)\.

#### View Patch Compliance Reports \(Console\)<a name="compliance-view-results-console"></a>

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To view patch compliance reports \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Compliance**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Compliance** in the navigation pane\.

1. In the **Corresponding managed instances** area, choose the name of an instance to view its detailed configuration compliance report\.

**To view patch compliance reports \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Configuration Compliance**\.

1. Use the tools on the **Configuration Compliance** page to view default compliance data about patches and State Manager associations, or to create compliance types\.

1. In the Amazon EC2 console navigation pane, choose **Patch Compliance**\.

1. Use the tools on the **Patch Compliance** page to view compliance reports per instance or per patch groups\.

#### View Patch Compliance Reports \(CLI\)<a name="compliance-view-results-cli"></a>

You can view patch compliance details by running the following commands in the CLI\.

[list\-compliance\-summaries](http://docs.aws.amazon.com/cli/latest/reference/ssm/list-compliance-summaries.html)  
Returns a summary count of compliant and non\-compliant patch statuses according to the filter you specify\. \(API: [ListComplianceSummaries](http://docs.aws.amazon.com/ssm/latest/APIReference/API_ListComplianceSummaries.html)\)

[list\-resource\-compliance\-summaries](http://docs.aws.amazon.com/cli/latest/reference/ssm/list-resource-compliance-summaries.html)  
Returns a resource\-level summary count\. The summary includes information about compliant and non\-compliant statuses and detailed compliance\-item severity counts, according to the filter criteria you specify\. \(API: [ListResourceComplianceSummaries](http://docs.aws.amazon.com/ssm/latest/APIReference/API_ListResourceComplianceSummaries.html)\)

[describe\-patch\-group\-state](http://docs.aws.amazon.com/cli/latest/reference/ssm/describe-patch-group-state.html)  
Returns high\-level aggregated patch compliance state for a patch group\. \(API: [DescribePatchGroupState](http://docs.aws.amazon.com/ssm/latest/APIReference/API_DescribePatchGroupState.html)\)

[describe\-instance\-patch\-states\-for\-patch\-group](http://docs.aws.amazon.com/cli/latest/reference/ssm/describe-instance-patch-states-for-patch-group.html)  
Returns the high\-level patch state for the instances in the specified patch group\. \(API: [DescribeInstancePatchStatesForPatchGroup](http://docs.aws.amazon.com/ssm/latest/APIReference/API_DescribeInstancePatchStatesForPatchGroup.html)\)

For an illustration of using the AWS CLI to configure patching and how to view patch compliance details, see [Systems Manager Patch Manager Walkthroughs](sysman-patch-walkthrough.md)\.

## About Association Compliance<a name="sysman-compliance-about-association"></a>

After you create one or more State Manager associations, Configuration Compliance automatically reports association compliance status\. You don't need to perform any additional steps to view these statuses\. If you want to assign a specific association compliance status to an instance, you can use the [PutComplianceItems](http://docs.aws.amazon.com/ssm/latest/APIReference/API_PutComplianceItems.html) API action to explicitly assign a status\. You can use this API action from the AWS CLI, AWS Tools for Windows PowerShell, or the SDK\. You currently can't assign compliance status by using the Amazon EC2 console\.

You can view association compliance details in the Amazon EC2 console on the **Compliance Configuration** page, or you can use the following API actions to view compliance details:

**Note**  
Currently, Configuration Compliance shows compliance statuses of `Compliant` or `Non-compliant` and severity of `Unspecified`\.
+ [ListComplianceSummaries](http://docs.aws.amazon.com/ssm/latest/APIReference/API_ListComplianceSummaries.html): Returns a summary count of compliant and non\-compliant association statuses according to the filter you specify\.
+ [ListResourceComplianceSummaries](http://docs.aws.amazon.com/ssm/latest/APIReference/API_ListResourceComplianceSummaries.html): Returns a resource\-level summary count\. The summary includes information about compliant and non\-compliant statuses and `Unspecified` counts, according to the filter criteria you specify\. 

## About Custom Compliance<a name="sysman-compliance-custom"></a>

You can assign compliance metadata to a managed instance\. This metadata can then be aggregated with other compliance data for compliance reporting purposes\. For example, say that your business runs versions 2\.0, 3\.0, and 4\.0 of software X on your managed instances\. The company wants to standardize on version 4\.0, meaning that instances running versions 2\.0 and 3\.0 are non\-compliant\. You can use the [PutComplianceItems](http://docs.aws.amazon.com/ssm/latest/APIReference/API_PutComplianceItems.html) API action to explicitly note which managed instances are running older versions of software X\. Currently you can only assign compliance metadata by using the AWS CLI, AWS Tools for Windows PowerShell, or the SDKs\. The following CLI sample command assigns compliance metadata to a managed instance and specifies the compliance type in the required format `Custom:`\.

```
aws ssm put-compliance-items --resource-id i-1234567890 --resource-type ManagedInstance --compliance-type Custom:SoftwareXCheck --execution-summary ExecutionTime=AnyStringToDenoteTimeOrDate, --items Id=Version2.0,Title=SoftwareXVersion,Severity=CRITICAL,Status=NON_COMPLIANT 
```

Compliance managers can then view summaries or create reports about which instances are or aren't compliant\. You can assign a maximum of 10 different custom compliance types to an instance\.

For an example of how to create a custom compliance type and view compliance data, see [Systems Manager Configuration Compliance Manager Walkthrough \(AWS CLI\)](sysman-compliance-walk.md)\.