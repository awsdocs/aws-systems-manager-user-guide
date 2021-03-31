# AWSConfigRemediation\-DeleteElasticsearchDomain<a name="automation-aws-delete-elastic-domain"></a>

**Description**

The AWSConfigRemediation\-DeleteElasticsearchDomain runbook deletes the given Amazon Elasticsearch Service \(Amazon ES\) domain\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableRDSInstanceBackup)

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
+ DomainName

  Type: String

  Description: \(Required\) The name of the Amazon ES service domain to be deleted\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `es:DeleteElasticsearchDomain`
+ `es:DescribeElasticsearchDomainConfig`

**Document Steps**
+ aws:executeScript \- Accepts the Amazon ES service domain name as input, deletes it, and verifies the deletion\.