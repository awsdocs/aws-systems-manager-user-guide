# AWSConfigRemediation\-EnableCloudFrontOriginFailover<a name="automation-aws-enable-cloudfront-failover"></a>

**Description**

The AWSConfigRemediation\-EnableCloudFrontOriginFailover runbook enables origin failover for the Amazon CloudFront \(CloudFront\) distribution you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableCloudFrontOriginFailover)

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
+ OriginGroupId

  Type: String

  Description: \(Required\) The ID of the origin group\.
+ PrimaryOriginId

  Type: String

  Description: \(Required\) The ID of the primary origin in the origin group\.
+ SecondaryOriginId

  Type: String

  Description: \(Required\) The ID of the secondary origin in the origin group\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `cloudfront:GetDistributionConfig`
+ `cloudfront:UpdateDistribution`

**Document Steps**
+ aws:executeScript \- Enables origin failover for the CloudFront distribution you specify in the `CloudFrontDistributionId` parameter, and verifies that failover has been enabled\.