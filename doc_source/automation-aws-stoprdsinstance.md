# AWS\-StopRDSInstance<a name="automation-aws-stoprdsinstance"></a>

**Description**

Stop an Amazon Relational Database Service \(Amazon RDS\) instance\.

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
+ InstanceId

  Type: String

  Description: \(Required\) ID of the Amazon RDS instance to stop\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-StopRdsInstance --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

aws:assertAwsResourceProperty

aws:executeAwsApi

aws:waitForAwsResourceProperty

**Outputs**

None