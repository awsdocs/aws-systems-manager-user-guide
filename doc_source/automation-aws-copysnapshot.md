# AWS\-CopySnapshot<a name="automation-aws-copysnapshot"></a>

**Description**

Copy a snapshot of an Amazon Elastic Block Store \(Amazon EBS\) volume\.

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

  Description: \(Optional\) A description for the Amazon EBS snapshot\.
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role assumed by Lambda\.
+ SnapshotId

  Type: String

  Description: \(Required\) The ID of the Amazon EBS snapshot to copy\.
+ SourceRegion

  Type: String

  Description: \(Required\) The region where the source snapshot currently exists\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-CopySnapshot --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

aws:createStack

aws:invokeLambdaFunction

aws:deleteStack

**Outputs**

copySnapshot\.Payload