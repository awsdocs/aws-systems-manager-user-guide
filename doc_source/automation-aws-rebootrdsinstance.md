# AWS\-RebootRDSInstance<a name="automation-aws-rebootrdsinstance"></a>

**Description**

The AWS\-RebootRdsInstance Automation document reboots an Amazon Relational Database Service \(Amazon RDS\) DB instance if it is not already rebooting\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-RebootRDSInstance)

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

  Description: \(Required\) The ID of the Amazon RDS DB instance that you want to reboot\.

**Document Steps**

RebootInstance \- Reboots the DB instance if it is not already rebooting\.

WaitForAvailableState \- Waits for the DB instance to complete the reboot process\.

**Outputs**

The automation execution has no outputs\.