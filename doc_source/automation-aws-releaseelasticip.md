# AWS\-ReleaseElasticIP<a name="automation-aws-releaseelasticip"></a>

**Description**

Release the specified Elastic IP address using the allocation ID\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-ReleaseElasticIP)

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
+ AllocationId

  Type: String

  Description: \(Required\) The Allocation ID of the Elastic IP address\.