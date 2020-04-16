# AWS\-SetupManagedInstance<a name="automation-aws-setupmanagedinstance"></a>

**Description**

Configure an instance with an AWS Identity and Access Management \(IAM\) role for Systems Manager access\.

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

  Description: \(Required\) ID of the EC2 instance to configure
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf\. If not specified a transient role will be created to run the Lambda function\.
+ RoleName

  Type: String

  Default: SSMRoleForManagedInstance

  Description: \(Optional\) The name of the IAM role for the EC2 instance\. If this role does not exist, it will be created\. When specifying this value, verify that the role contains the **AmazonSSMManagedInstanceCore** Managed Policy\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-SetupManagedInstance --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```