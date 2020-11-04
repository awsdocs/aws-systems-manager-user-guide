# AWSConfigRemediation\-DeleteUnusedSecurityGroup<a name="automation-aws-delete-ec2-security-group"></a>

**Description**

The AWSConfigRemediation\-DeleteUnusedSecurityGroup Automation document deletes the security group you specify in the `GroupId` parameter\. If you attempt to delete a security group that is associated with an Amazon Elastic Compute Cloud \(Amazon EC2\) instance, or is referenced by another security group, the automation fails\. This automation does not delete a default security group\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DeleteUnusedSecurityGroup)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ GroupId

  Type: String

  Description: \(Required\) The ID of the security group that you want to delete\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:ExecuteAutomation`
+ `ssm:GetAutomationExecution`
+ `ec2:DescribeSecurityGroups`
+ `ec2:DeleteSecurityGroup`

**Document Steps**
+ aws:executeAwsApi \- Returns the security group name using the value you provide in the `GroupId` parameter\.
+ aws:branch \- Confirms that the group name is not "default"\.
+ aws:executeAwsApi \- Deletes the security group specified in the `GroupId` parameter\.
+ aws:executeScript \- Confirms the security group was deleted\.