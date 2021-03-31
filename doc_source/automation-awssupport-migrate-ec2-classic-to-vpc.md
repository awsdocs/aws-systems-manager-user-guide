# AWSSupport\-MigrateEC2ClassicToVPC<a name="automation-awssupport-migrate-ec2-classic-to-vpc"></a>

**Description**

The AWSSupport\-MigrateEC2ClassicToVPC runbook migrates an Amazon Elastic Compute Cloud \(Amazon EC2\) instance from EC2\-Classic to a virtual private cloud \(VPC\)\. This runbook supports migrating Amazon EC2 instances of the hardware virtual machine \(HVM\) virtualization type with Amazon Elastic Block Store \(Amazon EBS\) root volumes\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-MigrateEC2ClassicToVPC)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ ApproverIAM

  Type: StringList

  Description: \(Optional\) The Amazon Resource Names \(ARNs\) of IAM users who can approve or deny the action\. This parameter only applies if you specify the `CutOver` value for the `MigrationType` parameter\.
+ DestinationSecurityGroupId

  Type: StringList

  Description: \(Optional\) The ID of the security group you want to associate with the Amazon EC2 instance that is launched in your VPC\. If you do not specify a value for this parameter, the automation creates a security group in your VPC and copies the rules from the security group in EC2\-Classic\. If the rules fail to copy to the new security group, the default security group of your VPC is associated with the Amazon EC2 instance\.
+ DestinationSubnetId

  Type: String

  Description: \(Optional\) The ID of the subnet you want to migrate your Amazon EC2 instance to\. If you do not specify a value for this parameter, the automation randomly chooses a subnet from your VPC\.
+ InstanceId

  Type: String

  Description: \(Required\) The ID of the Amazon EC2 instance you want to migrate\.
+ MigrationType

  Type: String

  Valid values: CutOver \| Test

  Description: \(Required\) The type of migration you want to perform\.

  The `CutOver` option requires approval to stop your Amazon EC2 instance that's running in EC2\-Classic\. After this action is approved, the Amazon EC2 instance is stopped and the automation creates an Amazon Machine Image \(AMI\)\. When the AMI status is `available`, a new Amazon EC2 instance is launched from this AMI in the `DestinationSubnetId` you specify in your VPC\. If your Amazon EC2 instance that's running in EC2\-Classic has an Elastic IP address attached, the instance will be moved to the newly created Amazon EC2 instance in your VPC\.If the Amazon EC2 instance launching in your VPC fails to create for any reason, it is terminated and approval is requested to start your Amazon EC2 instance in EC2\-Classic\.

  The `Test` option creates an AMI of your Amazon EC2 instance that's running in EC2\-Classic without rebooting\. Because the Amazon EC2 instance does not reboot, we can't guarantee the file system integrity of the created image\. When the AMI status is `available`, a new Amazon EC2 instance is launched from this AMI in the `DestinationSubnetId` you specify in your VPC\. If your Amazon EC2 instance that's running in EC2\-Classic has an Elastic IP address attached, the automation verifies the `DestinationSubnetId` you specify is public\. If the Amazon EC2 instance launching in your VPC fails to create for any reason, it is terminated and the automation ends\.
+ SNSNotificationARNforApproval

  Type: String

  Description: \(Optional\) The ARN of the Amazon Simple Notification Service \(Amazon SNS\) topic you want to send approval requests to\. This parameter only applies if you specify the `CutOver` value for the `MigrationType` parameter\.
+ TargetInstanceType

  Type: String

  Default: t2\.2xlarge

  Description: \(Optional\) The type of Amazon EC2 instance you want to launch in your VPC\. Only T2 instances are supported\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `sns:GetTopicAttributes`
+ `sns:ListSubscriptions`
+ `sns:ListTopics`
+ `sns:Publish`
+ `ec2:AssociateAddress`
+ `ec2:AuthorizeSecurityGroupIngress`
+ `ec2:CreateImage`
+ `ec2:CreateSecurityGroup`
+ `ec2:DeleteSecurityGroup`
+ `ec2:MoveAddressToVpc`
+ `ec2:RunInstances`
+ `ec2:StopInstances`
+ `ec2:CreateTags`
+ `ec2:DescribeAddresses`
+ `ec2:DescribeInstanceAttribute`
+ `ec2:DescribeInstances`
+ `ec2:DescribeInstanceStatus`
+ `ec2:DescribeRouteTables`
+ `ec2:DescribeSecurityGroupReferences`
+ `ec2:DescribeSecurityGroups`
+ `ec2:DescribeSubnets`
+ `ec2:DescribeTags`
+ `ec2:DescribeVpcs`
+ `ec2:DescribeImages`

**Document Steps**
+ aws:executeAwsApi \- Gathers details about the Amazon EC2 instance you specify in the `InstanceId` parameter\.
+ aws:branch \- Branches based on whether Amazon EC2 instance you specify in the `InstanceId` parameter is in EC2\-Classic\.
+ aws:assertAwsResourceProperty \- Confirms the Amazon EC2 instance you specify in the `InstanceId` parameter is of the HVM virtualization type\.
+ aws:assertAwsResourceProperty \- Confirms the Amazon EC2 instance you specify in the `InstanceId` parameter has an Amazon EBS root volume\.
+ aws:executeScript \- Creates a security group as needed depending on the value you specify for the `DestinationSecurityGroupId` parameter\.
+ aws:branch \- Branches based on the value you specify in the `DestinationSubnetId` parameter\.
+ aws:executeAwsApi \- Identifies the default VPC in the AWS Region where you run this automation\.
+ aws:executeAwsApi \- Randomly chooses the ID of a subnet located in the default VPC\.
+ aws:createImage \- Creates an AMI without rebooting the Amazon EC2 instance\.
+ aws:branch \- Branches based on the value you specify for the `MigrationType` parameter\.
+ aws:branch \- Branches based on the value you specify for the `DestinationSubnetId` parameter\.
+ aws:runInstances \- Launches a new instance from the AMI created without rebooting the Amazon EC2 instance in EC2\-Classic\.
+ aws:changeInstanceState \- Terminates the newly launched Amazon EC2 instance if the previous step fails for any reason\.
+ aws:runInstances \- Launches a new instance from the AMI created without rebooting the Amazon EC2 instance in EC2\-Classic in the `DestinationSubnetId` if provided\.
+ aws:changeInstanceState \- Terminates the newly launched Amazon EC2 instance if the previous step fails for any reason\.
+ aws:assertAwsResourceProperty \- Confirms the stop behavior for the Amazon EC2 instance running in EC2\-Classic\.
+ aws:approve \- Waits for approval to stop the Amazon EC2 instance\.
+ aws:changeInstanceState \- Stops the Amazon EC2 instance running in EC2\-Classic\.
+ aws:changeInstanceState \- Force stops the Amazon EC2 instance running in EC2\-Classic if needed\.
+ aws:createImage \- Creates an AMI of the Amazon EC2 instance after it has stopped\.
+ aws:branch \- Branches based on the value specified for the `DestinationSubnetId` parameter\.
+ aws:runInstances \- Launches a new instance from the AMI created of the stopped Amazon EC2 instance in EC2\-Classic\.
+ aws:approve \- Waits for approval to terminate the newly launched instance and start the Amazon EC2 instance in EC2\-Classic if the previous step fails for any reason\.
+ aws:changeInstanceState \- Terminates the newly launched Amazon EC2 instance\.
+ aws:runInstances \- Launches a new instance from the AMI created of the stopped Amazon EC2 instance in EC2\-Classic in the `DestinationSubnetId`\.
+ aws:approve \- Waits for approval to terminate the newly launched instance and start the Amazon EC2 instance in EC2\-Classic if the previous step fails for any reason\.
+ aws:changeInstanceState \- Terminates the newly launched Amazon EC2 instance\.
+ aws:changeInstanceState \- Starts the Amazon EC2 instance that was stopped in EC2\-Classic\.
+ aws:branch \- Branches based on whether the Amazon EC2 instance has a public IP address\.
+ aws:executeAwsApi \- Verifies whether the public IP address is an EIP\.
+ aws:branch \- Branches based on the value you specify in the `MigrationType` parameter\.
+ aws:executeAwsApi \- Moves the EIP to your VPC\.
+ aws:executeAwsApi \- Gathers the allocation ID of the EIP that was moved to your VPC\.
+ aws:branch \- Branches based on which subnet the Amazon EC2 instance running in your VPC was launched\.
+ aws:executeAwsApi \- Attaches the EIP to the newly launched instance in your VPC\.
+ aws:executeScript \- Confirms the subnet your newly launched Amazon EC2 instance running in your VPC is public\.

**Outputs**

getInstanceProperties\.virtualizationType \- The virtualization type of the Amazon EC2 instance running in EC2\-Classic\.

getInstanceProperties\.rootDeviceType \- The root device type of the Amazon EC2 instance running in EC2\-Classic\.

createAMIWithoutReboot\.ImageId \- The ID of the AMI created without rebooting the Amazon EC2 instance running in EC2\-Classic\.

getDefaultVPC\.VpcId \- The ID of the default VPC where the new Amazon EC2 instance will be launched if a value for the `DestinationSubnetId` parameter is not provided\.

getSubnetIdinDefaultVPC\.subnetIdFromDefaultVpc \- The ID of the subnet in the default VPC where the new Amazon EC2 instance will be launched if a value for the `DestinationSubnetId` parameter is not provided\.

launchTestInstanceDefaultVPC\.InstanceIds \- The ID of the newly launched Amazon EC2 instance in your default VPC during the `Test` migration type\.

launchTestInstanceProvidedSubnet\.InstanceIds \- The ID of the newly launched Amazon EC2 instance in the `DestinationSubnetId` you specified during the `Test` migration type\.

createAMIAfterStoppingInstance\.ImageId \- The ID of the AMI created after stopping the Amazon EC2 instance running in EC2\-Classic\.

launchCutOverInstanceProvidedSubnet\.InstanceIds \- The ID of the newly launched Amazon EC2 instance in the `DestinationSubnetId` you specified during the `CutOver` migration type\.

launchCutOverInstanceDefaultVPC\.InstanceIds \- The ID of the newly launched Amazon EC2 instance in your default VPC during the `CutOver` migration type\.

verifySubnetIsPublicTestDefaultVPC\.IsSubnetPublic \- Whether the subnet chosen by the automation in your default VPC is public or not\.

verifySubnetIsPublicTestProvidedSubnet\.IsSubnetPublic \- Whether the subnet you specified in the `DestinationSubnetId` is public or not\.