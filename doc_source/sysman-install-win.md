# Install and Configure SSM Agent on Windows Instances<a name="sysman-install-win"></a>

SSM Agent is installed by default on Windows Server 2016 instances\. It is also installed by default on instances created from Windows Server 2003\-2012 R2 AMIs published in November 2016 or later\. You don't need to install SSM Agent on these instances\. If you need to update SSM Agent, we recommend that you use State Manager to automatically update SSM Agent on your instances when new versions become available\. For more information, see [Walkthrough: Automatically Update SSM Agent \(CLI\)](sysman-state-cli.md)\.

If your instance is a Windows Server 2003\-2012 R2 instance created *before* November 2016, then EC2Config processes Systems Manager requests on your instance\. We recommend that you upgrade your existing instances to use the latest version of EC2Config\. By using the latest EC2Config installer, you install SSM Agent side\-by\-side with EC2Config\. This side\-by\-side version of SSM Agent is compatible with your instances created from earlier Windows AMIs and enables you to use SSM features published after November 2016\. For information about how to install the latest version of the EC2Config service, see [Installing the Latest Version of EC2Config](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/UsingConfig_Install.html) in the *Amazon EC2 User Guide for Windows Instances*\.

If necessary, you can manually download and install the latest version of SSM Agent by using the following procedure\.

**To manually download and install the latest version of SSM Agent**

1. Log in to your instance by using, for example, Remote Desktop or Windows PowerShell\.

1. Download the latest version of SSM Agent to your instance:

   [https://s3\.amazonaws\.com/ec2\-downloads\-windows/SSMAgent/latest/windows\_amd64/AmazonSSMAgentSetup\.exe](https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/windows_amd64/AmazonSSMAgentSetup.exe)

   This URL lets you download SSM Agent from any AWS region\. If you want to download the agent from a specific region, use a region\-specific URL instead:

   `https://amazon-ssm-region.s3.amazonaws.com/latest/windows_amd64/AmazonSSMAgentSetup.exe`

   *region* represents the Region identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager table of regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

1. Start or restart SSM Agent \(AmazonSSMAgent\.exe\) using the Windows Services Control Panel or by sending the following command in PowerShell: 

   ```
   Restart-Service AmazonSSMAgent
   ```

**Important**  
SSM Agent requires Windows PowerShell 3\.0 or later to run certain SSM Documents on Windows instances \(for example, the **AWS\-ApplyPatchBaseline** document\)\. Verify that your Windows instances are running Windows Management Framework 3\.0 or later\. This framework includes Windows PowerShell\. For more information, see [Windows Management Framework 3\.0](https://www.microsoft.com/en-us/download/details.aspx?id=34595&751be11f-ede8-5a0c-058c-2ee190a24fa6=True)\.