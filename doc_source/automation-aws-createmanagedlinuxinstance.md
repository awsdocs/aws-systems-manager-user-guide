# AWS\-CreateManagedLinuxInstance<a name="automation-aws-createmanagedlinuxinstance"></a>

**Description**

Create an EC2 instance for Linux that is configured for Systems Manager\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-CreateManagedLinuxInstance)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Linux

**Parameters**
+ AmiId

  Type: String

  Description: \(Required\) AMI ID to use for launching the instance\.
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ GroupName

  Type: String

  Default: SSMSecurityGroupForLinuxInstances

  Description: \(Required\) Security group name to create\.
+ InstanceType

  Type: String

  Default: t2\.medium

  Description: \(Required\) Type of instance to launch\. Default is t2\.medium\.
+ KeyPairName

  Type: String

  Description: \(Required\) Key pair to use when creating instance\.
+ RemoteAccessCidr

  Type: String

  Default: 0\.0\.0\.0/0

  Description: \(Required\) Creates Security group with port for SSH\(Port range 22\) open to IPs specified by CIDR \(default is 0\.0\.0\.0/0\)\. If the security group already exists it will not be modified and rules will not be changed\.
+ RoleName

  Type: String

  Default: SSMManagedInstanceProfileRole

  Description: \(Required\) Role name to create\.
+ StackName

  Type: String

  Default: CreateManagedInstanceStack\{\{automation:EXECUTION\_ID\}\}

  Description: \(Optional\) Specify stack name used by this document
+ SubnetId

  Type: String

  Default: Default

  Description: \(Required\) New instance will be deployed into this subnet or in the default subnet if not specified\.
+ VpcId

  Type: String

  Default: Default

  Description: \(Required\) New instance will be deployed into this Amazon Virtual Private Cloud \(Amazon VPC\) or in the default Amazon VPC if not specified\.