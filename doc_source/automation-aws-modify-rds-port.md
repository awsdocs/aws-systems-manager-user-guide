# AWSConfigRemediation\-ModifyRDSInstancePortNumber<a name="automation-aws-modify-rds-port"></a>

**Description**

The AWSConfigRemediation\-ModifyRDSInstancePortNumber runbook modifies the port number on which the Amazon Relational Database Service \(Amazon RDS\) instance accepts connections\. Running this automation will restart the database\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-ModifyRDSInstancePortNumber)

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
+ PortNumber

  Type: String

  Description: \(Optional\) The port number you want the DB instance to accept connections on\.
+ RDSDBInstanceResourceId

  Type: String

  Description: \(Required\) The resource identifier for the DB instance whose inbound port number you want to modify\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `rds:DescribeDBInstances`
+ `rds:ModifyDBInstance`

**Document Steps**
+ aws:executeAwsApi \- Gathers the DB instance identifier from the DB instance resource identifier\.
+ aws:assertAwsResourceProperty \- Confirms the DB Instance is in an `AVAILABLE` state\.
+ aws:executeAwsApi \- Modifies the inbound port number on which your DB instance accepts connections\.
+ aws:waitForAwsResourceProperty \- Waits for the DB Instance to be in a `MODIFYING` state\.
+ aws:waitForAwsResourceProperty \- Waits for the DB Instance to be in in an `AVAILABLE` state\.