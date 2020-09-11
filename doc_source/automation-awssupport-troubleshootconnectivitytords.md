# AWSSupport\-TroubleshootConnectivityToRDS<a name="automation-awssupport-troubleshootconnectivitytords"></a>

 **Description** 

The AWSSupport\-TroubleshootConnectivityToRDS Automation document diagnoses connectivity issues between an EC2 instance and an Amazon Relational Database Service instance\. The automation ensures the DB instance is available, and then checks the associated security group rules, network access control lists \(network ACLs\), and route tables for potential connectivity issues\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-TroubleshootConnectivityToRDS)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ DBInstanceIdentifier

  Type: String

  Description: \(Required\) The DB instance ID to test connectivity to\.
+ SourceInstance

  Type: String

  Allowed pattern: ^i\-\[a\-z0\-9\]\{8,17\}$

  Description: \(Required\) The ID of the EC2 instance to test connectivity from\.

**Required IAM Permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ec2:DescribeInstances`
+ `ec2:DescribeNetworkAcls`
+ `ec2:DescribeRouteTables`
+ `ec2:DescribeSecurityGroups`
+ `ec2:DescribeSubnets`
+ `rds:DescribeDBInstances`

**Document Steps**
+ aws:assertAwsResourceProperty \- Confirms the DB instance status is `available`\.
+ aws:executeAwsApi \- Gets information about the DB instance\.
+ aws:executeAwsApi \- Gets information about the DB instance network ACLs\.
+ aws:executeAwsApi \- Gets the DB instance subnet CIDR\.
+ aws:executeAwsApi \- Gets information about the EC2 instance\.
+ aws:executeAwsApi \- Gets information about the EC2 instance network ACLs\.
+ aws:executeAwsApi \- Gets information about the security groups associated with the EC2 instance\.
+ aws:executeAwsApi \- Gets information about the security groups associated with the DB instance\.
+ aws:executeAwsApi \- Gets information about the route tables associated with the EC2 instance\.
+ aws:executeAwsApi \- Gets information about the main route table associated with the Amazon VPC for the EC2 instance\.
+ aws:executeAwsApi \- Gets information about the route tables associated with the DB instance\.
+ aws:executeAwsApi \- Gets information about the main route table associated with the Amazon VPC for the DB instance\.
+ aws:executeScript \- Evaluates security group rules\.
+ aws:executeScript \- Evaluates network ACLs\.
+ aws:executeScript \- Evaluates route tables\.
+ aws:sleep \- Ends the automation execution\.

**Outputs**

getRDSInstanceProperties\.DBInstanceIdentifier \- The DB instance used in the automation execution\.

getRDSInstanceProperties\.DBInstanceStatus \- The current status of the DBInstance\.

evalSecurityGroupRules\.SecurityGroupEvaluation \- Results from comparing the `SourceInstance` security group rules to the DB instance security group rules\.

evalNetworkAclRules\.NetworkAclEvaluation \- Results from comparing the `SourceInstance` network ACLs to the DB instance network ACLs\.

evalRouteTableEntries\.RouteTableEvaluation \- Results from comparing the `SourceInstance` route table to the DB instance routes\.