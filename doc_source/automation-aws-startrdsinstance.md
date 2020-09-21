# AWS\-StartRDSInstance<a name="automation-aws-startrdsinstance"></a>

**Description**

Start an Amazon Relational Database Service \(Amazon RDS\) instance\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-StartRDSInstance)

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

  Description: \(Required\) ID of the Amazon RDS instance to start\.