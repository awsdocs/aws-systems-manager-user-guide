# AWS\-RestartEC2Instance<a name="automation-aws-restartec2instance"></a>

**Description**

Restart one or more Amazon EC2 instances\.

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

  Type: StringList

  Description: \(Required\) EC2 instance\(s\) to restart

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-RestartEC2Instance --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

aws:changeInstanceState

aws:changeInstanceState

**Outputs**

None