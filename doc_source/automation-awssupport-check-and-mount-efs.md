# AWSSupport\-CheckAndMountEFS<a name="automation-awssupport-check-and-mount-efs"></a>

**Description**

The AWSSupport\-CheckAndMountEFS runbook verifies the prerequisites to mount your Amazon Elastic File System \(Amazon EFS\) file system and mounts the file system on the Amazon Elastic Compute Cloud \(Amazon EC2\) instance you specify\. This runbook supports mounting your Amazon EFS file system with the DNS name, or using the mount target’s IP address\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-CheckAndMountEFS)

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
+ Action

  Type: String

  Valid values: Check \| CheckAndMount

  Description: \(Required\) Determines whether the runbook verifies prerequisites, or verifies prerequisites and mounts the file system\.
+ EfsId

  Type: String

  Description: \(Required\) The ID of the file system you want to mount\.
+ InstanceId

  Type: String

  Description: \(Required\) The ID of the Amazon EC2 instance on which you want to mount the file system\.
+ MountOptions

  Type: String

  Description: \(Optional\) The options supported by the Amazon EFS mount helper that you want to use when mounting the file system\. If you specify the `tls` option, verify stunnel has been upgraded on the target instance\.
+ MountPoint

  Type: String

  Description: \(Optional\) The directory where you want to mount the file system\. If you specify the `Check` value for the `Action` parameter, this parameter should not be specified\.
+ MountTargetIP

  Type: String

  Description: \(Optional\) The mount target's IP address\. Mounting by IP address works in environments where DNS is disabled, such as virtual private clouds \(VPCs\) with DNS hostnames disabled\. Also, you can use this option if your environment uses a DNS provider other than Amazon Route 53 \(Route 53\)\.
+ Region

  Type: String

  Description: \(Required\) The AWS Region where the Amazon EC2 instance and file system are located\.

**Required IAM permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:DescribeAutomationExecutions`
+ `ssm:DescribeAutomationStepExecutions`
+ `ssm:DescribeAutomationStepExecutions`
+ `ssm:DescribeInstanceInformation`
+ `ssm:DescribeInstanceProperties`
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `ssm:GetDocument`
+ `ssm:ListCommands`
+ `ssm:ListCommandInvocations`
+ `ssm:ListDocuments`
+ `ssm:StartAutomationExecution`
+ `iam:ListRoles`
+ `ec2:DescribeInstances`
+ `ec2:DescribeSecurityGroups`
+ `elasticfilesystem:DescribeFileSystemPolicy`
+ `elasticfilesystem:DescribeMountTargets`
+ `elasticfilesystem:DescribeMountTargetSecurityGroups`
+ `resource-groups:*`

**Document Steps**
+ aws:executeScript \- Gathers details about the Amazon EC2 instance you specify in the `InstanceId` parameter\.
+ aws:executeScript \- Gathers details about the file system you specify in the `EfsId` parameter\.
+ aws:executeScript \- Verifies the security group associated with the file system allows traffic on port 2049 from the Amazon EC2 instance you specify in the `InstanceId` parameter\.
+ aws:assertAwsResourceProperty \- Verifies the Amazon EC2 instance you specify in the `InstanceId` parameter is managed by Systems Manager and that the status is `Online`\.
+ aws:branch \- Branches based on the value you specify for the `Action` parameter\.
+ aws:runCommand \- Verifies prerequisites for mounting the file system you specify in the `EfsId` parameter\.
+ aws:runCommand \- Verifies prerequisites for mounting the file system you specify in the `EfsId` parameter, and mounts the file system on the Amazon EC2 instance you specify in the `InstanceId` parameter\.