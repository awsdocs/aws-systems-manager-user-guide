# AWS\-PatchASGInstance<a name="automation-aws-patchasginstance"></a>

**Description**

Patch Amazon EC2 instances in an Auto Scaling group\.

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

  Description: \(Required\) ID of the instance to patch\. Don't specify an instance ID that is configured to run during a Maintenance Window\.
+ LambdaRoleArn

  Type: String

  Description: \(Optional\) The ARN of the role that allows Lambda created by Automation to perform the actions on your behalf\. If not specified a transient role will be created to run the Lambda function\.
+ WaitForInstance

  Type: String

  Default: PT2M

  Description: \(Optional\) Duration the Automation should sleep to allow the instance to come back into service\.
+ WaitForReboot

  Type: String

  Default: PT5M

  Description: \(Optional\) Duration the Automation should sleep to allow a patched instance to reboot\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-PatchAsgInstance --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```