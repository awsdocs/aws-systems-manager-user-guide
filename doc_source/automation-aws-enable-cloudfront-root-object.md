# AWSConfigRemediation\-EnableCloudFrontDefaultRootObject<a name="automation-aws-enable-cloudfront-root-object"></a>

**Description**

The AWSConfigRemediation\-EnableCloudFrontDefaultRootObject runbook configures the default root object for the Amazon CloudFront \(CloudFront\) distribution that you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableCloudFrontDefaultRootObject)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ CloudFrontDistributionId

  Type: String

  Description: \(Required\) The ID of the CloudFront distribution that you want to configure the default root object for\.
+ DefaultRootObject

  Type: String

  Description: \(Required\) The object that you want CloudFront to return when a viewer request points to your root URL\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `cloudfront:GetDistributionConfig`
+ `cloudfront:UpdateDistribution`

**Document Steps**
+ aws:executeScript \- Configures the default root object for the CloudFront distribution that you specify in the `CloudFrontDistributionId` parameter\.