# AWS\-AttachIAMToInstance<a name="automation-aws-attachiamtoinstance"></a>

**Description**

Attach an AWS Identity and Access Management \(IAM\) role to a managed instance\.

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

  Description: \(Required\) The ID of the instance on which you want to assign an IAM role\.
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role assumed by AWS Lambda\.
+ RoleName

  Type: String

  Description: \(Required\) The IAM role name to add to the managed instance\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-AttachIAMToInstance --parameters parameters
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

attachIAMToInstance\.LogResult attachIAMToInstance\.Payload