# AWS\-ASGEnterStandby<a name="automation-aws-asgenterstandby"></a>

**Description**

Change the standby state of an Amazon EC2 instance in an Auto Scaling group\.

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
+ InstanceId

  Type: String

  Description: \(Required\) ID of an Amazon EC2 instance for which you want to change the standby state within an Auto Scaling group\.
+ LambdaRoleArn

  Type: String

  Description: \(Optional\) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf\. If not specified a transient role will be created to run the Lambda function\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-ASGEnterStandby --parameters "parameters"
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

None