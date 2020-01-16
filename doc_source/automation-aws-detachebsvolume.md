# AWS\-DetachEBSVolume<a name="automation-aws-detachebsvolume"></a>

**Description**

Detach an Amazon EBS volume from an Amazon EC2 instance\.

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

  Description: \(Optional\) The ARN of the role assumed by Lambda
+ VolumeId

  Type: String

  Description: \(Required\) The ID of the EBS volume\. The volume and instance must be within the same Availability Zone

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-DetachEBSVolume --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```