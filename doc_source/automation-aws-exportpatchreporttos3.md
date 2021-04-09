# AWS\-ExportPatchReportToS3<a name="automation-aws-exportpatchreporttos3"></a>

**Description**

This runbook retrieves lists of patch summary data and patch details in AWS Systems Manager Patch Manager and exports them to \.csv files in a specified Amazon Simple Storage Service \(Amazon S3\) bucket\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-ExportPatchReportToS3)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ assumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ s3BucketName

  Type: String

  Description: \(Required\) The S3 bucket where you want to download the output file\.
+ snsTopicArn

  Type: String

  Description: \(Optional\) The Amazon Simple Notification Service \(Amazon SNS\) topic Amazon Resource Name \(ARN\) to notify when the download completes\.
+ snsSuccessMessage

  Type: String

  Description: \(Optional\) Text of the message to send when the runbook finishes\.
+ targets

  Type: String

  Description: \(Required\) The instance ID or a wildcard character \(\*\) to indicate whether to report patch data for a specific instance or for all instances\.

**Document Steps**

ExportReportStep – The action for this step depends on the value of the `targets` parameter\. If `targets` is in the format of `instanceids=*`, the step retrieves up to 10,000 patch summaries for instances in your account and exports the data to a \.csv file\.

If `targets` is in the format `instanceids=<instance-id>`, the step retrieves both the patch summary and all the patches for the specified instance in your account and exports them to a \.csv file\.

**Outputs**

PatchSummary/Patches object – If the runbook runs successfully, the exported patch report object is downloaded to your target S3 bucket\.