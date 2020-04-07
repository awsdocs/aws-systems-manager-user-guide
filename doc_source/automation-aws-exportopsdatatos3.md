# AWS\-ExportOpsDataToS3<a name="automation-aws-exportopsdatatos3"></a>

**Description**

This document retrieves a list of OpsData summaries in AWS Systems Manager Explorer and exports them to an object in a specified Amazon S3 bucket\.

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ assumeRole

  Type: String

  Description: \(Required\) The role ARN to assume during automation execution\.
+ filters

  Type: String

  Description: \(Optional\) Filters for the getOpsSummary request\.
+ syncName

  Type: String

  Description: \(Optional\) The name of the resource data sync\.
+ resultAttribute

  Type: String

  Description: \(Optional\) The result attribute for getOpsSummary request\.
+ columnFields

  Type: StringList

  Description: \(Required\) Column fields to write to the output file\.
+ s3BucketName

  Type: String

  Description: \(Required\) Amazon S3 bucket where you want to download the output file\.
+ snsTopicArn

  Type: String

  Description: \(Required\) Amazon Simple Notification Service \(Amazon SNS\) topic ARN to notify when the download completes\.
+ snsSuccessMessage

  Type: String

  Description: \(Optional\) Message to send when document finishes\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-ExportOpsDataToS3 --parameters parameters
```

**Document Steps**

getOpsSummaryStep – Retrieves up to 5,000 ops summaries to export in a CSV file now\.

**Outputs**

OpsData object – If the document is executed successfully, you will find the exported OpsData object in your target S3 bucket\.