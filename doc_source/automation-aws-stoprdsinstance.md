# AWS\-StopRDSInstance<a name="automation-aws-stoprdsinstance"></a>

**Description**

Stop an Amazon Relational Database Service \(Amazon RDS\) instance\. This document calls the [StopDBInstance](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_StopDBInstance.html) API action\. The StopDBInstance API doesn't apply to Aurora MySQL and Aurora PostgreSQL\. For Aurora clusters, you must use the [StopDBCluster](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_StopDBCluster.html) API action instead\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-StopRDSInstance)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Databases

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ InstanceId

  Type: String

  Description: \(Required\) ID of the Amazon RDS instance to stop\.