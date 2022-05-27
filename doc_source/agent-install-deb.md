# Manually installing SSM Agent on Debian Server instances<a name="agent-install-deb"></a>

The Amazon Machine Images \(AMIs\) for Debian Server that are provided by AWS do not come with AWS Systems Manager Agent \(SSM Agent\) preinstalled by default\. For a list of AWS managed AMIs on which the agent might be preinstalled, see [Amazon Machine Images \(AMIs\) with SSM Agent preinstalled](ami-preinstalled-agent.md)\.

Use the information in this section to help you manually install or reinstall SSM Agent on a Debian Server instance\.

**Before you begin**  
Before you install SSM Agent on a Debian Server instance, note the following:
+ For important information that applies to installation of SSM Agent on all Linux\-based operating systems, see [Manually installing SSM Agent on EC2 instances for Linux](sysman-manual-agent-install.md)\.

**Topics**
+ [Quick installation commands for SSM Agent on Debian Server](#quick-install-debian)
+ [Create custom agent installation commands for Debian Server in your Region](#custom-url-debian)

## Quick installation commands for SSM Agent on Debian Server<a name="quick-install-debian"></a>

Use the following steps to manually install SSM Agent on a single instance\. This procedure uses globally available installation files\. 

**To install SSM Agent on Debian Server**

1. Connect to your Debian Server instance using your preferred method, such as SSH\. 

1. Run the following command to create a temporary directory on the instance\.

   ```
   mkdir /tmp/ssm
   ```

1. Run the following command to change to the temporary directory\.

   ```
   cd /tmp/ssm
   ```

1. Copy the command for your instance’s architecture and run it on the instance\.
**Note**  
Even though URLs in the following commands include an `ec2-downloads-windows` directory, these are the correct global installation files for Debian Server\.   
For Debian Server 8, only the x86\_64 architecture is supported\.  
x86\_64 instances  

   ```
   wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb
   ```  
ARM64 instances  

   ```
   wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_arm64/amazon-ssm-agent.deb
   ```

1. Run the following command\.

   ```
   sudo dpkg -i amazon-ssm-agent.deb
   ```

1. \(Recommended\) Run the following command to verify that the agent is running\.

   ```
   sudo systemctl status amazon-ssm-agent
   ```

   In most cases, the command reports that the agent is running, as shown in the following example\.

   ```
   ● amazon-ssm-agent.service - amazon-ssm-agent
      Loaded: loaded (/lib/systemd/system/amazon-ssm-agent.service; enabled; vendor
      Active: active (running) since Tue 2022-04-19 16:25:03 UTC; 4s ago
    Main PID: 628 (amazon-ssm-agen)
      CGroup: /system.slice/amazon-ssm-agent.service
              ├─628 /usr/bin/amazon-ssm-agent
              └─650 /usr/bin/ssm-agent-worker
               --truncated--
   ```

   In rare cases, the command reports that the agent is installed but not running, as shown in the following example\.

   ```
   ● amazon-ssm-agent.service - amazon-ssm-agent
      Loaded: loaded (/lib/systemd/system/amazon-ssm-agent.service; enabled; vendor
      Active: inactive (dead) since Tue 2022-04-19 16:26:30 UTC; 5s ago
    Main PID: 628 (code=exited, status=0/SUCCESS)
               --truncated--
   ```

   To activate the agent in these cases, run the following commands\.

   ```
   sudo systemctl enable amazon-ssm-agent
   ```

   ```
   sudo systemctl start amazon-ssm-agent
   ```

## Create custom agent installation commands for Debian Server in your Region<a name="custom-url-debian"></a>

When you install SSM Agent on multiple instances using a script or template, we recommended using installation files that are stored in the AWS Region you're working in\. 

For the following commands, we provide examples that use a publicly accessible Amazon S3 bucket in the US East \(Ohio\) Region \(`us-east-2`\)\. 

**Tip**  
You can also replace a global URL in the procedure [Quick installation commands for SSM Agent on Debian Server](#quick-install-debian) earlier in this topic with a custom Regional URL you construct\.

In the following command, replace *region* with your own information\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

**Note**  
For Debian Server 8, only the x86\_64 architecture is supported\.

x86\_64  

```
wget https://s3.region.amazonaws.com/amazon-ssm-region/latest/debian_amd64/amazon-ssm-agent.deb
```

```
sudo dpkg -i amazon-ssm-agent.deb
```
See the following example\.  

```
wget https://s3.us-east-2.amazonaws.com/amazon-ssm-us-east-2/latest/debian_amd64/amazon-ssm-agent.deb
```

```
sudo dpkg -i amazon-ssm-agent.deb
```

ARM64  

```
sudo yum install -y https://s3.region.amazonaws.com/amazon-ssm-region/latest/debian_arm64/amazon-ssm-agent.deb
```

```
sudo dpkg -i amazon-ssm-agent.deb
```
See the following example\.  

```
sudo yum install -y https://s3.us-east-2.amazonaws.com/amazon-ssm-us-east-2/latest/debian_arm64/amazon-ssm-agent.deb
```

```
sudo dpkg -i amazon-ssm-agent.deb
```