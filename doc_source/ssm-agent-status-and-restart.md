# Checking SSM Agent status and starting the agent<a name="ssm-agent-status-and-restart"></a>

This topic lists the commands to check whether AWS Systems Manager Agent \(SSM Agent\) is running on each supported operating system\. It also provides the commands to start the agent if it isn't running\.


| Operating system | Command to check SSM Agent status | Command to start SSM Agent | 
| --- | --- | --- | 
| Amazon Linux |  `sudo status amazon-ssm-agent`  |  `sudo start amazon-ssm-agent`  | 
| Amazon Linux 2 |  `sudo systemctl status amazon-ssm-agent`  |  `sudo systemctl enable amazon-ssm-agent` `sudo systemctl start amazon-ssm-agent`  | 
| CentOS 6\.x |  `sudo status amazon-ssm-agent`  |  `sudo start amazon-ssm-agent`  | 
| CentOS 7\.x and CentOS 8\.x |  `sudo systemctl status amazon-ssm-agent`  |  `sudo systemctl enable amazon-ssm-agent` `sudo systemctl start amazon-ssm-agent`  | 
| Debian Server 8, 9, and 10 |  `sudo systemctl status amazon-ssm-agent`  |  `sudo systemctl enable amazon-ssm-agent` `sudo systemctl start amazon-ssm-agent`  | 
| macOS | Check the agent log file at /var/log/amazon/ssm/amazon\-ssm\-agent\.log |  `sudo launchctl load -w /Library/LaunchDaemons/com.amazon.aws.ssm.plist` `sudo launchctl start com.amazon.aws.ssm`  | 
| Oracle Linux |  `sudo systemctl status amazon-ssm-agent`  |  `sudo systemctl enable amazon-ssm-agent` `sudo systemctl start amazon-ssm-agent`  | 
| Red Hat Enterprise Linux \(RHEL\) 6\.x |  `sudo status amazon-ssm-agent`  |  `sudo start amazon-ssm-agent`  | 
| Red Hat Enterprise Linux \(RHEL\) 7\.x and 8\.x |  `sudo systemctl status amazon-ssm-agent`  |  `sudo systemctl enable amazon-ssm-agent` `sudo systemctl start amazon-ssm-agent`  | 
| SUSE Linux Enterprise Server \(SLES\) |  `sudo systemctl status amazon-ssm-agent`  |  `sudo systemctl enable amazon-ssm-agent` `sudo systemctl start amazon-ssm-agent`  | 
| Ubuntu Server 14\.04 \(all\) and 16\.04 \(32\-bit\) |  `sudo status amazon-ssm-agent`  |  `sudo start amazon-ssm-agent`  | 
| Ubuntu Server 16\.04 64\-bit instances \(deb package installation\) |  `sudo systemctl status amazon-ssm-agent`  |  `sudo systemctl enable amazon-ssm-agent` `sudo systemctl start amazon-ssm-agent`  | 
| Ubuntu Server 16\.04, 18\.04, and 20\.04 LTS & and 20\.10 STR 64\-bit \(Snap package installation\) |  `sudo systemctl status snap.amazon-ssm-agent.amazon-ssm-agent.service`  |  `sudo snap start amazon-ssm-agent`  | 
| Windows Server |  *Run in PowerShell:* `Get-Service AmazonSSMAgent`  |  *Run in PowerShell Administrator mode:* `Start-Service AmazonSSMAgent`  | 

**Related content**
+ [Installing and configuring SSM Agent on EC2 instances for Linux](sysman-install-ssm-agent.md)
+ [Installing and configuring SSM Agent on EC2 instances for Windows Server](sysman-install-ssm-win.md)
+ [Checking the SSM Agent version number](ssm-agent-get-version.md)