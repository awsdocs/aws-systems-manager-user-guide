# AWS\-ReleaseElasticIP<a name="automation-aws-releaseelasticip"></a>

**Description**

Release the specified Elastic IP address using the allocation ID\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-ReleaseElasticIP)

**Document Type**

Automation

**Owner**

Amazon

**Platform\(s\)**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\.
+ AllocationId

  Type: String

  Description: \(Required\) The Allocation ID of the Elastic IP address\.