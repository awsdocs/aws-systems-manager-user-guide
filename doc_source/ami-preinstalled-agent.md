# Amazon Machine Images \(AMIs\) with SSM Agent preinstalled<a name="ami-preinstalled-agent"></a>

AWS Systems Manager Agent \(SSM Agent\) is preinstalled on some Amazon Machine Images \(AMIs\) provided by AWS\.

For example, when you launch an Amazon Elastic Compute Cloud \(Amazon EC2\) instance created from an AMI with one of the following operating systems, you'll likely find that the SSM Agent is already installed:
+ Amazon Linux Base AMIs dated 2017\.09 and later
+ Amazon Linux 2
+ Amazon Linux 2 ECS\-Optimized Base AMIs
+ Amazon EKS\-Optimized Amazon Linux AMIs
+ macOS 10\.14\.x \(Mojave\), 10\.15\.x \(Catalina\), and 11\.x \(Big Sur\)
+ SUSE Linux Enterprise Server \(SLES\) 12 and 15
+ Ubuntu Server 16\.04, 18\.04, and 20\.04  
+ Windows Server 2008\-2012 R2 AMIs published in November 2016 or later
+ Windows Server 2016, 2019, and 2022

Although it's unlikely, it's possible that instances created from AMIs on the preceding list might not have the agent installed, or have the agent installed but not running\. Therefore, we recommend that each time you create an instance from an AWS managed AMI, you use the following procedure\. By doing so, you can verify that SSM Agent is installed and running before you try to use Systems Manager\.

If you find that the SSM Agent is not installed, you can manually install it on [Linux, ](sysman-manual-agent-install.md) [macOS](sysman-manual-agent-install-macos2.md), and [Windows Server](sysman-install-win.md) instances\. 

**Note**  
SSM Agent might be preinstalled on AMIs found in AWS Marketplace or in the Community AMIs repository, but AWS doesn’t support these AMIs\.  
SSM Agent might also be preinstalled on AWS managed AMIs with operating systems \(OSs\) that aren’t on the preceding list\. This typically indicates that the OS isn't supported by all components or capabilities of Systems Manager\. In this case, although it’s expected that Systems Manager will fully support the OS in the future, support isn't guaranteed\.

**To verify installation of SSM Agent on an instance**

1. After launching a new instance, wait a few minutes for it to initialize\.

1. Connect to the instance using your preferred method\. For example, you can use SSH to connect to Linux instances or use Remote Desktop to connect to Windows Server instances\.

1. Check the status of SSM Agent by running the command for your instance's operating system type\.

------
#### [ Amazon Linux ]

   `sudo systemctl status amazon-ssm-agent`

------
#### [ Amazon Linux 2 ]

   `sudo systemctl status amazon-ssm-agent`

------
#### [ macOS ]

   There is no command to check SSM Agent status on macOS\. You can check the status by locating and evaluating the agent log file `/var/log/amazon/ssm/amazon-ssm-agent.log`\.

------
#### [ SUSE Linux Enterprise Server ]

   `sudo systemctl status amazon-ssm-agent`

------
#### [ Ubuntu Server \(32\-bit\) ]

   `sudo status amazon-ssm-agent`

------
#### [ Ubuntu Server \(64\-bit \- Deb\) ]

   `sudo systemctl status amazon-ssm-agent`

------
#### [ Ubuntu Server \(64\-bit \- Snap\) ]

   `sudo systemctl status snap.amazon-ssm-agent.amazon-ssm-agent.service`

------
#### [ Windows Server ]

   Run the following command in PowerShell\.

   `Get-Service AmazonSSMAgent`

------
**Tip**  
To view the commands for checking SSM Agent status on all operating system types supported by Systems Manager, see [Checking SSM Agent status and starting the agent](ssm-agent-status-and-restart.md)\.

1. Evaluate the command output to learn the status of the SSM Agent\.

**Status: *Installed and running***  
In most cases, the command output indicates that the agent is installed and running\.

   The following example shows that SSM Agent is installed and running on an Amazon Linux 2 instance\.

   ```
   amazon-ssm-agent.service - amazon-ssm-agent
   Loaded: loaded (/usr/lib/systemd/system/amazon-ssm-agent.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2021-10-20 19:09:29 UTC; 4min 6s ago
   --truncated--
   ```

   The following example shows that SSM Agent is installed and running on a Windows Server instance\.

   ```
   Status   Name               DisplayName
   ------   ----               -----------
   Running  AmazonSSMAgent     Amazon SSM Agent
   ```

**Status: *Installed but not running***  
In some cases, the command output indicates that the agent is installed but not running\.

   The following example shows that SSM Agent is installed but not running on an Amazon Linux 2 instance\.

   ```
   amazon-ssm-agent.service - amazon-ssm-agent
   Loaded: loaded (/usr/lib/systemd/system/amazon-ssm-agent.service; enabled; vendor preset: enabled)
   Active: inactive (dead) since Wed 2021-10-20 22:16:41 UTC; 18s ago
   --truncated--
   ```

   The following example shows that SSM Agent is installed but not running on a Windows Server instance\.

   ```
   Status   Name               DisplayName
   ------   ----               -----------
   Stopped  AmazonSSMAgent     Amazon SSM Agent
   ```

   If the agent is installed but not running, you can activate it manually using the commands for your instance's operating system type\.

------
#### [ Amazon Linux ]

   `sudo systemctl start amazon-ssm-agent`

------
#### [ Amazon Linux 2 ]

   `sudo systemctl enable amazon-ssm-agent`

   `sudo systemctl start amazon-ssm-agent`

------
#### [ macOS ]

   `sudo launchctl load -w /Library/LaunchDaemons/com.amazon.aws.ssm.plist`

   `sudo launchctl start com.amazon.aws.ssm`

------
#### [ SUSE Linux Enterprise Server ]

   `sudo systemctl enable amazon-ssm-agent`

   `sudo systemctl start amazon-ssm-agent`

------
#### [ Ubuntu Server \(32\-bit\) ]

   `sudo start amazon-ssm-agent`

------
#### [ Ubuntu Server \(64\-bit \- Deb\) ]

   `sudo systemctl enable amazon-ssm-agent`

   `sudo systemctl start amazon-ssm-agent`

------
#### [ Ubuntu Server \(64\-bit \- Snap\) ]

   `sudo snap start amazon-ssm-agent`

------
#### [ Windows Server ]

   Run the following command in PowerShell\.

   `Start-Service AmazonSSMAgent`

------

**Status: *Not installed***  
In some cases, the command output indicates that the agent is not installed\.

   The following example shows that SSM Agent is not installed on an Amazon Linux 2 instance\.

   ```
   Unit amazon-ssm-agent.service could not be found.
   ```

   The following example shows that SSM Agent is not installed on a Windows Server instance\.

   ```
   Get-Service : Cannot find any service with service name 'AmazonSSMAgent'.
   --truncated--
   ```

   If the agent isn't installed, you can install it manually using the procedure for your operating system type:
   + [Manually installing SSM Agent on EC2 instances for Linux](sysman-manual-agent-install.md)
   + [Manually installing SSM Agent on EC2 instances for macOS](sysman-manual-agent-install-macos2.md)
   + [Manually installing SSM Agent on EC2 instances for Windows Server](sysman-install-win.md)