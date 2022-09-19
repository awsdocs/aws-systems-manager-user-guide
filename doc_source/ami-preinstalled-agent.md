# Amazon Machine Images \(AMIs\) with SSM Agent preinstalled<a name="ami-preinstalled-agent"></a>

AWS Systems Manager Agent \(SSM Agent\) is preinstalled on some Amazon Machine Images \(AMIs\) provided by AWS\.

In most cases, SSM Agent is preinstalled on AMIs provided by AWS for the following operating systems \(OSs\):
+ Amazon Linux Base AMIs dated 2017\.09 and later
+ Amazon Linux 2
+ Amazon Linux 2 ECS\-Optimized Base AMIs
+ Amazon EKS\-Optimized Amazon Linux AMIs
+ macOS 10\.14\.x \(Mojave\), 10\.15\.x \(Catalina\), and 11\.x \(Big Sur\)
+ SUSE Linux Enterprise Server \(SLES\) 12 and 15
+ Ubuntu Server 16\.04, 18\.04, and 20\.04  
+ Windows Server 2008\-2012 R2 AMIs published in November 2016 or later
+ Windows Server 2016, 2019, and 2022

In rare cases, the agent might not be preinstalled, or it might be installed but not running on an Amazon Elastic Compute Cloud \(Amazon EC2\) instance created from one of the AWS managed AMIs for these OSs\. Therefore, before you use Systems Manager with instances created from any of these AMIs, we recommend connecting to one of the instances to verify that SSM Agent is installed and running\.

**Note**  
Instances launched from AMIs that are not included in the preceding list might not have SSM Agent preinstalled\. You can use the following procedure to verify whether the agent is installed on your instance\. If you find that the agent is not installed, you can manually install the agent on [Linux, ](sysman-manual-agent-install.md) [macOS](sysman-manual-agent-install-macos2.md), and [Windows Server](sysman-install-win.md) instances\. You must also manually install SSM Agent on [virtual machines](systems-manager-managedinstances.md) in your hybrid environment or on\-premises servers, and on your [edge devices](systems-manager-setting-up-edge-devices.md)\.  
SSM Agent might be preinstalled on AMIs found in AWS Marketplace or Community AMIs; however, AWS doesn't support these AMIs\.

**To verify installation of SSM Agent on an instance**

1. After waiting a few minutes after the instance is launched, connect to it by using your preferred method, such as SSH for Linux or Remote Desktop for Windows Server\.

1. Run the command for the operating system type of your instance to check SSM Agent status\.  
**Amazon Linux**  
`sudo systemctl status amazon-ssm-agent`  
**Amazon Linux 2**  
`sudo systemctl status amazon-ssm-agent`  
**macOS**  
Check for an agent log file at `/var/log/amazon/ssm/amazon-ssm-agent.log`\.  
**SUSE Linux Enterprise Server**  
`sudo systemctl status amazon-ssm-agent`  
**Ubuntu Server 16\.04 \(32\-bit\)**  
`sudo status amazon-ssm-agent`  
**Ubuntu Server 16\.04 64\-bit instances \(deb package installation\)**  
`sudo systemctl status amazon-ssm-agent`  
**Ubuntu Server 16\.04, 18\.04, and 20\.04 LTS & and 20\.10 STR 64\-bit \(Snap package installation\)**  
`sudo systemctl status snap.amazon-ssm-agent.amazon-ssm-agent.service`  
**Windows Server**  
*Run in PowerShell:*  
`Get-Service AmazonSSMAgent`
**Tip**  
To view commands for checking agent status on all operating system types supported by Systems Manager, see [Checking SSM Agent status and starting the agent](ssm-agent-status-and-restart.md)\.

1. Check the results of the command to determine the status of the agent\.

**Result 1: Installed and running**  
In most cases, the command reports that the agent is running\.

   See the following Amazon Linux 2 example\.

   ```
   amazon-ssm-agent.service - amazon-ssm-agent
      Loaded: loaded (/usr/lib/systemd/system/amazon-ssm-agent.service; enabled; vendor preset: enabled)
      Active: active (running) since Wed 2021-10-20 19:09:29 UTC; 4min 6s ago
               --truncated--
   ```

   See the following Windows Server example\.

   ```
   Status   Name               DisplayName
   ------   ----               -----------
   Running  AmazonSSMAgent     Amazon SSM Agent
   ```

**Result 2: Installed but not running**  
In rare cases, the command reports that the agent is installed but not running\.

   See the following Amazon Linux 2 example\.

   ```
   amazon-ssm-agent.service - amazon-ssm-agent
      Loaded: loaded (/usr/lib/systemd/system/amazon-ssm-agent.service; enabled; vendor preset: enabled)
      Active: inactive (dead) since Wed 2021-10-20 22:16:41 UTC; 18s ago
               --truncated--
   ```

   See the following Windows Server example\.

   ```
   Status   Name               DisplayName
   ------   ----               -----------
   Stopped  AmazonSSMAgent     Amazon SSM Agent
   ```

   If the agent is installed but not running, activate it manually using the command for your operating system type\.  
**Amazon Linux**  
`sudo systemctl start amazon-ssm-agent`  
**Amazon Linux 2**  
`sudo systemctl enable amazon-ssm-agent`  
`sudo systemctl start amazon-ssm-agent`  
**macOS**  
`sudo launchctl load -w /Library/LaunchDaemons/com.amazon.aws.ssm.plist`  
`sudo launchctl start com.amazon.aws.ssm`  
**SUSE Linux Enterprise Server**  
`sudo systemctl enable amazon-ssm-agent`  
`sudo systemctl start amazon-ssm-agent`  
**Ubuntu Server 16\.04 \(32\-bit\)**  
`sudo start amazon-ssm-agent`  
**Ubuntu Server 16\.04 64\-bit instances \(deb package installation\)**  
`sudo systemctl enable amazon-ssm-agent`  
`sudo systemctl start amazon-ssm-agent`  
**Ubuntu Server 16\.04, 18\.04, and 20\.04 LTS & and 20\.10 STR 64\-bit \(Snap package installation\)**  
`sudo snap start amazon-ssm-agent`  
**Windows Server**  
*Run in PowerShell:*  
`Start-Service AmazonSSMAgent`

**Result 3: Not installed**  
In rare cases, the command reports that the agent isn't installed\.

   See the following Amazon Linux 2 example\.

   ```
   Unit amazon-ssm-agent.service could not be found.
   ```

   See the following Windows Server example\.

   ```
   Get-Service : Cannot find any service with service name 'AmazonSSMAgent'.
   --truncated--
   ```

   If the agent isn't installed, install it manually by using the procedure for your operating system type:
   + [Manually installing SSM Agent on EC2 instances for Linux](sysman-manual-agent-install.md)
   + [Manually installing SSM Agent on EC2 instances for macOS](sysman-manual-agent-install-macos2.md)
   + [Manually installing SSM Agent on EC2 instances for Windows Server](sysman-install-win.md)