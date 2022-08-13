# Supported operating systems<a name="prereqs-operating-systems"></a>

You can manage Amazon Elastic Compute Cloud \(Amazon EC2\) instances and on\-premises servers and virtual machines \(VMs\), including VMs hosted by other cloud providers, by using Systems Manager\. These nodes must be running one of the following operating systems\.

**Note**  
If you plan to manage and configure AWS IoT Greengrass core devices by using Systems Manager, those devices must meet the requirements for AWS IoT Greengrass\. For more information, see [Setting up AWS IoT Greengrass core devices](https://docs.aws.amazon.com/greengrass/v2/developerguide/setting-up.html) in the *AWS IoT Greengrass Version 2 Developer Guide*\.  
If you plan to manage and configure AWS IoT and non\-AWS edge devices, those devices must meet the requirements listed here and be configured as on\-premises managed nodes for Systems Manager\. For more information, see [Setting up AWS Systems Manager for edge devices](systems-manager-setting-up-edge-devices.md)\.

**Topics**
+ [Linux](#prereqs-os-linux)
+ [macOS](#prereqs-os-mac)
+ [Raspberry Pi OS \(formerly Raspbian\)](#prereqs-os-raspbian)
+ [Windows Server](#prereqs-os-windows-server)

## Linux<a name="prereqs-os-linux"></a>


**Amazon Linux**  

| Versions | x86 | x86\_64 | ARM64 | 
| --- | --- | --- | --- | 
| 2012\.03 – 2018\.03 | ✓ | ✓ |  | 

**Note**  
Beginning with version 2015\.03, Amazon Linux is released in x86\_64 versions\.


**Amazon Linux 2**  

| Versions | x86 | x86\_64 | ARM64 | 
| --- | --- | --- | --- | 
| 2\.0 and all later versions |  | ✓ | ✓ | 


**CentOS**  

| Versions | x86 | x86\_64 | ARM64 | 
| --- | --- | --- | --- | 
| 6\.x¹ | ✓ | ✓ |  | 
| 7\.1 and later 7\.x versions |  | ✓ | ✓ | 
| 8\.0\-8\.5 versions |  | ✓ | ✓ | 

**¹** To use these versions, you must use a 3\.0\.x version of the SSM Agent\. We recommend using the latest available 3\.0\.x version of the SSM Agent\. Later SSM Agent versions \(3\.1 or later\) are not supported\.


**CentOS Stream**  

| Versions | x86 | x86\_64 | ARM64 | 
| --- | --- | --- | --- | 
| 8 |  | ✓ | ✓ | 


**Debian Server**  

| Versions | x86 | x86\_64 | ARM64 | 
| --- | --- | --- | --- | 
| Jessie \(8\) |  | ✓ |  | 
| Stretch \(9\) |  | ✓ | ✓ | 
| Buster \(10\) |  | ✓ | ✓ | 
| Bullseye \(11\) |  | ✓ | ✓ | 


**Oracle Linux**  

| Versions | x86 | x86\_64 | ARM64 | 
| --- | --- | --- | --- | 
| 7\.5\-7\.8 |  | ✓ |  | 
| 8\.1\-8\.5 |  | ✓ |  | 


**Red Hat Enterprise Linux \(RHEL\)**  

| Versions | x86 | x86\_64 | ARM64 | 
| --- | --- | --- | --- | 
| 6\.x¹ | ✓ | ✓ |  | 
| 7\.0\-7\.5 |  | ✓ |  | 
| 7\.6\-8\.5 |  | ✓ | ✓ | 

**¹** To use these versions, you must use a 3\.0\.x version of the SSM Agent\. We recommend using the latest available 3\.0\.x version of the SSM Agent\. Later SSM Agent versions \(3\.1 or later\) are not supported\.


**Rocky Linux**  

| Versions | x86 | x86\_64 | ARM64 | 
| --- | --- | --- | --- | 
| 8\.4\-8\.5 |  | ✓ | ✓ | 


**SUSE Linux Enterprise Server \(SLES\)**  

| Versions | x86 | x86\_64 | ARM64 | 
| --- | --- | --- | --- | 
| 12 and later 12\.x versions |  | ✓ |  | 
| 15 and later 15\.x versions |  | ✓ | ✓ | 


**Ubuntu Server**  

| Versions | x86 | x86\_64 | ARM64 | 
| --- | --- | --- | --- | 
| 12\.04 LTS and 14\.04 LTS | ✓ | ✓ |  | 
| 16\.04 LTS and 18\.04 LTS |  | ✓ | ✓ | 
| 20\.04 LTS and 20\.10 STR |  | ✓ | ✓ | 

## macOS<a name="prereqs-os-mac"></a>


****  

| Version | x86 | x86\_64 | ARM64 | 
| --- | --- | --- | --- | 
| 10\.14\.x \(Mojave\) |  | ✓ |  | 
| 10\.15\.x \(Catalina\) |  | ✓ |  | 
| 11\.x \(BigSur\) |  | ✓ |  | 
| 12\.x \(Monterey\) |  | ✓ |  | 

**Note**  
macOS support is limited to the following AWS Regions:  
US East \(N\. Virginia\) \(us\-east\-1\)
US East \(Ohio\) \(us\-east\-2\)
US West \(Oregon\) \(us\-west\-2\)
Europe \(Ireland\) \(eu\-west\-1\)
Asia Pacific \(Singapore\) \(ap\-southeast\-1\)
For more information about Amazon EC2 support for macOS, see [Amazon EC2 Mac instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-mac-instances.html) in the *Amazon EC2 User Guide for Linux Instances*

## Raspberry Pi OS \(formerly Raspbian\)<a name="prereqs-os-raspbian"></a>


****  

| Version | ARM32 | 
| --- | --- | 
| 8 \(Jessie\) | ✓ | 
| 9 \(Stretch\) | ✓ | 

**More info**  
+ [Manage Raspberry Pi devices using AWS Systems Manager](http://aws.amazon.com/blogs/mt/manage-raspberry-pi-devices-using-aws-systems-manager/)

## Windows Server<a name="prereqs-os-windows-server"></a>

SSM Agent requires Windows PowerShell 3\.0 or later to run certain AWS Systems Manager documents \(SSM documents\) on Windows Server instances \(for example, the legacy `AWS-ApplyPatchBaseline` document\)\. Verify that your Windows Server instances are running Windows Management Framework 3\.0 or later\. This framework includes Windows PowerShell\. For more information, see [https://www.microsoft.com/en-us/download/details.aspx?id=34595&751be11f-ede8-5a0c-058c-2ee190a24fa6=True](https://www.microsoft.com/en-us/download/details.aspx?id=34595&751be11f-ede8-5a0c-058c-2ee190a24fa6=True)\.


****  

| Version | x86 | x86\_64 | ARM64 | 
| --- | --- | --- | --- | 
| 2008¹ | ✓ | ✓ |  | 
| 2008 R2¹ |  | ✓ |  | 
| 2012 and 2012 R2 |  | ✓ |  | 
| 2016 |  | ✓ |  | 
| 2019 |  | ✓ |  | 
| 2022 |  | ✓ |  | 

**¹** As of January 14, 2020, Windows Server 2008 is no longer supported for feature or security updates from Microsoft\. Legacy Amazon Machine Images \(AMIs\) for Windows Server 2008 and 2008 R2 still include version 2 of SSM Agent preinstalled, but Systems Manager no longer officially supports 2008 versions and no longer updates the agent for these versions of Windows Server\. In addition, [SSM Agent version 3\.0](ssm-agent-v3.md) might not be compatible with all operations on Windows Server 2008 and 2008 R2\. The final officially supported version of SSM Agent for Windows Server 2008 versions is 2\.3\.1644\.0\.