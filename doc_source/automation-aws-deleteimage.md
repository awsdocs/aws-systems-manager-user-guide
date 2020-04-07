# AWS\-DeleteImage<a name="automation-aws-deleteimage"></a>

**Description**

Delete an Amazon Machine Image \(AMI\) and all associated snapshots\.

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
+ ImageId

  Type: String

  Description: \(Required\) The ID of the AMI\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-DeleteImage --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```