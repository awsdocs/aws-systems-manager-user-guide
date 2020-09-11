# AWS\-ExportOpsDataToS3<a name="automation-aws-exportopsdatatos3"></a>

**Description**

This document retrieves a list of OpsData summaries in AWS Systems Manager Explorer and exports them to an object in a specified S3 bucket\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-ExportOpsDataToS3)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ columnFields

  Type: StringList

  Description: \(Required\) Column fields to write to the output file\.
+ filters

  Type: String

  Description: \(Optional\) Filters for the getOpsSummary request\.
+ resultAttribute

  Type: String

  Description: \(Optional\) The result attribute for getOpsSummary request\.
+ s3BucketName

  Type: String

  Description: \(Required\) S3 bucket where you want to download the output file\.
+ snsSuccessMessage

  Type: String

  Description: \(Optional\) Message to send when document finishes\.
+ snsTopicArn

  Type: String

  Description: \(Required\) Amazon Simple Notification Service \(Amazon SNS\) topic ARN to notify when the download completes\.
+ syncName

  Type: String

  Description: \(Optional\) The name of the resource data sync\.

**Document Steps**

getOpsSummaryStep – Retrieves up to 5,000 ops summaries to export in a CSV file now\.

**Outputs**

OpsData object – If the Automation document runs successfully, you will find the exported OpsData object in your target S3 bucket\.