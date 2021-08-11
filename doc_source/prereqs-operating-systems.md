# Supported operating systems<a name="prereqs-operating-systems"></a>

Your Amazon Elastic Compute Cloud \(Amazon EC2\) instances, on\-premises servers, and virtual machines \(VMs\) must be running one of the following operating systems to use them with AWS Systems Manager\.

**Topics**
+ [Linux](#prereqs-os-linux)
+ [macOS](#prereqs-os-mac)
+ [Raspbian](#prereqs-os-raspbian)
+ [Windows Server](#prereqs-os-windows-server)

## Linux<a name="prereqs-os-linux"></a>


**Amazon Linux**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 2012\.03 – 2018\.03 | ✓ | ✓ |  | 

**Note**  
Beginning with version 2015\.03, Amazon Linux is released in Intel 64\-bit \(x86\_64\) versions\.


**Amazon Linux 2**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 2\.0 and all later versions |  | ✓ | ✓ | 


**CentOS**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 6\.x¹ | ✓ | ✓ |  | 
| 7\.1 and later 7\.x versions |  | ✓ | ✓ | 
| 8\.0, 8\.1, and 8\.2 versions |  | ✓ | ✓ | 

**¹** SSM Agent no longer officially supports these versions and no longer updates the agent for these versions of CentOS\. SSM Agent version 3\.0\.1390\.0 and earlier is supported for CentOS 6\.


**Debian Server**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| Jessie \(8\) |  | ✓ |  | 
| Stretch \(9\) |  | ✓ | ✓ | 
| Buster \(10\) |  | ✓ | ✓ | 


**Oracle Linux**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 7\.5 |  | ✓ |  | 
| 7\.6 |  | ✓ |  | 
| 7\.7 |  | ✓ |  | 
| 7\.8 |  | ✓ |  | 
| 8\.1 |  | ✓ |  | 
| 8\.2 |  | ✓ |  | 
| 8\.3 |  | ✓ |  | 


**Red Hat Enterprise Linux \(RHEL\)**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 6\.x¹ | ✓ | ✓ |  | 
| 7\.0 |  | ✓ |  | 
| 7\.1 |  | ✓ |  | 
| 7\.2 |  | ✓ |  | 
| 7\.3 |  | ✓ |  | 
| 7\.4 |  | ✓ |  | 
| 7\.5 |  | ✓ |  | 
| 7\.6 |  | ✓ | ✓ | 
| 7\.7 |  | ✓ | ✓ | 
| 7\.8 |  | ✓ | ✓ | 
| 7\.9 |  | ✓ | ✓ | 
| 8\.0 |  | ✓ | ✓ | 
| 8\.1 |  | ✓ | ✓ | 
| 8\.2 |  | ✓ | ✓ | 
| 8\.3 |  | ✓ | ✓ | 

**¹** SSM Agent no longer officially supports these versions and no longer updates the agent for these versions of RHEL\. SSM Agent version 3\.0\.1390\.0 and earlier is supported for RHEL 6\.


**SUSE Linux Enterprise Server \(SLES\)**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 12 and later 12\.x versions |  | ✓ |  | 
| 15 and later 15\.x versions |  | ✓ |  | 


**Ubuntu Server**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 12\.04 LTS and 14\.04 LTS | ✓ | ✓ |  | 
| 16\.04 LTS and 18\.04 LTS |  | ✓ | ✓ | 
| 20\.04 LTS and 20\.10 STR |  | ✓ | ✓ | 

## macOS<a name="prereqs-os-mac"></a>


****  

| Version | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 10\.14\.x \(Mojave\) |  | ✓ |  | 
| 10\.15\.x \(Catalina\) |  | ✓ |  | 
| 11\.x \(BigSur\) |  | ✓ |  | 

**Note**  
macOS support is limited to the following AWS Regions:  
US East \(N\. Virginia\) \(us\-east\-1\)
US East \(Ohio\) \(us\-east\-2\)
US West \(Oregon\) \(us\-west\-2\)
Europe \(Ireland\) \(eu\-west\-1\)
Asia Pacific \(Singapore\) \(ap\-southeast\-1\)
For more information about Amazon EC2 support for macOS, see [Amazon EC2 Mac instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-mac-instances.html) in the *Amazon EC2 User Guide for Linux Instances*

## Raspbian<a name="prereqs-os-raspbian"></a>


****  

| Version | ARM 32\-bit \(arm\) | 
| --- | --- | 
| Jessie | ✓ | 
| Stretch | ✓ | 

**Related content**

[Manage Raspberry Pi devices using AWS Systems Manager](http://aws.amazon.com/blogs/mt/manage-raspberry-pi-devices-using-aws-systems-manager/)

## Windows Server<a name="prereqs-os-windows-server"></a>

SSM Agent requires Windows PowerShell 3\.0 or later to run certain AWS Systems Manager documents \(SSM documents\) on Windows Server instances \(for example, the legacy `AWS-ApplyPatchBaseline` document\)\. Verify that your Windows Server instances are running Windows Management Framework 3\.0 or later\. This framework includes Windows PowerShell\. For more information, see [https://www.microsoft.com/en-us/download/details.aspx?id=34595&751be11f-ede8-5a0c-058c-2ee190a24fa6=True](https://www.microsoft.com/en-us/download/details.aspx?id=34595&751be11f-ede8-5a0c-058c-2ee190a24fa6=True)\.


****  

| Version | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 2008¹ | ✓ | ✓ |  | 
| 2008 R2¹ |  | ✓ |  | 
| 2012 and 2012 R2 |  | ✓ |  | 
| 2016 |  | ✓ |  | 
| 2019 |  | ✓ |  | 

**Important**  
The SSM Agent requires Windows PowerShell 3\.0 or later to run certain SSM documents on Windows Server instances \(for example, the legacy `AWS-ApplyPatchBaseline` document\)\. Verify that your Windows Server instances are running Windows Management Framework 3\.0 or later\. This framework includes Windows PowerShell\. For more information, see [https://www.microsoft.com/en-us/download/details.aspx?id=34595&751be11f-ede8-5a0c-058c-2ee190a24fa6=True](https://www.microsoft.com/en-us/download/details.aspx?id=34595&751be11f-ede8-5a0c-058c-2ee190a24fa6=True)\.

**¹** As of January 14, 2020, Windows Server 2008 is [no longer supported](https://www.microsoft.com/en-us/cloud-platform/windows-server-2008) for feature or security updates from Microsoft\. Legacy Amazon Machine Images \(AMIs\) for Windows Server 2008 and 2008 R2 still include version 2 of SSM Agent preinstalled, but Systems Manager no longer officially supports 2008 versions and no longer updates the agent for these versions of Windows Server\. In addition, [SSM Agent version 3\.0](ssm-agent-v3.md) might not be compatible with all operations on Windows Server 2008 and 2008 R2\. The final officially supported version of SSM Agent for Windows Server 2008 versions is 2\.3\.1644\.0\.