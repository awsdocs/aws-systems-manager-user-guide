# AWSConfigRemediation\-EnableRedshiftClusterEncryption<a name="automation-aws-enable-redshift-encrypt"></a>

**Description**

The AWSConfigRemediation\-EnableRedshiftClusterEncryption runbook enables encryption on the Amazon Redshift cluster you specify using an AWS Key Management Service \(AWS KMS\) customer managed key\. This runbook should only be used as a baseline to ensure that your Amazon Redshift clusters are encrypted according to minimum recommended security best practices\. We recommend encrypting multiple clusters with different customer managed keys\. This runbook cannot change the AWS KMS customer managed key used on an already encrypted cluster\. To change the AWS KMS customer managed key used to encrypt a cluster, you must first disable encryption on the cluster\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-EnableRedshiftClusterEncryption)

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
+ ClusterIdentifer

  Type: String

  Description: \(Required\) The unique identifier of the cluster you want to enable encryption on\.
+ KmsKeyArn

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS KMS customer managed key you want to use to encrypt the cluster's data\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `redshift:DescribeClusters`
+ `redshift:ModifyCluster`

**Document Steps**
+ aws:executeAwsApi \- Enables encryption on the Amazon Redshift cluster specified in the `ClusterIdentifer` parameter\.
+ aws:assertAwsResourceProperty \- Verifies encryption has been enabled on the cluster\.