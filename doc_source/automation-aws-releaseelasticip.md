# AWS\-ReleaseElasticIP<a name="automation-aws-releaseelasticip"></a>

**Description**

Release the specified Elastic IP address using the allocation ID\.

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
+ AllocationId

  Type: String

  Description: \(Required\) The Allocation ID of the Elastic IP address\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-ReleaseElasticIP --parameters AllocationId=eipalloc-0123456789abcdefg
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```