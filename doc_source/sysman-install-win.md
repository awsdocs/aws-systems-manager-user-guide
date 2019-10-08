# Install and Configure SSM Agent on Amazon EC2 Windows Instances<a name="sysman-install-win"></a>

SSM Agent is installed by default on instances created from Windows Server 2016 and Windows Server 2019 Amazon Machine Images \(AMIs\), and on instances created from Windows Server 2003\-2012 R2 AMIs published in November 2016 or later\.

If your instance is a Windows Server 2003\-2012 R2 instance created *before* November 2016, then EC2Config processes Systems Manager requests on your instance\. We recommend that you upgrade your existing instances to use the latest version of EC2Config\. By using the latest EC2Config installer, you install SSM Agent side\-by\-side with EC2Config\. This side\-by\-side version of SSM Agent is compatible with your instances created from earlier Windows AMIs and enables you to use SSM features published after November 2016\. For information about how to install the latest version of the EC2Config service, see [Installing the Latest Version of EC2Config](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/UsingConfig_Install.html) in the *Amazon EC2 User Guide for Windows Instances*\.

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate Updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

If necessary, you can manually download and install the latest version of SSM Agent on your Amazon EC2 Windows instance by using the following procedure\.

**Important**  
This procedure applies to installing or reinstalling SSM Agent on an Amazon EC2 Windows instance\. If you need to install the agent on an on\-premises server or a virtual machine \(VM\) so it can be used with Systems Manager, see [Install SSM Agent for a Hybrid Environment \(Windows\)](sysman-install-managed-win.md)\.

**To manually download and install the latest version of SSM Agent**

1. Log in to your instance by using Remote Desktop or Windows PowerShell\.

1. Download the latest version of SSM Agent to your instance\. You have two options for downloading: 

   **a\. PowerShell**

   Run the following PowerShell command\. This command enables you to download SSM Agent without adjusting Internet Explorer \(IE\) Enhanced Security settings\.

   ```
   Invoke-WebRequest https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/windows_amd64/AmazonSSMAgentSetup.exe -OutFile $env:USERPROFILE\Desktop\SSMAgent_latest.exe
   ```
**Note**  
The URLs in this step let you download SSM Agent from *any* AWS region\. If you want to download the agent from a specific region, use a region\-specific URL instead:  
`https://amazon-ssm-region.s3.amazonaws.com/latest/windows_amd64/AmazonSSMAgentSetup.exe`  
*region* represents the Region identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager Table of Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

   **b\. Download link**

   Download the latest version of SSM Agent to your instance by using the following link\. If you want, update this URL with a region\-specific URL\.

   [https://s3\.amazonaws\.com/ec2\-downloads\-windows/SSMAgent/latest/windows\_amd64/AmazonSSMAgentSetup\.exe](https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/windows_amd64/AmazonSSMAgentSetup.exe)

1. Run the downloaded `AmazonSSMAgentSetup.exe` file to install SSM Agent\.

1. Start or restart SSM Agent by sending the following command in PowerShell: 

   ```
   Restart-Service AmazonSSMAgent
   ```

**Important**  
SSM Agent requires Windows PowerShell 3\.0 or later to run certain SSM Documents on Windows instances \(for example, the **AWS\-ApplyPatchBaseline** document\)\. Verify that your Windows instances are running Windows Management Framework 3\.0 or later\. This framework includes Windows PowerShell\. For more information, see [Windows Management Framework 3\.0](https://www.microsoft.com/en-us/download/details.aspx?id=34595&751be11f-ede8-5a0c-058c-2ee190a24fa6=True)\.