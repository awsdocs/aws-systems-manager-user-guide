# AWS\-CreateSnapshot<a name="automation-aws-createsnapshot"></a>

**Description**

Create a snapshot of an Amazon EBS volume\.

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
+ Description

  Type: String

  Description: \(Optional\) A description for the snapshot
+ VolumeId

  Type: String

  Description: \(Required\) The ID of the volume\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-CreateSnapshot --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```