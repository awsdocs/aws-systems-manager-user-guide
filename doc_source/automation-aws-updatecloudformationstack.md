# AWS\-UpdateCloudFormationStack<a name="automation-aws-updatecloudformationstack"></a>

**Description**

Update an AWS CloudFormation stack by using an AWS CloudFormation template stored in an Amazon S3 bucket\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-UpdateCloudFormationStack)

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

  Description: \(Required\) S3 bucket location that contains the updated CloudFormation template \(e\.g\. https://s3\.amazonaws\.com/doc\-example\-bucket/updated\.template\)