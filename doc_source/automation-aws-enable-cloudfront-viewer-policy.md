# AWSConfigRemediation\-EnableCloudFrontViewerPolicyHTTPS<a name="automation-aws-enable-cloudfront-viewer-policy"></a>

**Description**

The AWSConfigRemediation\-EnableCloudFrontViewerPolicyHTTPS runbook enables the viewer protocol policy for the Amazon CloudFront \(CloudFront\) distribution you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableCloudFrontViewerPolicyHTTPS)

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

  Description: \(Required\) The ID of the CloudFront distribution you want to enable the viewer protocol policy on\.
+ ViewerProtocolPolicy

  Type: String

  Valid values: https\-only, redirect\-to\-https

  Description: \(Required\) The protocol that viewers can use to access the files in the origin\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `cloudfront:GetDistributionConfig`
+ `cloudfront:UpdateDistribution`

**Document Steps**
+ aws:executeScript \- Enables the viewer protocol policy for the CloudFront distribution you specify in the `CloudFrontDistributionId` parameter, and verifies the policy was assigned\.