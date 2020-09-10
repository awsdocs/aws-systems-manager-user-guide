# Troubleshooting managed instances<a name="troubleshooting-managed-instances"></a>

Use the following information to help you troubleshoot problems with your managed instances\.

## Where are my instances?<a name="missing-instances"></a>

For several Systems Manager operations, you can choose to manually select the instances on which you will run the operation\. Examples include running a Run Command command, specifying maintenance window targets, and installing Distributor packages\. In cases like these, after you specify that you want to choose instances manually, a list is displayed of managed instances you can choose to run the operation on\.

If an instance you expect to see is not listed, check the following requirements\.
+ **SSM Agent**: Make sure the latest version of SSM Agent is installed on the instance\. Only Amazon Machine Images \(AMIs\) for Windows Server and some Linux AMIs are pre\-configured with SSM Agent\. For information about installing or reinstalling SSM Agent on an instance, see [Installing and configuring SSM Agent on EC2 instances for Linux](sysman-install-ssm-agent.md) or [Installing and configuring SSM Agent on EC2 instances for Windows Server](sysman-install-ssm-win.md)\.
+ ** IAM instance role**: Verify that the instance is configured with an AWS Identity and Access Management \(IAM\) role that enables the instance to communicate with the Systems Manager API\. Also verify that your user account has an IAM user trust policy that enables your account to communicate with the Systems Manager API\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. 
+ **Service Endpoint connectivity**: Verify that the instance has connectivity to the Systems Manager service endpoints\. This connectivity is provided by creating and configuring VPC endpoints for Systems Manager, or by allowing HTTPS \(port 443\) outbound traffic to the service endpoints\. For more information, see [Step 6: \(Optional\) Create a Virtual Private Cloud endpoint](setup-create-vpc.md)\.
+ **Target operating system type**: Double\-check that you have selected an SSM document that supports the type of instance you want to update\. Most SSM documents support both Windows and Linux instances, but some do not\. For example, if you select the SSM document `AWS-InstallPowerShellModule`, which applies only to Windows Server instances, you will not see Linux instances in the target instances list\.

## What's the status of my Windows instances?<a name="rc-healthapi-win"></a>

Use the following PowerShell command to get status details about one or more instances:

```
Get-SSMInstanceInformation `
	-InstanceInformationFilterList @{Key="InstanceIds";ValueSet="instance-ID-1","instance-ID-2"}
```

Use the following PowerShell command with no filters to see all instances registered to your account that are currently reporting an online status\. Substitute the ValueSet="Online" with "ConnectionLost" or "Inactive" to view those statuses:

```
Get-SSMInstanceInformation `
	-InstanceInformationFilterList @{Key="PingStatus";ValueSet="Online"}
```

Use the following PowerShell command to see which instances are running the latest version of the EC2Config service\. Substitute `ValueSet="LATEST"` with a specific version \(for example, `3.0.54` or `3.10`\) to view those details:

```
Get-SSMInstanceInformation `
	-InstanceInformationFilterList @{Key="AgentVersion";ValueSet="LATEST"}
```

## What's the status of my Linux instances?<a name="rc-healthapi-linux"></a>

Use the following AWS CLI command to get status details about one or more instances\.

------
#### [ Linux ]

```
aws ssm describe-instance-information \
	--instance-information-filter-list key=InstanceIds,valueSet=instance-ID
```

------
#### [ Windows ]

```
aws ssm describe-instance-information ^
	--instance-information-filter-list key=InstanceIds,valueSet=instance-ID
```

------

Use the following command with no filters to see all instances registered to your account that are currently reporting an online status\. Substitute the `ValueSet="Online"` with `"ConnectionLost"` or `"Inactive"` to view those statuses\.

------
#### [ Linux ]

```
aws ssm describe-instance-information \
	--instance-information-filter-list key=PingStatus,valueSet=Online
```

------
#### [ Windows ]

```
aws ssm describe-instance-information ^
	--instance-information-filter-list key=PingStatus,valueSet=Online
```

------

Use the following command to see which instances are running the latest version of SSM Agent\. Substitute `ValueSet="LATEST"` with a specific version \(for example, `1.0.145` or `1.0`\) to view those details\.

------
#### [ Linux ]

```
aws ssm describe-instance-information \
	--instance-information-filter-list key=AgentVersion,valueSet=LATEST
```

------
#### [ Windows ]

```
aws ssm describe-instance-information ^
	--instance-information-filter-list key=AgentVersion,valueSet=LATEST
```

------

If the describe\-instance\-information API operation returns an AgentStatus of `Online`, then your instance is ready to be managed using Run Command\. If the status is `Inactive`, the instance has one or more of the following problems\. 
+ SSM Agent is not installed\.
+ The instance does not have outbound internet connectivity\.
+ The instance was not launched with an IAM role that enables it to communicate with the SSM API, or the permissions for the IAM role are not correct for Run Command\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\.