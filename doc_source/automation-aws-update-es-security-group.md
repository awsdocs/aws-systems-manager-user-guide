# AWSConfigRemediation\-UpdateElasticsearchDomainSecurityGroups<a name="automation-aws-update-es-security-group"></a>

**Description**

The AWSConfigRemediation\-UpdateElasticsearchDomainSecurityGroups runbook updates the security group configuration on the Amazon Elasticsearch Service \(Amazon ES\) domain you specify\. Security groups can only be applied to Amazon ES domains configured for virtual private cloud \(VPC\) access\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-UpdateElasticsearchDomainSecurityGroups)

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

  Description: \(Required\) The name of the Amazon ES domain\.
+ SecurityGroupList

  Type: StringList

  Description: \(Required\) The security group IDs you want to assign to the Amazon ES domain\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `es:DescribeElasticsearchDomain`
+ `es:UpdateElasticsearchDomainConfig`

**Document Steps**
+ aws:executeScript \- Updates the security group configuration on the Amazon ES service domain you specify in the `DomainName` parameter\.