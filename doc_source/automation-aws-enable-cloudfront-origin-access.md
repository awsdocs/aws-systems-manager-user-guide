# AWSConfigRemediation\-EnableCloudFrontOriginAccessIdentity<a name="automation-aws-enable-cloudfront-origin-access"></a>

**Description**

The AWSConfigRemediation\-EnableCloudFrontOriginAccessIdentity runbook enables origin access identity for the Amazon CloudFront \(CloudFront\) distribution you specify\. This automation assigns the same CloudFront Origin Access Identity for all Origins of the Amazon Simple Storage Service \(Amazon S3\) Origin type without origin access identity for the CloudFront distribution you specify\. This automation does not grant read permission to the origin access identity for CloudFront to access objects in your Amazon S3 bucket\. You must update your Amazon S3 bucket permissions to allow access\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableCloudFrontOriginAccessIdentity)

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

  Description: \(Required\) The ID of the CloudFront distribution you want to enable origin failover on\.
+ OriginAccessIdentityId

  Type: String

  Description: \(Required\) The ID of the CloudFront origin access identity to associate with the origin\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `cloudfront:GetDistributionConfig`
+ `cloudfront:UpdateDistribution`

**Document Steps**
+ aws:executeScript \- Enables origin access identity for the CloudFront distribution you specify in the `CloudFrontDistributionId` parameter, and verifies the origin access identity was assigned\.