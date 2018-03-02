# Setting Up AWS Systems Manager<a name="systems-manager-setting-up"></a>

This section describes tasks and prerequisites for setting up AWS Systems Manager\. Use the following table to help you get started\.


****  

| What do you want to do with Systems Manager? | Set up tasks | 
| --- | --- | 
|  Test it out / Play with it  |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-setting-up.html)  | 
| Use Systems Manager to manage and configure my existing EC2 instances |  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-setting-up.html) | 
| Use Systems Manager to manage and configure my servers and VMs in a hybrid environment | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-setting-up.html) | 

## Systems Manager Prerequisites<a name="systems-manager-prereqs"></a>

Systems Manager includes the following prerequisites\.


****  

| Requirement | Description | 
| --- | --- | 
|  Supported Operating System \(Windows\)  |  Instances must run a supported version of Windows Server: Windows Server 2003 through Windows Server 2016, including R2 versions\. Patch Manager currently supports a different set of Windows operating systems\. For information, see [Operating Systems Supported by Patch Manager](patch-manager-supported-oses.md)\.   | 
|  Supported Operating System \(Linux\)  |  Instances must run a supported version of Linux\.  Patch Manager currently supports a different set of Linux operating systems\. For information, see [Operating Systems Supported by Patch Manager](patch-manager-supported-oses.md)\.  [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-setting-up.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-setting-up.html) [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-setting-up.html)  | 
|  Supported Regions  |  Systems Manager is [available in these regions](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region)\. For servers and VMs in your hybrid environment, we recommend that you choose the region closest to your data center or computing environment\.  | 
|  Access to Systems Manager  |  Systems Manager requires an IAM role for instances that will process commands and a separate role for users executing commands\. Both roles require permission policies that enable them to communicate with the Systems Manager API\. You can choose to use Systems Manager managed policies or you can create your own roles and specify permissions\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\.  If you are configuring on\-premises servers or VMs that you want to configure using Systems Manager, you must also configure an IAM service role\. For more information, see [Create an IAM Service Role](systems-manager-managedinstances.md#sysman-service-role)\.  | 
|  SSM Agent \(EC2 Windows instances\)  |  SSM Agent processes Systems Manager requests and configures your machine as specified in the request\. The SSM Agent is installed by default on Windows Server 2016 instances and instances created from Windows Server 2003\-2012 R2 AMIs published in November 2016 or later\. Windows AMIs published before November 2016 use the EC2Config service to process requests and configure instances\. Unless you have a specific reason for using the EC2Config service or an earlier version of the SSM Agent to process Systems Manager requests, we recommend that you download and install the latest version of the SSM Agent to each of your Amazon EC2 instances or managed instances \(servers and VMs in a hybrid environment\)\. For more information, see [Installing and Configuring SSM Agent on Windows Instances](sysman-install-ssm-win.md)\.  | 
|  SSM Agent \(EC2 Linux instances\)  |  SSM Agent processes Systems Manager requests and configures your machine as specified in the request\. SSM Agent is installed, by default, on Amazon Linux *base* AMIs dated 2017\.09 and later\. You must manually install SSM Agent on other versions of EC2 Linux, including non\-base images like *Amazon ECS\-Optimized AMIs*\. For more information, see [Installing and Configuring SSM Agent on Linux Instances](sysman-install-ssm-agent.md)\. The source code for SSM Agent is available on [GitHub](https://github.com/aws/amazon-ssm-agent) so that you can adapt the agent to meet your needs\. We encourage you to submit [pull requests](https://github.com/aws/amazon-ssm-agent/blob/master/CONTRIBUTING.md) for changes that you would like to have included\. However, Amazon Web Services does not currently provide support for running modified copies of this software\.  | 
|  SSM Agent \(hybrid environment\)  |  The SSM Agent download and installation process for managed instances in a hybrid environment is different than Amazon EC2 instances\. For more information, see [Install the SSM Agent on Servers and VMs in Your Windows Hybrid Environment](systems-manager-managedinstances.md#sysman-install-managed-win)\.  | 
|  Windows PowerShell 3\.0 or Later  |  SSM Agent requires Windows PowerShell 3\.0 or later to execute certain SSM Documents on Windows instances \(for example, the AWS\-ApplyPatchBaseline document\)\. Verify that your Windows instances are running Windows Management Framework 3\.0 or later\. The framework includes PowerShell\. For more information, see [Windows Management Framework 3\.0](https://www.microsoft.com/en-us/download/details.aspx?id=34595&751be11f-ede8-5a0c-058c-2ee190a24fa6=True)\.  | 
|  Internet Access  |  Verify that your EC2 instances have outbound Internet access\. Inbound Internet access is not required\.  | 
|  Configure Monitoring and Notifications \(Optional\)  |  You can configure Amazon CloudWatch Events to log status execution changes of the commands you send using Systems Manager\. You can also configure Amazon Simple Notification Service \(Amazon SNS\) to send you notifications about specific command status changes\. For more information, see [Understanding Command Statuses](monitor-commands.md)\.  | 
|  Amazon S3 Bucket \(Optional\)  |  You can store System Manager output in an Amazon Simple Storage Service \(Amazon S3\) bucket\. Output in the Amazon EC2 console is truncated after 2500 characters\. Additionally, you might want to create an Amazon S3 key prefix \(a subfolder\) to help you organize output\. For more information, see [Create a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)\.  | 

For information about Systems Manager limits, see [AWS Systems Manager Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_ssm)\. To increase limits, go to [AWS Support Center](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-ec2-instances) and submit a limit increase request form\.

## Ec2messages and Undocumented API Calls<a name="systems-manager-setting-up-messageAPIs"></a>

If you monitor API calls, you will see calls to the following APIs\.

+ ec2messages:AcknowledgeMessage

+ ec2messages:DeleteMessage

+ ec2messages:FailMessage

+ ec2messages:GetEndpoint

+ ec2messages:GetMessages

+ ec2messages:SendReply

+ UpdateInstanceInformation

+ ListInstanceAssociations

+ DescribeInstanceProperties

+ DescribeDocumentParameters

Calls to `ec2messages:*` APIs are calls to the ec2messages endpoint\. Systems Manager uses this endpoint to make calls from the SSM Agent to the Systems Manager service in the cloud\. This endpoint is required to send and receive commands\.

`UpdateInstanceInformation`: SSM Agent calls the Systems Manager service in the cloud every five minutes to provide heartbeat information\. This call is necessary to maintain a heartbeat with the agent so that the service knows the agent is functioning as expected\. 

`ListInstanceAssociations`: The agent calls this API to see if a new Systems Manager State Manager association is available\. This API is required for State Manager to function\.

`DescribeInstanceProperties` and `DescribeDocumentParameters`: Systems Manager calls these APIs to render specific nodes in the Amazon EC2 console\. The `DescribeInstanceProperties` API displays the **Managed Instances** node in the left navigation\. The `DescribeDocumentParameters` API displays the **Documents** node in the left navigation\.