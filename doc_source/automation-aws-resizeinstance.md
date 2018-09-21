# AWS\-ResizeInstance<a name="automation-aws-resizeinstance"></a>

**Description**

Change the instance type of an Amazon EC2 instance\.

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

  Description: \(Required\) The ID of the instance\.
+ InstanceType

  Type: String

  Description: \(Required\) The instance type\.
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role assumed by Lambda\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-ResizeInstance --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

aws:createStack

aws:changeInstanceState

aws:invokeLambdaFunction

aws:changeInstanceState

aws:deleteStack

**Outputs**

None