# AWS\-UpdateCloudFormationStack<a name="automation-aws-updatecloudformationstack"></a>

**Description**

Update an AWS CloudFormation stack by using an AWS CloudFormation template stored in an Amazon S3 bucket\.

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
+ LambdaAssumeRole

  Type: String

  Description: \(Required\) The ARN of the role assumed by Lambda
+ StackNameOrId

  Type: String

  Description: \(Required\) Name or Unique ID of the AWS CloudFormation stack to be updated
+ TemplateUrl

  Type: String

  Description: \(Required\) Amazon S3 bucket location that contains the updated CloudFormation template \(e\.g\. https://s3\.amazonaws\.com/example/updated\.template\)

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-UpdateCloudFormationStack --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```