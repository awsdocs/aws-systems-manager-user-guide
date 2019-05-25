# Systems Manager Prerequisites<a name="systems-manager-prereqs"></a>

The prerequisites for using AWS Systems Manager to manage your Amazon EC2 instances, and your on\-premises servers or virtual machines \(VMs\), are covered in the *Setting Up* chapters of this user guide\. This topic provides additional details about some of those requirements\. 

Before you start setting up Systems Manager for your organization, we recommend that you learn about the following AWS services\. A working knowledge of these services is essential for successfully setting up Systems Manager\. 
+ **Amazon Elastic Compute Cloud \(Amazon EC2\)** provides scalable computing capacity in the AWS Cloud\. For more information, see [What is Amazon EC2?](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html) \(Linux\) and [What is Amazon EC2?](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/concepts.html) \(Windows\)\.
+ **AWS Identity and Access Management \(IAM\)** is a web service that helps you securely control access to AWS resources\. For more information, see [What is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*\.

**Topics**
+ [Prerequisite: Supported Operating Systems](#prereqs-operating-systems)
+ [Prerequisite: SSM Agent](#prereqs-ssm-agent)
+ [Prerequisite: Interface VPC Endpoint or Internet Access](#prereqs-vpc-endpoint-or-net-access)
+ [Prerequisite: TLS Certificates](#prereqs-tls-certificate)

## Prerequisite: Supported Operating Systems<a name="prereqs-operating-systems"></a>

Your Amazon EC2 instances, on\-premises servers, and VMs must be running one of the following operating systems in order to be used with AWS Systems Manager\.

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

## Prerequisite: SSM Agent<a name="prereqs-ssm-agent"></a>

AWS Systems Manager Agent \(SSM Agent\) is Amazon software that can be installed and configured on an Amazon EC2 instance, an on\-premises server, or a virtual machine \(VM\)\. SSM Agent makes it possible for Systems Manager to update, manage, and configure these resources\. The agent processes requests from the Systems Manager service in the AWS Cloud, and then runs them as specified in the request\. SSM Agent then sends status and execution information back to the Systems Manager service by using the Amazon Message Delivery Service \(service prefix: `ec2messages`\)\.

SSM Agent must be installed on each instance you want to use with Systems Manager\. SSM Agent is preinstalled, by default, on instances created from the following Amazon Machine Images \(AMIs\): 
+ Windows Server 2003\-2012 R2 AMIs published in November 2016 or later
+ Windows Server 2016 and 2019
+ Amazon Linux
+ Amazon Linux 2
+ Ubuntu Server 16\.04
+ Ubuntu Server 18\.04

On other AMIs, and on on\-premises servers and virtual machines for your hybrid environment, you must install the agent manually, as described in the following table\.

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate Updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.


****  

| Operating System Type | Description | 
| --- | --- | 
| Windows |  Windows AMIs published before November 2016 use the EC2Config service to process requests and configure instances\. Unless you have a specific reason for using the EC2Config service or an earlier version of SSM Agent to process Systems Manager requests, we recommend that you download and install the latest version of the SSM Agent to each of your Amazon EC2 instances and managed instances in your hybrid environment\. For more information, see [Installing and Configuring SSM Agent on Windows Instances](sysman-install-ssm-win.md)\.  | 
| Linux | SSM Agent is installed by default on Amazon Linux, Amazon Linux 2, Ubuntu Server 16\.04, and Ubuntu Server 18\.04 LTS base EC2 AMIs\. You must manually install SSM Agent on other versions of Amazon EC2 for Linux, including non\-base images like Amazon ECS\-Optimized AMIs\. For more information, see [Installing and Configuring SSM Agent on Amazon EC2 Linux Instances](sysman-install-ssm-agent.md)\. | 
| On\-premises servers and VMs |  SSM Agent must be installed manually on on\-premises servers and virtual machines \(VMs\) you want to use in a hybrid environment\. The SSM Agent download and installation process for these machines is different than the process used for Amazon EC2 instances\. For more information, see the following topics:[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-prereqs.html)  | 

## Prerequisite: Interface VPC Endpoint or Internet Access<a name="prereqs-vpc-endpoint-or-net-access"></a>

In order for your managed instances and the Systems Manager service to communicate with each other, you must do one of the following:
+ Configure Systems Manager to use an interface Virtual Private Cloud \(VPC\) endpoint
+ Enable outbound internet access on your managed instances

**Note**  
Enabling *inbound* internet access on your managed instances is not required\.

To enhance the security posture of your managed instance, we recommend that you configure Systems Manager to use an interface VPC endpoint\. Interface endpoints are powered by PrivateLink, a technology that enables you to privately access Amazon EC2 and Systems Manager APIs by using private IP addresses\. PrivateLink restricts all network traffic among the Amazon EC2 service, the Systems Manager service, and your managed instances to the Amazon network\. \(Managed instances don't have access to the internet\.\) Also, you don't need an internet gateway, a NAT device, or a virtual private gateway\. For more information, see [\(Optional\) Create a Virtual Private Cloud Endpoint](setup-create-vpc.md)\.

For more information about PrivateLink and VPC endpoints, see [Accessing Services Through AWS PrivateLink](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html#what-is-privatelink) in the *Amazon VPC User Guide*\.

## Prerequisite: TLS Certificates<a name="prereqs-tls-certificate"></a>

Each of your managed instances must have one of the following Transport Layer Security \(TLS\) certificates installed\.
+ Amazon Root CA 1
+ Starfield Services Root Certificate Authority \- G2
+ Starfield Class 2 Certificate Authority

AWS services use these certificates to encrypt calls to other AWS services\. One of these certificates is installed, by default, on all instance created from Amazon Machine Images \(AMIs\)\. On instances created from AMIs that are not supplied by Amazon, and on your own on\-premises servers and VMs, you must install and enable one of these certificates from [Amazon Trust Services](https://www.amazontrust.com/repository/) using AWS Certificate Manager \(ACM\)\.

For information about using Amazon Trust Services certificates or ACM, see the *[AWS Certificate Manager User Guide](https://docs.aws.amazon.com/acm/latest/userguide/)*\.

If certificates in your computing environment are managed by a Group Policy Object \(GPO\), then you might need to configure Group Policy to include one of these certificates\.

For more information about the Amazon Root and Starfield certificates, see the blog post [How to Prepare for AWS’s Move to Its Own Certificate Authority](https://aws.amazon.com/blogs/security/how-to-prepare-for-aws-move-to-its-own-certificate-authority/)\.