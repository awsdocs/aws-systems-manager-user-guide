# AWS\-DeleteEBSVolumeSnapshots<a name="automation-aws-deleteebsvolumesnapshots"></a>

**Description**

Delete a snapshot of an Amazon Elastic Block Store \(Amazon EBS\) volume\.

**Note**  
The AWS Lambda function that runs during this operation has a maximum execution time \(timeout\) of 60 seconds\. If you have a large number of Amazon EBS volume snapshots to delete, the operation might fail with an error message\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-DeleteEBSVolumeSnapshots)

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
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf\. If not specified a transient role will be created to run the Lambda function\.
+ RetentionCount

  Type: String

  Default: 10

  Description: \(Optional\) Number of snapshots to keep for the volume\. Either RetentionCount or RetentionDays should be mentioned, not both\.
+ RetentionDays

  Type: String

  Description: \(Optional\) Number of days to keep snapshots for the volume\. Either RetentionCount or RetentionDays should be mentioned, not both
+ VolumeId

  Type: String

  Description: \(Required\) The volume identifier to delete snapshots for\.