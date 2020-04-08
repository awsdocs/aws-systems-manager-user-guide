# AWS\-ConfigureCloudWatchOnEC2Instance<a name="automation-aws-configurecloudwatchonec2instance"></a>

**Description**

Enable or disable Amazon CloudWatch monitoring on managed instances\.

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

  Description: \(Required\) The ID of the managed that you want to configure to use CloudWatch\.
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role assumed by Lambda\.
+ properties

  Type: String

  Description: \(Optional\) The configuration for CloudWatch in JSON format\.
+ status

  Allowed values: Enabled,Disabled

  Default: Enabled

  Description: \(Optional\) Specifies whether to enable or disable CloudWatch\. Valid values: "Enabled" \| "Disabled"\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-ConfigureCloudWatchOnEC2Instance --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```