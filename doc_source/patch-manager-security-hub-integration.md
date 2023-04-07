# Integrating Patch Manager with AWS Security Hub<a name="patch-manager-security-hub-integration"></a>

[AWS Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html) provides you with a comprehensive view of your security state in AWS\. Security Hub collects security data from across AWS accounts, AWS services, and supported third\-party partner products\. With Security Hub, you can check your environment against security industry standards and best practices\. Security Hub helps you to analyze your security trends and identify the highest priority security issues\.

By using the integration between Patch Manager, a capability of AWS Systems Manager, and Security Hub, you can send findings about noncompliant nodes from Patch Manager to Security Hub\. A finding is the observable record of a security check or security\-related detection\. Security Hub can then include those patch\-related findings in its analysis of your security posture\.

**Note**  
The information in the following topics applies no matter which method or type of configuration you are using for your patching operations:  
A patch policy configured in Quick Setup
A Host Management option configured in Quick Setup
A maintenance window to run a patch `Scan` or `Install` task
An on\-demand **Patch now** operation

**Contents**
+ [How Patch Manager sends findings to Security Hub](#securityhub-integration-sending-findings)
  + [Types of findings that Patch Manager sends](#securityhub-integration-finding-types)
  + [Latency for sending findings](#securityhub-integration-finding-latency)
  + [Retrying when Security Hub isn't available](#securityhub-integration-retry-send)
  + [Updating existing findings in Security Hub](#securityhub-integration-finding-updates)
+ [Typical finding from Patch Manager](#securityhub-integration-finding-example)
+ [Turning on and configuring the integration](#securityhub-integration-enable)
+ [How to stop sending findings](#securityhub-integration-disable)

## How Patch Manager sends findings to Security Hub<a name="securityhub-integration-sending-findings"></a>

In Security Hub, security issues are tracked as findings\. Some findings come from issues that are detected by other AWS services or by third\-party partners\. Security Hub also has a set of rules that it uses to detect security issues and generate findings\.

 Patch Manager is one of the Systems Manager capabilities that sends findings to Security Hub\. After you perform a patching operation by running a SSM document \(`AWS-RunPatchBaseline`, `AWS-RunPatchBaselineAssociation`, or `AWS-RunPatchBaselineWithHooks`\), the patching information is sent to Inventory or Compliance, capabilities of AWS Systems Manager, or both\. After Inventory, Compliance, or both receive the data, Patch Manager receives a notification\. Then, Patch Manager evaluates the data for accuracy, formatting, and compliance\. If all conditions are met, Patch Manager forwards the data to Security Hub\.

Security Hub provides tools to manage findings from across all of these sources\. You can view and filter lists of findings and view details for a finding\. For more information, see [Viewing findings](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-viewing.html) in the *AWS Security Hub User Guide*\. You can also track the status of an investigation into a finding\. For more information, see [Taking action on findings](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-taking-action.html) in the *AWS Security Hub User Guide*\.

All findings in Security Hub use a standard JSON format called the AWS Security Finding Format \(ASFF\)\. The ASFF includes details about the source of the issue, the affected resources, and the current status of the finding\. For more information, see [AWS Security Finding Format \(ASFF\)](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format.htm) in the *AWS Security Hub User Guide*\.

### Types of findings that Patch Manager sends<a name="securityhub-integration-finding-types"></a>

Patch Manager sends the findings to Security Hub using the [AWS Security Finding Format \(ASFF\)](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format.html)\. In ASFF, the `Types` field provides the finding type\. Findings from Patch Manager have the following value for `Types`:
+ Software and Configuration Checks/Patch Management

 Patch Manager sends one finding per noncompliant managed node\. The finding is reported with the resource type [https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format-attributes.html#asff-resourcedetails-awsec2instance](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format-attributes.html#asff-resourcedetails-awsec2instance) so that findings can be correlated with other Security Hub integrations that report `AwsEc2Instance` resource types\. Patch Manager only forwards a finding to Security Hub if the operation discovered the managed node to be noncompliant\. The finding includes the Patch Summary results\. For more information about compliance definitions, see [Understanding patch compliance state values](patch-manager-compliance-states.md)\. For more information about `PatchSummary`, see [PatchSummary](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_PatchSummary.html) in the *AWS Security Hub API Reference*\.

### Latency for sending findings<a name="securityhub-integration-finding-latency"></a>

When Patch Manager creates a new finding, it's usually sent to Security Hub within a few seconds to 2 hours\. The speed depends on the traffic in the AWS Region being processed at that time\.

### Retrying when Security Hub isn't available<a name="securityhub-integration-retry-send"></a>

If there is a service outage, an AWS Lambda function is run to put the messages back into the main queue after the service is running again\. After the messages are in the main queue, the retry is automatic\.

If Security Hub isn't available, Patch Manager retries sending the findings until they're received\.

### Updating existing findings in Security Hub<a name="securityhub-integration-finding-updates"></a>

Patch Manager doesn't update a finding after it sends the finding to Security Hub\. If a patching operation runs on an `AwsEc2Instance` resource type for which patch noncompliance was already reported, and the resource is again found to be noncompliant, new findings are sent to Security Hub\. No findings are sent for resources that are compliant, even if they were previously noncompliant\.

## Typical finding from Patch Manager<a name="securityhub-integration-finding-example"></a>

Patch Manager sends findings to Security Hub using the [AWS Security Finding Format \(ASFF\)](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings-format.html)\.

Here is an example of a typical finding from Patch Manager\.

```
{
  "SchemaVersion": "2018-10-08",
  "Id": "arn:aws:patchmanager:us-east-2:111122223333:instance/i-02573cafcfEXAMPLE/document/AWS-RunPatchBaseline/run-command/d710f5bd-04e3-47b4-82f6-df4e0EXAMPLE",
  "ProductArn": "arn:aws:securityhub:us-east-1::product/aws/ssm-patch-manager",
  "GeneratorId": "d710f5bd-04e3-47b4-82f6-df4e0EXAMPLE",
  "AwsAccountId": "111122223333",
  "Types": [
    "Software & Configuration Checks/Patch Management/Compliance"
  ],
  "CreatedAt": "2021-11-11T22:05:25Z",
  "UpdatedAt": "2021-11-11T22:05:25Z",
  "Severity": {
    "Label": "INFORMATIONAL",
    "Normalized": 0
  },
  "Title": "Systems Manager Patch Summary - Managed Instance Non-Compliant",
  "Description": "This AWS control checks whether each instance that is managed by AWS Systems Manager is in compliance with the rules of the patch baseline that applies to that instance when a compliance Scan runs.",
  "Remediation": {
    "Recommendation": {
      "Text": "For information about bringing instances into patch compliance, see 'Remediating out-of-compliance instances (Patch Manager)'.",
      "Url": "https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-compliance-remediation.html"
    }
  },
  "SourceUrl": "https://us-east-2.console.aws.amazon.com/systems-manager/managed-instances/i-02573cafcfEXAMPLE/patch?region=us-east-2",
  "ProductFields": {
    "aws/securityhub/FindingId": "arn:aws:securityhub:us-east-2::product/aws/ssm-patch-manager/arn:aws:patchmanager:us-east-2:111122223333:instance/i-02573cafcfEXAMPLE/document/AWS-RunPatchBaseline/run-command/d710f5bd-04e3-47b4-82f6-df4e0EXAMPLE",
    "aws/securityhub/ProductName": "Systems Manager Patch Manager",
    "aws/securityhub/CompanyName": "AWS"
  },
  "Resources": [
    {
      "Type": "AwsEc2Instance",
      "Id": "i-02573cafcfEXAMPLE",
      "Partition": "aws",
      "Region": "us-east-2"
    }
  ],
  "WorkflowState": "NEW",
  "Workflow": {
    "Status": "NEW"
  },
  "RecordState": "ACTIVE",
  "PatchSummary": {
    "Id": "pb-0c10e65780EXAMPLE",
    "InstalledCount": 45,
    "MissingCount": 2,
    "FailedCount": 0,
    "InstalledOtherCount": 396,
    "InstalledRejectedCount": 0,
    "InstalledPendingReboot": 0,
    "OperationStartTime": "2021-11-11T22:05:06Z",
    "OperationEndTime": "2021-11-11T22:05:25Z",
    "RebootOption": "NoReboot",
    "Operation": "SCAN"
  }
}
```

## Turning on and configuring the integration<a name="securityhub-integration-enable"></a>

To use the Patch Manager integration with Security Hub, you must turn on Security Hub\. For information about how to turn on Security Hub, see [Setting up Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-settingup.html) in the *AWS Security Hub User Guide*\.

The following procedure describes how to integrate Patch Manager and Security Hub when Security Hub is already active but Patch Manager integration is turned off\. You only need to complete this procedure if integration was manually turned off\.

**To add Patch Manager to Security Hub integration**

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. Choose the **Settings** tab\.

   \-or\-

   If you are accessing Patch Manager for the first time in the current AWS Region, choose **View predefined patch baselines**, and then choose the **Settings** tab\.

1. Under the **Export to Security Hub** section, to the right of **Patch compliance findings aren't being exported to Security Hub**, choose **Enable**\.

## How to stop sending findings<a name="securityhub-integration-disable"></a>

To stop sending findings to Security Hub, you can use either the Security Hub console or the API\.

For more information, see the following topics in the *AWS Security Hub User Guide*:
+ [Disabling and enabling the flow of findings from an integration \(console\)](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-integrations-managing.html#securityhub-integration-findings-flow-console)
+ [Disabling the flow of findings from an integration \(Security Hub API, AWS CLI\)](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-integrations-managing.html#securityhub-integration-findings-flow-disable-api)