# AWS\-SetupManagedRoleOnEC2Instance<a name="automation-aws-setupmanagedroleonec2instance"></a>

**Description**

Configure an instance with the SSMRoleForManagedInstance managed IAM role for Systems Manager access\.

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

  Description: \(Required\) ID of the Amazon EC2 instance to configure
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf\. If not specified a transient role will be created to run the Lambda function\.
+ RoleName

  Type: String

  Default: SSMRoleForManagedInstance

  Description: \(Optional\) The name of the IAM role for the Amazon EC2 instance\. If this role does not exist, it will be created\. When specifying this value, verify that the role contains the **AmazonEC2RoleForSsm** Managed Policy\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-SetupManagedRoleOnEc2Instance --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

aws:createStack

aws:invokeLambdaFunction

aws:invokeLambdaFunction

aws:invokeLambdaFunction

aws:deleteStack

**Outputs**

None