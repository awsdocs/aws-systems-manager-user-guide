# AWSConfigRemediation\-EnforceHTTPSOnESDomain<a name="automation-aws-enforce-https-es"></a>

**Description**

The AWSConfigRemediation\-EnforceHTTPSOnESDomain runbook enables the `EnforceHTTPS` endpoint option on an Amazon Elasticsearch Service \(Amazon ES\) domain you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnforceHTTPSOnESDomain)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ DomainName

  Type: String

  Description: \(Required\) The name of the Amazon ES service domain\.
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `es:DescribeElasticsearchDomain`
+ `es:UpdateElasticsearchDomainConfig`

**Document Steps**
+ aws:executeScript \- Enables the `EnforceHTTPS` endpoint option on the Amazon ES service domain you specify in the `DomainName` parameter\.