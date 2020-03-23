# AWSSupport\-TroubleshootConnectivityToRDS<a name="automation-awssupport-troubleshootconntords"></a>

 **Description** 

The AWSSupport\-TroubleshootConnectivityToRDS Automation document diagnoses connectivity issues between an Amazon EC2 instance and an Amazon Relational Database Service instance\. The automation ensures the DB instance is available, and then checks the associated security group rules, network access control lists \(network ACLs\), and route tables for potential connectivity issues\.

 **Document Type** 

Automation

 **Owner** 

Amazon

 **Parameters** 
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The IAM role for this execution\. If no role is specified, AWS Systems Manager Automation will use the permissions of the user that runs this document\.
+ DBInstanceIdentifier

  Type: String

  Description: \(Required\) The DB instance ID to test connectivity to\.
+ SourceInstance

  Type: String

  Allowed pattern: ^i\-\[a\-z0\-9\]\{8,17\}$

  Description: \(Required\) The Amazon EC2 instance ID to test connectivity from\.

 **Examples** 

Start the automation

```
aws ssm start-automation-execution --document-name "AWSSupport-TroubleshootConnectivityToRDS" --parameters "DBInstanceIdentifier=DBINSTANCEIDENTIFIER,InstanceId=INSTANCEID"
```

**Required IAM Permissions**

The `AutomationAssumeRole` requires the following actions to successfully execute the Automation document\.
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
+ aws:executeAwsApi \- Gets information about the Amazon EC2 instance\.
+ aws:executeAwsApi \- Gets information about the Amazon EC2 instance network ACLs\.
+ aws:executeAwsApi \- Gets information about the security groups associated with the Amazon EC2 instance\.
+ aws:executeAwsApi \- Gets information about the security groups associated with the DB instance\.
+ aws:executeAwsApi \- Gets information about the route tables associated with the Amazon EC2 instance\.
+ aws:executeAwsApi \- Gets information about the main route table associated with the Amazon VPC for the Amazon EC2 instance\.
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