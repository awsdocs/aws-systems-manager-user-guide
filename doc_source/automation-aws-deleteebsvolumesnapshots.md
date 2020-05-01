# AWS\-DeleteEBSVolumeSnapshots<a name="automation-aws-deleteebsvolumesnapshots"></a>

**Description**

Delete a snapshot of an Amazon Elastic Block Store \(Amazon EBS\) volume\.

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

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.
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