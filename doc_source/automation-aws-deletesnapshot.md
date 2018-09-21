# AWS\-DeleteSnapshot<a name="automation-aws-deletesnapshot"></a>

**Description**

Delete a snapshot of an Amazon EBS volume\.

**Document Type**

Automation

**Owner**

Amazon

**Platform\(s\)**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.
+ SnapshotId

  Type: String

  Description: \(Required\) The ID of the EBS snapshot\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-DeleteSnapshot --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

aws:executeAwsApi

**Outputs**

None