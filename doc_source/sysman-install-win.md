# Manually installing SSM Agent on EC2 instances for Windows Server<a name="sysman-install-win"></a>

AWS Systems Manager Agent \(SSM Agent\) is preinstalled, by default, on the following Amazon Machine Images \(AMIs\) for Windows Server:
+ Windows Server 2008\-2012 R2 AMIs published in November 2016 or later
+ Windows Server 2016, 2019, and 2022

If your managed instance is a Windows Server 2008\-2012 R2 instance created *before* November 2016, then EC2Config processes Systems Manager requests on your instance\. We recommend that you upgrade your existing instances to use the latest version of EC2Config\. By using the latest EC2Config installer, you install SSM Agent side\-by\-side with EC2Config\. This side\-by\-side version of SSM Agent is compatible with your instances created from earlier Windows Server AMIs and allows you to use SSM features published after November 2016\. For information about how to install the latest version of the EC2Config service, see [Install the latest version of EC2Config](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/UsingConfig_Install.html) in the *Amazon EC2 User Guide for Windows Instances*\.

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. Failing to use the latest version of the agent can prevent your managed node from using various Systems Manager capabilities and features\. For that reason, we recommend that you automate the process of keeping SSM Agent up to date on your machines\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. Subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub to get notifications about SSM Agent updates\.

If necessary, you can manually download and install the latest version of SSM Agent on your Amazon Elastic Compute Cloud \(Amazon EC2\) instance for Windows Server by using the following procedure\. The commands provided in this procedure can also be passed to Amazon EC2 instances as scripts through user data\.

**Important**  
This procedure applies to installing or reinstalling SSM Agent on an EC2 instance for Windows Server\. If you need to install the agent on an on\-premises server or a virtual machine \(VM\) so it can be used with Systems Manager, see [Install SSM Agent for a hybrid environment \(Windows\)](sysman-install-managed-win.md)\.

**To manually install the latest version of SSM Agent on EC2 instances for Windows Server**

1. Connect to your instance by using Remote Desktop or Windows PowerShell\. For more information, see [Connect to your instance](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/EC2_GetStarted.html#ec2-connect-to-instance-windows) in the *Amazon EC2 User Guide for Windows Instances*\.

1. Download the latest version of SSM Agent to your instance\. You can download using either PowerShell commands or a direct download link\. 
**Note**  
The URLs in this step let you download SSM Agent from *any* AWS Region\. If you want to download the agent from a specific Region, use a Region\-specific URL instead:  
`https://amazon-ssm-region.s3.region.amazonaws.com/latest/windows_amd64/AmazonSSMAgentSetup.exe`  
*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.  
**PowerShell**  
Run the following three PowerShell commands in order\. These commands allow you to download SSM Agent without adjusting Internet Explorer \(IE\) Enhanced Security settings, and then install the agent and remove the installation file\.  

   ```
   $progressPreference = 'silentlyContinue'
   Invoke-WebRequest `
       https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/windows_amd64/AmazonSSMAgentSetup.exe `
       -OutFile $env:USERPROFILE\Desktop\SSMAgent_latest.exe
   ```

   ```
   $progressPreference = 'silentlyContinue'
   Invoke-WebRequest `
       https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/windows_386/AmazonSSMAgentSetup.exe `
       -OutFile $env:USERPROFILE\Desktop\SSMAgent_latest.exe
   ```

   ```
   Start-Process `
       -FilePath $env:USERPROFILE\Desktop\SSMAgent_latest.exe `
       -ArgumentList "/S"
   ```

   ```
   rm -Force $env:USERPROFILE\Desktop\SSMAgent_latest.exe
   ```  
**Direct download**  
Download the latest version of SSM Agent to your instance by using the following link\. If you want, update this URL with an AWS Region\-specific URL\.  
[https://s3\.amazonaws\.com/ec2\-downloads\-windows/SSMAgent/latest/windows\_amd64/AmazonSSMAgentSetup\.exe](https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/windows_amd64/AmazonSSMAgentSetup.exe)  
Run the downloaded `AmazonSSMAgentSetup.exe` file to install SSM Agent\.

1. Start or restart SSM Agent by sending the following command in PowerShell: 

   ```
   Restart-Service AmazonSSMAgent
   ```

**Important**  
SSM Agent requires Windows PowerShell 3\.0 or later to run certain AWS Systems Manager documents \(SSM documents\) on Windows Server instances \(for example, the legacy `AWS-ApplyPatchBaseline` document\)\. Verify that your Windows Server instances are running Windows Management Framework 3\.0 or later\. This framework includes Windows PowerShell\. For more information, see [Windows Management Framework 3\.0](https://www.microsoft.com/en-us/download/details.aspx?id=34595&751be11f-ede8-5a0c-058c-2ee190a24fa6=True)\.

To uninstall the SSM Agent from a Windows instance, open **Control Panel**, **Programs**\. Choose the **Uninstall a program** option\. Open the context \(right\-click\) menu for **Amazon SSM Agent** and choose **Uninstall**\.