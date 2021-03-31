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

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `ec2:DeleteInternetGateway`
+ `ec2:DescribeInternetGateways`
+ `ec2:DetachInternetGateway`

**Document Steps**
+ aws:waitForAwsResourceProperty \- Accepts the ID of the virtual private gateway and waits until the virtual private gateway's state property changes to `available` or times out\.
+ aws:executeAwsApi \- Retrieves a specified virtual private gateway configuration\.
+ aws:branch \- Branches based on the VpcAttachments\.state parameter value\.
+ aws:waitForAwsResourceProperty \- Accepts the ID of the virtual private gateway and waits until the virtual private gateway's VpcAttachments\.state's property changes to `attached` or times out\.
+ aws:executeAwsApi \- Accepts the ID of the virtual private gateway and the ID of the Amazon VPC as input, and detaches the virtual private gateway from the Amazon VPC\.
+ aws:waitForAwsResourceProperty \- Accepts the ID of the virtual private gateway and waits until the virtual private gateway's VpcAttachments\.state's property changes to `detached` or times out\.
+ aws:executeAwsApi \- Accepts the ID of the virtual private gateway as input and deletes it\.
+ aws:waitForAwsResourceProperty \- Accepts the ID of the virtual private gateway as input and verifies its deletion\.

  aws:executeAwsApi \- Gathers the VPC ID from the internet gateway ID\.
+ aws:executeAwsApi \- Detaches the internet gateway ID from the VPC\.
+ aws:executeAwsApi \- Deletes the internet gateway\.