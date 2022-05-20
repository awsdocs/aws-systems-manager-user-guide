# Manually install SSM Agent on Rocky Linux instances<a name="agent-install-rocky"></a>

The Amazon Machine Images \(AMIs\) for Rocky Linux that are provided by AWS do not come with AWS Systems Manager Agent \(SSM Agent\) preinstalled by default\. For a list of AWS managed AMIs on which the agent might be preinstalled, see [Amazon Machine Images \(AMIs\) with SSM Agent preinstalled](ami-preinstalled-agent.md)\.

Use the information in this section to help you manually install or reinstall SSM Agent on an Rocky Linux instance\.

**Before you begin**  
Before you install SSM Agent on a Rocky Linux instance, note the following:
+ For important information that applies to installation of SSM Agent on all Linux\-based operating systems, see [Manually installing SSM Agent on EC2 instances for Linux](sysman-manual-agent-install.md)\.

**Topics**
+ [Quick installation commands for SSM Agent on Rocky Linux](#quick-install-rocky)
+ [Create custom agent installation commands for Rocky Linux in your Region](#custom-url-rocky)

## Quick installation commands for SSM Agent on Rocky Linux<a name="quick-install-rocky"></a>

Use the following steps to manually install SSM Agent on a single instance\. This procedure uses globally available installation files\. 

**Before you begin**  
Before you install SSM Agent on a Rocky Linux instance, note the following:
+ Ensure that either Python 2 or Python 3 is installed on your Rocky Linux instance\. This is required in order for SSM Agent to work properly\.

**To install SSM Agent on Rocky Linux**

1. Connect to your Rocky Linux instance using your preferred method, such as SSH\. 

1. Copy the command for your instance’s architecture and run it on the instance\.
**Note**  
Even though URLs in the following commands include an `ec2-downloads-windows` directory, these are the correct global installation files for Rocky Linux\.   
x86\_64 instances  

   ```
   sudo dnf install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
   ```  
ARM64 instances  

   ```
   sudo dnf install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_arm64/amazon-ssm-agent.rpm
   ```

1. \(Recommended\) Run the following command to verify that the agent is running\.

   ```
   sudo systemctl status amazon-ssm-agent
   ```

   In most cases, the command reports that the agent is running, as shown in the following example\.

   ```
   ● amazon-ssm-agent.service - amazon-ssm-agent
      Loaded: loaded (/etc/systemd/system/amazon-ssm-agent.service; enabled; vendo>
      Active: active (running) since Tue 2022-04-19 16:40:41 UTC; 9s ago
    Main PID: 4898 (amazon-ssm-agen)
       Tasks: 14 (limit: 4821)
      Memory: 34.6M
      CGroup: /system.slice/amazon-ssm-agent.service
              ├─4898 /usr/bin/amazon-ssm-agent
              └─4954 /usr/bin/ssm-agent-worker
               --truncated--
   ```

   In rare cases, the command reports that the agent is installed but not running, as shown in the following example\.

   ```
   ● amazon-ssm-agent.service - amazon-ssm-agent
      Loaded: loaded (/etc/systemd/system/amazon-ssm-agent.service; enabled; vendo>
      Active: inactive (dead) since Tue 2022-04-19 16:42:05 UTC; 2s ago
               --truncated--
   ```

   To activate the agent in these cases, run the following commands\.

   ```
   sudo systemctl enable amazon-ssm-agent
   ```

   ```
   sudo systemctl start amazon-ssm-agent
   ```

## Create custom agent installation commands for Rocky Linux in your Region<a name="custom-url-rocky"></a>

When you install SSM Agent on multiple instances using a script or template, we recommended using installation files that are stored in the AWS Region you're working in\. 

For the following commands, we provide examples that use a publicly accessible Amazon S3 bucket in the US East \(Ohio\) Region \(`us-east-2`\)\. 

**Tip**  
You can also replace a global URL in the procedure [](#quick-install-rocky) earlier in this topic with a custom Regional URL you construct\.

In the following command, replace *region* with your own information\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

**x86\_64**  

```
sudo dnf install -y https://s3.region.amazonaws.com/amazon-ssm-region/latest/linux_amd64/amazon-ssm-agent.rpm
```
See the following example\.  

```
sudo dnf install -y https://s3.us-east-2.amazonaws.com/amazon-ssm-us-east-2/latest/linux_amd64/amazon-ssm-agent.rpm
```

**ARM64**  

```
sudo dnf install -y https://s3.region.amazonaws.com/amazon-ssm-region/latest/linux_arm64/amazon-ssm-agent.rpm
```
See the following example\.  

```
sudo dnf install -y https://s3.us-east-2.amazonaws.com/amazon-ssm-us-east-2/latest/linux_arm64/amazon-ssm-agent.rpm
```