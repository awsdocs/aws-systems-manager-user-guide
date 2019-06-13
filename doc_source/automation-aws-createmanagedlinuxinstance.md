# AWS\-CreateManagedLinuxInstance<a name="automation-aws-createmanagedlinuxinstance"></a>

**Description**

Create an Amazon EC2 Linux instance that is configured for Systems Manager\.

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AmiId

  Type: String

  Description: \(Required\) AMI ID to use for launching the instance\.
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf
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

**Examples**

Start the automation

```
aws ssm start-automation-execution --document-name AWS-CreateManagedLinuxInstance --parameters parameters
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

**Document Steps**

aws:createStack

aws:deleteStack

**Outputs**

None