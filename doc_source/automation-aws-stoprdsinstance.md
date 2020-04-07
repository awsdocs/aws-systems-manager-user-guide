# AWS\-StopRDSInstance<a name="automation-aws-stoprdsinstance"></a>

**Description**

Stop an Amazon Relational Database Service \(Amazon RDS\) instance\. This document calls the [StopDBInstance](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_StopDBInstance.html) API action\. The StopDBInstance API doesn't apply to Aurora MySQL and Aurora PostgreSQL\. For Aurora clusters, you must use the [StopDBCluster](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_StopDBCluster.html) API action instead\.

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.
+ InstanceId

  Type: String

  Description: \(Required\) ID of the Amazon RDS instance to stop\.

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-StopRdsInstance --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```