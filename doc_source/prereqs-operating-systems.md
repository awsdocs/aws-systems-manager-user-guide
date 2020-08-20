# Supported operating systems<a name="prereqs-operating-systems"></a>

Your EC2 instances, on\-premises servers, and virtual machines \(VMs\) must be running one of the following operating systems in order to be used with AWS Systems Manager\.

**Topics**
+ [Linux](#prereqs-os-linux)
+ [Raspbian](#prereqs-os-raspbian)
+ [Windows Server](#prereqs-os-windows-server)

## Linux<a name="prereqs-os-linux"></a>


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
| 20\.04 LTS |  | ✓ | ✓ | 


**Debian Server**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| Jessie \(8\) |  | ✓ |  | 
| Stretch \(9\) |  | ✓ |  | 
| Buster \(10\) |  | ✓ |  | 


**Red Hat Enterprise Linux \(RHEL\)**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 6\.0 |  | ✓ |  | 
| 6\.5 | ✓ | ✓ |  | 
| 6\.9 | ✓ | ✓ |  | 
| 7\.0 |  | ✓ |  | 
| 7\.1 |  | ✓ |  | 
| 7\.2 |  | ✓ |  | 
| 7\.3 |  | ✓ |  | 
| 7\.4 |  | ✓ |  | 
| 7\.5 |  | ✓ |  | 
| 7\.6 |  | ✓ | ✓ | 
| 7\.7 |  | ✓ | ✓ | 
| 7\.8 |  | ✓ | ✓ | 
| 8\.0 |  | ✓ | ✓ | 
| 8\.1 |  | ✓ | ✓ | 
| 8\.2 |  | ✓ | ✓ | 


**Oracle Linux**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 7\.5 |  | ✓ |  | 
| 7\.6 |  | ✓ |  | 
| 7\.7 |  | ✓ |  | 
| 7\.8 |  | ✓ |  | 


**CentOS**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 6\.0 | ✓ |  |  | 
| 6\.3 and later 6\.x versions | ✓ | ✓ |  | 
| 7\.1 and later 7\.x versions |  | ✓ | ✓ | 
| 8\.0 and 8\.1 versions |  | ✓ |  | 


**SUSE Linux Enterprise Server \(SLES\)**  

| Versions | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 12 and later 12\.x versions |  | ✓ |  | 
| 15 and later 15\.x versions |  | ✓ |  | 

## Raspbian<a name="prereqs-os-raspbian"></a>


****  

| Version | ARM 32\-bit \(arm\) | 
| --- | --- | 
| Jessie | ✓ | 
| Stretch | ✓ | 

## Windows Server<a name="prereqs-os-windows-server"></a>


****  

| Version | Intel 32\-bit \(x86\) | Intel 64\-bit \(x86\_64\) | ARM 64\-bit \(arm64\) | 
| --- | --- | --- | --- | 
| 2008 | ✓ | ✓ |  | 
| 2008 R2 |  | ✓ |  | 
| 2012 and 2012 R2 |  | ✓ |  | 
| 2016 |  | ✓ |  | 
| 2019 |  | ✓ |  | 

**Important**  
SSM Agent requires Windows PowerShell 3\.0 or later to run certain SSM Documents on Windows Server instances \(for example, the legacy **AWS\-ApplyPatchBaseline** document\)\. Verify that your Windows Server instances are running Windows Management Framework 3\.0 or later\. This framework includes Windows PowerShell\. For more information, see [Windows Management Framework 3\.0](https://www.microsoft.com/en-us/download/details.aspx?id=34595&751be11f-ede8-5a0c-058c-2ee190a24fa6=True)\.