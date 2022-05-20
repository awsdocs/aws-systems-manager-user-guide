# Manually install SSM Agent on SUSE Linux Enterprise Server instances<a name="agent-install-sles"></a>

In most cases, the Amazon Machine Images \(AMIs\) for SUSE Linux Enterprise Server \(SLES\) that are provided by AWS come with AWS Systems Manager Agent \(SSM Agent\) preinstalled by default\. For more information, see [Amazon Machine Images \(AMIs\) with SSM Agent preinstalled](ami-preinstalled-agent.md)\.

In the event that SSM Agent isn’t preinstalled on a new SLES instance, or if you need to manually reinstall the agent, use the information on this page to help you\.

**Before you begin**  
Before you install SSM Agent on a SLES instance, note the following:
+ For important information that applies to installation of SSM Agent on all Linux\-based operating systems, see [Manually installing SSM Agent on EC2 instances for Linux](sysman-manual-agent-install.md)\.

**Topics**
+ [Quick installation commands for SSM Agent on SLES](#quick-install-sles)
+ [Create custom agent installation commands for SLES in your Region](#custom-url-sles)

## Quick installation commands for SSM Agent on SLES<a name="quick-install-sles"></a>

Use the following steps to manually install SSM Agent on a single instance\. This procedure uses globally available installation files\. 

**To install SSM Agent on SLES using quick copy and paste commands**

1. Connect to your SLES instance using your preferred method, such as SSH\.

1. **Option 1**: Use a `zypper` command:
   + Run the following command:

     ```
     sudo zypper install amazon-ssm-agent
     ```
   + Enter `y` in response to any prompts\.

   **Option 2**: Use an `rpm` command\.
   + Create a temporary directory on the instance\.

     ```
     mkdir /tmp/ssm
     ```
   + Change to the temporary directory\.

     ```
     cd /tmp/ssm
     ```
   + Run the following commands one at a time to download and run the SSM Agent installer\.
**Note**  
Even though URLs in the following commands include an `ec2-downloads-windows` directory, these are the correct global installation files for SLES\. 

   x86\_64 instances:

   ```
   wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
   ```

   ARM64 instances:

   ```
   wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_arm64/amazon-ssm-agent.rpm
   ```

1. Run the following command\.

   ```
   sudo rpm --install amazon-ssm-agent.rpm
   ```

1. \(Recommended\) Run the following command to verify that the agent is running\.

   ```
   sudo systemctl status amazon-ssm-agent
   ```

   In most cases, the command reports that the agent is running, as shown in the following example\.

   ```
   ● amazon-ssm-agent.service - amazon-ssm-agent
    Loaded: loaded (/usr/lib/systemd/system/amazon-ssm-agent.service; enabled; vendor preset: disabled)
    Active: active (running) since Mon 2022-02-21 23:13:28 UTC; 7s ago
    Main PID: 2102 (amazon-ssm-agen)
    Tasks: 15 (limit: 512)
    CGroup: /system.slice/amazon-ssm-agent.service
    ├─2102 /usr/sbin/amazon-ssm-agent
    └─2107 /usr/sbin/ssm-agent-worker
               --truncated--
   ```

   In rare cases, the command reports that the agent is installed but not running, as shown in the following example\.

   ```
   ● amazon-ssm-agent.service - amazon-ssm-agent
      Loaded: loaded (/usr/lib/systemd/system/amazon-ssm-agent.service; disabled; vendor preset: disabled)
      Active: inactive (dead)
               --truncated--
   ```

   To activate the agent in these cases, run the following commands\.

   ```
   sudo systemctl enable amazon-ssm-agent
   ```

   ```
   sudo systemctl start amazon-ssm-agent
   ```

## Create custom agent installation commands for SLES in your Region<a name="custom-url-sles"></a>

When you install SSM Agent on multiple instances using a script or template, we recommended using installation files that are stored in the AWS Region you're working in\. 

For the following commands, we provide examples that use a publicly accessible Amazon S3 bucket in the US East \(Ohio\) Region \(`us-east-2`\)\. 

**Tip**  
You can also replace a global URL in the procedure [Quick installation commands for SSM Agent on Amazon Linux](agent-install-al.md#quick-install-al) earlier in this topic with a custom Regional URL you construct\.

In the following command, replace *region* with your own information\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

**x86\_64**  

```
wget https://s3.region.amazonaws.com/amazon-ssm-region/latest/linux_amd64/amazon-ssm-agent.rpm
```

```
sudo rpm --install amazon-ssm-agent.rpm
```
See the following example\.  

```
wget https://s3.us-east-2.amazonaws.com/amazon-ssm-us-east-2/latest/linux_amd64/amazon-ssm-agent.rpm
```

```
sudo rpm --install amazon-ssm-agent.rpm
```

**ARM64**  

```
wget https://s3.region.amazonaws.com/amazon-ssm-region/latest/linux_arm64/amazon-ssm-agent.rpm
```

```
sudo rpm --install amazon-ssm-agent.rpm
```
See the following example\.  

```
wget https://s3.us-east-2.amazonaws.com/amazon-ssm-us-east-2/latest/linux_arm64/amazon-ssm-agent.rpm
```

```
sudo rpm --install amazon-ssm-agent.rpm
```