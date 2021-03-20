# AWSConfigRemediation\-DetachAndDeleteInternetGateway<a name="automation-aws-detach-delete-igw"></a>

**Description**

The AWSConfigRemediation\-DetachAndDeleteInternetGateway runbook detaches and deletes the internet gateway you specify\. If any Amazon EC2 instances in your virtual private cloud \(VPC\) have elastic IP addresses or public IPv4 addresses associated with them, the runbook fails\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DetachAndDeleteInternetGateway)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ InternetGatewayId

  Type: String

  Description: \(Required\) The ID of the internet gateway that you want to delete\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `ec2:DeleteInternetGateway`
+ `ec2:DescribeInternetGateways`
+ `ec2:DetachInternetGateway`

**Document Steps**
+ aws:executeAwsApi \- Gathers the VPC ID from the internet gateway ID\.
+ aws:executeAwsApi \- Detaches the internet gateway ID from the VPC\.
+ aws:executeAwsApi \- Deletes the internet gateway\.