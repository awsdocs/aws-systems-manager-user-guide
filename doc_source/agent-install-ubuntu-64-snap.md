# Install SSM Agent on Ubuntu Server 22\.04 LTS, 20\.10 STR & 20\.04, 18\.04, and 16\.04 LTS 64\-bit \(Snap\)<a name="agent-install-ubuntu-64-snap"></a>

**Before you begin**  
Before you install SSM Agent on an Ubuntu Server 22\.04 LTS, 20\.10 STR & 20\.04, 18\.04, and 16\.04 LTS 64\-bit \(Snap\), note the following: 

Version 16\.04 installation by Snaps or deb installers  
On Ubuntu Server 16\.04, SSM Agent is installed using either Snaps or deb installation packages, depending on the version of the 16\.04 AMI\.

SSM Agent installer files locations  
On Ubuntu Server22\.04 LTS, 20\.10 STR & 20\.04, 18\.04, and 16\.04 LTS \(with Snap\), SSM Agent installer files, including agent binaries and config files, are stored in the following directory: `/snap/amazon-ssm-agent/current/`\. If you make changes to any configuration files in this directory, then you must copy these files from the `/snap` directory to the `/etc/amazon/ssm/` directory\. Log and library files haven't changed \(`/var/lib/amazon/ssm`, `/var/log/amazon/ssm`\)\.

Using the Snap `candidate` channel  
The *candidate* channel in the Snap store contains the latest version of SSM Agent \(including all of the latest bug fixes\); not the stable channel\. To to learn more about the differences between the candidate and stable channels, see **Risk\-levels** at [https://snapcraft\.io/docs/channels](https://snapcraft.io/docs/channels)\.  
If you want to track SSM Agent version information on the candidate channel, run the following command on your Ubuntu Server 20\.10 STR & 20\.04, 18\.04, and 16\.04 LTS 64\-bit instances\.  

```
sudo snap switch --channel=candidate amazon-ssm-agent
```

Snaps recommended on versions 18\.04 and later  
On Ubuntu Server 22\.04 LTS, 20\.10 STR & 20\.04 and 18\.04 LTS, we recommend you only use Snaps\. Also verify that only one instance of the agent is installed and running on your instances\. If you want to use SSM Agent without Snaps, uninstall the SSM Agent\. Then [install the SSM Agent as a debian package](agent-install-ubuntu-64-deb.md) using the instructions for installing SSM Agent on Ubuntu Server 16\.04 and 14\.04 64\-bit \(deb\)\. Before installing, ensure you don't have any Snaps installed that overlap with the list of packages you want managed as debian packages\.

`Maximum timeout exceeded` error message  
Because of a known issue with Snap, you might see a `Maximum timeout exceeded` error with `snap` commands\. If you get this error, run the following commands one at a time to start the agent, stop it, and check its status:   

```
sudo systemctl start snap.amazon-ssm-agent.amazon-ssm-agent.service
```

```
sudo systemctl stop snap.amazon-ssm-agent.amazon-ssm-agent.service
```

```
sudo systemctl status snap.amazon-ssm-agent.amazon-ssm-agent.service
```

**To install SSM Agent on Ubuntu Server 22\.04 LTS, 20\.10 STR & 20\.04, 18\.04, and 16\.04 LTS 64\-bit instances \(with Snap package\)**

1. SSM Agent is installed, by default, on Ubuntu Server 22\.04 LTS, 20\.04, 18\.04, and 16\.04 LTS 64\-bit AMIs with an identifier of `20180627` or later\.

   You can use the following script if you need to install SSM Agent on an on\-premises server or if you need to reinstall the agent\. You don't need to specify a URL for the download, because the `snap` command automatically downloads the agent from the [Snap app store](https://snapcraft.io/amazon-ssm-agent) at [https://snapcraft\.io](https://snapcraft.io)\.

   ```
   sudo snap install amazon-ssm-agent --classic
   ```

1. Run the following command to determine if SSM Agent is running\. 

   ```
   sudo snap list amazon-ssm-agent
   ```

1. Run the following command to start the service if the previous command returned amazon\-ssm\-agent is stopped, inactive, or disabled\.

   ```
   sudo snap start amazon-ssm-agent
   ```

1. Check the status of the agent\.

   ```
   sudo snap services amazon-ssm-agent
   ```