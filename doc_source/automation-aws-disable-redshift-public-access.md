# AWSConfigRemediation\-DisablePublicAccessToRedshiftCluster<a name="automation-aws-disable-redshift-public-access"></a>

**Description**

The AWSConfigRemediation\-DisablePublicAccessToRedshiftCluster runbook disables public accessibility for the Amazon Redshift cluster that you specify\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DisablePublicAccessToRedshiftCluster)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Databases

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ ClusterIdentifier

  Type: String

  Description: \(Required\) The unique identifier of the cluster that you want to disable public accessibility for\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `redshift:DescribeClusters`
+ `redshift:ModifyCluster`

**Document Steps**
+ aws:executeAwsApi \- Disables public accessibility for the cluster specified in the `ClusterIdentifer` parameter\.
+ aws:waitForAwsResourceProperty \- Waits for the state of the cluster to change to `available`\.
+ aws:assertAwsResourceProperty \- Confirms the public accessibility setting is disabled on the cluster\.