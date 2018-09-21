# AWS\-RebootRDSInstance<a name="automation-aws-rebootrdsinstance"></a>

**Description**

Reboot an Amazon Relational Database Service \(Amazon RDS\) instance\.

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

  Description: \(Required\) The ID of the Amazon RDS instance that you want to reboot\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-RebootRdsInstance --parameters parameters
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