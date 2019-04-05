# AWS\-CreateImage<a name="automation-aws-createimage"></a>

**Description**

Create a new Amazon Machine Image \(AMI\) from an Amazon EC2 instance\.

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.
+ InstanceId

  Type: String

  Description: \(Required\) The ID of the Amazon EC2 instance\.
+ NoReboot

  Type: Boolean

  Description: \(Optional\) Do not reboot the instance before creating the image\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-CreateImage --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

aws:createImage

**Outputs**

createImage\.ImageId