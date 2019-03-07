# Systems Manager Prerequisites<a name="systems-manager-prereqs"></a>

AWS Systems Manager helps you configure and manage Amazon EC2 instances, on\-premises servers and virtual machines \(VMs\), and other AWS resources at scale\.

This section describes the prerequisites for setting up and configuring your Amazon EC2 instances and your on\-premises servers or VMs for Systems Manager\. This section uses the term *managed instance* to describe EC2 instances or on\-premises servers and VMs that are configured for Systems Manager\. 

**Important**  
Before you start using Systems Manager, we recommend that you learn about the following AWS services\. A working knowledge of these services is essential for successfully setting up and using Systems Manager\. 
+ **Amazon Elastic Compute Cloud \(Amazon EC2\)** provides scalable computing capacity in the AWS Cloud\. For more information, see [What is Amazon EC2?](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) \(Linux\) and [What is Amazon EC2?](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/concepts.html) \(Windows\)\.
+ **AWS Identity and Access Management \(IAM\)** is a web service that helps you securely control access to AWS resources\. For more information, see [What is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*\.

**Topics**
+ [Operating Systems](#prereqs-operating-systems)
+ [SSM Agent](#prereqs-ssm-agent)
+ [Interface VPC Endpoint or Internet Access](#prereqs-vpc-endpoint-or-net-access)
+ [TLS Certificate](#prereqs-tls-certificate)
+ [Systems Manager Access Configurations](#prereqs-access-configurations)
+ [Windows PowerShell](#prereqs-powershell)
+ [AWS Regions](#prereqs-regions)
+ [\(Optional\) Monitoring and Notifications](#prereqs-monitoring-and-notifications)
+ [\(Optional\) Amazon S3 Storage Bucket](#prereqs-s3)

## Operating Systems<a name="prereqs-operating-systems"></a>

The following operating systems are supported by AWS Systems Manager\.

**Topics**
+ [Windows Server](#prereqs-os-windows-server)
+ [Linux](#prereqs-os-linux)
+ [Raspbian](#prereqs-os-raspbian)

### Windows Server<a name="prereqs-os-windows-server"></a>


****  

| Version | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 2003 and 2003 R2 | ✓ | ✓ |  | 
| 2008 | ✓ | ✓ |  | 
| 2008 R2 |  | ✓ |  | 
| 2012 and 2012 R2 |  | ✓ |  | 
| 2016 |  | ✓ |  | 
| 2019 |  | ✓ |  | 

### Linux<a name="prereqs-os-linux"></a>


**Amazon Linux**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 2012\.03 – 2018\.03 | ✓ | ✓ |  | 

**Note**  
Beginning with version 2015\.03, Amazon Linux is released in Intel 64\-bit \(x86\_64\) versions only\.


**Amazon Linux 2**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 2\.0 and all later versions |  | ✓ | ✓ | 


**Ubuntu Server**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 12\.04 LTS and 14\.04 LTS | ✓ | ✓ |  | 
| 16\.04 LTS and 18\.04 LTS |  | ✓ | ✓ | 


**Red Hat Enterprise Linux \(RHEL\)**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 6\.0 |  | ✓ |  | 
| 6\.5 | ✓ | ✓ |  | 
| 6\.9 | ✓ | ✓ |  | 
| 7\.0 |  | ✓ |  | 
| 7\.4 |  | ✓ |  | 
| 7\.5 |  | ✓ |  | 
| 7\.6 |  | ✓ | ✓ | 


**CentOS**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 6\.0 | ✓ |  |  | 
| 6\.3 and later 6\.x versions | ✓ | ✓ |  | 
| 7\.1 and later 7\.x versions |  | ✓ | ✓ | 


**SUSE Linux Enterprise Server \(SLES\)**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 12 and later 12\.x versions |  | ✓ |  | 

### Raspbian<a name="prereqs-os-raspbian"></a>


****  

| Version | ARM 32\-bit \(arm\) | 
| --- | --- | 
| Jessie | ✓ | 
| Stretch | ✓ | 

## SSM Agent<a name="prereqs-ssm-agent"></a>

SSM Agent is the tool that processes Systems Manager requests and configures your machine as specified in the request\. SSM Agent must be installed on each instance you want to use with Systems Manager\. On some instance types, SSM Agent is installed by default\. On others, you must install it manually, as described in the following table\.

**Note**  
SSM Agent is updated whenever changes are made to Systems Manager and when new capabilities are added\. To ensure that your instances are always running the newest version of SSM Agent, we recommend that you update the agent automatically whenever a new version is available using either of the following methods\.  
Use a State Manager association\. For information, see the State Manager topic [Automatically Update SSM Agent \(CLI\)](sysman-state-cli.md)\.
Use a Maintenance Window\. For information, see the Maintenance Window topics [Automatically Update SSM Agent \(AWS CLI\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/mw-walkthrough-cli.html) and [Automatically Update SSM Agent \(Console\)](https://docs.aws.amazon.com/systems-manager/latest/userguide/mw-walkthrough-console.html)\. 
You can also use Run Command to manually update one or more instances with the latest version\. For more information, see [Automatically Update SSM Agent \(CLI\)](sysman-state-cli.md) \(State Manager\) and [Update SSM Agent by using Run Command](rc-console.md#rc-console-agentexample)\.


****  

| Operating System Type | Description | 
| --- | --- | 
| Windows |  SSM Agent is installed by default on Windows Server 2016 and 2019 instances, as well as on instances created from Windows Server 2003\-2012 R2 AMIs published in November 2016 or later\. Windows AMIs published before November 2016 use the EC2Config service to process requests and configure instances\. Unless you have a specific reason for using the EC2Config service or an earlier version of SSM Agent to process Systems Manager requests, we recommend that you download and install the latest version of the SSM Agent to each of your Amazon EC2 instances or managed instances \(servers and virtual machines \(VMs\) in a hybrid environment\)\. For more information, see [Installing and Configuring SSM Agent on Windows Instances](sysman-install-ssm-win.md)\.  | 
| Linux | SSM Agent is installed by default on Amazon Linux, Amazon Linux 2, Ubuntu Server 16\.04, and Ubuntu Server 18\.04 LTS base EC2 AMIs\. You must manually install SSM Agent on other versions of EC2 Linux, including non\-base images like Amazon ECS\-Optimized AMIs\. For more information, see [Installing and Configuring SSM Agent on Amazon EC2 Linux Instances](sysman-install-ssm-agent.md)\. | 
| On\-premises servers and VMs |  SSM Agent must be installed manually on on\-premises servers and VMs you want to use in a hybrid environment\. The SSM Agent download and installation process for these machines is different than the process used for Amazon EC2 instances\. For more information, see [Installing SSM Agent on Servers and Virtual Machines in a Windows Hybrid Environment](sysman-install-managed-win.md)\.  | 

**Note**  
The source code for SSM Agent is available on [GitHub](https://github.com/aws/amazon-ssm-agent) so that you can adapt the agent to meet your needs\. We encourage you to submit [pull requests](https://github.com/aws/amazon-ssm-agent/blob/master/CONTRIBUTING.md) for changes that you would like to have included\. However, Amazon Web Services does not currently provide support for running modified copies of this software\.

## Interface VPC Endpoint or Internet Access<a name="prereqs-vpc-endpoint-or-net-access"></a>

In order for your managed instances and the Systems Manager service to communicate with each other, you must do one of the following:
+ Configure Systems Manager to use an interface Virtual Private Cloud \(VPC\) endpoint
+ Enable outbound internet access on your managed instances

**Note**  
Enabling inbound internet access is not required\.

To enhance the security posture of your managed instance, we recommend that you configure Systems Manager to use an interface VPC endpoint\. Interface endpoints are powered by PrivateLink, a technology that enables you to privately access Amazon EC2 and Systems Manager APIs by using private IP addresses\. PrivateLink restricts all network traffic between your managed instances, Systems Manager, and EC2 to the Amazon network \(managed instances don't have access to the internet\)\. Also, you don't need an internet gateway, a NAT device, or a virtual private gateway\. For more information, see [Setting Up VPC Endpoints for Systems Manager](sysman-setting-up-vpc.md)\. For more information about PrivateLink and VPC endpoints, see [Accessing AWS Services Through PrivateLink](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Introduction.html#what-is-privatelink)\.

## TLS Certificate<a name="prereqs-tls-certificate"></a>

Each of your managed instances must have one of the following Transport Layer Security \(TLS\) certificates installed\.
+ Amazon Root CA 1
+ Starfield Services Root Certificate Authority \- G2
+ Starfield Class 2 Certificate Authority

AWS services use these certificates to encrypt calls to other AWS services\. One of these certificates is installed, by default, on all Amazon Machine Images \(AMIs\)\. For base operating systems, or systems in your on\-premises environment, you must install and enable one of these certificates from [Amazon Trust Services](https://www.amazontrust.com/repository/)\. If certificates in your computing environment are managed by a Group Policy Object \(GPO\), then you might need to configure Group Policy to include one of these certificates\. For more information about the Amazon Root and Starfield certificates, see [How to Prepare for AWS’s Move to Its Own Certificate Authority](https://aws.amazon.com/blogs/security/how-to-prepare-for-aws-move-to-its-own-certificate-authority/)\.

## Systems Manager Access Configurations<a name="prereqs-access-configurations"></a>

Configuring access to Systems Manager requires that you do the following:
+ **Configure user access to Systems Manager**: See [Task 1: Configure User Access for Systems Manager](sysman-access-user.md)\.
+ **Configure access between your EC2 instances and Systems Manager**: See [Task 2: Create an Instance Profile for Systems Manager](sysman-configuring-access-role.md)\. This step is *not* required for your on\-premises servers or VMs\.
+ **Configure access between your on\-premises servers/VMs and Systems Manager**: See [Creating an IAM Service Role for a Hybrid Environment](sysman-service-role.md)\. This step is *not* required for your EC2 instances\.

For more information about access permissions for Systems Manager, see [Authentication and Access Control for AWS Systems Manager](auth-and-access-control.md)\. 

## Windows PowerShell<a name="prereqs-powershell"></a>

On your Windows Server instances, Windows PowerShell 3\.0 or later is required to run certain SSM documents \(for example, the `AWS-ApplyPatchBaseline` document\)\. Verify that your Windows instances are running Windows Management Framework 3\.0 or later\. The framework includes PowerShell\. 

## AWS Regions<a name="prereqs-regions"></a>

AWS Systems Manager is available in the AWS Regions listed in the [AWS Systems Manager Supported Regions](https://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) table in the *AWS General Reference*\. Before starting your Systems Manager configuration process, we recommend that you ensure the service is available in each of the AWS Regions you want to use it in\. 

For servers and VMs in your hybrid environment, we recommend that you choose the Region closest to your data center or computing environment\.

## \(Optional\) Monitoring and Notifications<a name="prereqs-monitoring-and-notifications"></a>

You can configure Amazon CloudWatch Events to log status execution changes of the commands you send using Systems Manager\. You can also configure Amazon Simple Notification Service \(Amazon SNS\) to send you notifications about specific command status changes\. Using monitoring and notifications is optional, but we recommend setting them up at the beginning of your Systems Manager configuration process if you have decided to use either one\. For more information, see [Understanding Command Statuses](monitor-commands.md)\.

For information about using CloudWatch Events to monitor Systems Manager events, see [Monitoring Systems Manager Events with Amazon CloudWatch Events](monitoring-cloudwatch-events.md)\.

## \(Optional\) Amazon S3 Storage Bucket<a name="prereqs-s3"></a>

Command output in the Systems Manager console is truncated after 2500 characters\. In order to access complete output logs, you can store Systems Manager output in an Amazon Simple Storage Service \(Amazon S3\) bucket\. You can also create an Amazon S3 key prefix \(a subfolder\) to help you organize the log output\. Saving output log data in an S3 bucket is optional, but we recommend setting it up at the beginning of your Systems Manager configuration process if you have decided to use it\. For more information, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)\.

For information about Systems Manager limits, see [AWS Systems Manager Limits](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_ssm)\.