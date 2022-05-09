# Manually installing SSM Agent on Amazon Linux 2 instances<a name="agent-install-al2"></a>

**Important**  
This topic provides commands for working with SSM Agent on **Amazon Linux 2** instances\. Some of these commands aren't supported on Amazon Linux instances\. Before continuing, ensure you're viewing the correct topic for your instance type\. For commands to run on Amazon Linux instances, see [Manually installing SSM Agent on Amazon Linux instances](agent-install-al.md)\.

In most cases, the Amazon Machine Images \(AMIs\) for Amazon Linux 2 that are provided by AWS come with AWS Systems Manager Agent \(SSM Agent\) preinstalled by default\. For more information, see [Amazon Machine Images \(AMIs\) with SSM Agent preinstalled](ami-preinstalled-agent.md)\.

In the event that SSM Agent isn’t preinstalled on a new Amazon Linux 2 instance, or if you need to manually reinstall the agent, use the information on this page to help you\.

**Before you begin**  
Before you install SSM Agent on an Amazon Linux 2 instance, note the following:
+ For important information that applies to installation of SSM Agent on all Linux\-based operating systems, see [Manually installing SSM Agent on EC2 instances for Linux](sysman-manual-agent-install.md)\.
+ If you use a `yum` command to update SSM Agent on a managed node after the agent has been installed or updated using the SSM document `AWS-UpdateSSMAgent`, you might see the following message: "Warning: RPMDB altered outside of yum\." This message is expected and can be safely ignored\.

**Topics**
+ [Quick installation commands for SSM Agent on Amazon Linux 2](#quick-install-al2)
+ [Create custom agent installation commands for Amazon Linux 2 in your Region](#custom-url-al2)

## Quick installation commands for SSM Agent on Amazon Linux 2<a name="quick-install-al2"></a>

Use the following steps to manually install SSM Agent on a single instance\. This procedure uses globally available installation files\. 

**To install SSM Agent on Amazon Linux 2 using quick copy and paste commands**

1. Connect to your Amazon Linux 2 instance using your preferred method, such as SSH\.

1. Copy the command for your instance’s architecture and run it on the instance\.
**Note**  
Even though URLs in the following commands include an `ec2-downloads-windows` directory, these are the correct global installation files for Amazon Linux 2\.   
**x86\_64**  

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
   ```  
**ARM64**  

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_arm64/amazon-ssm-agent.rpm
   ```

1. \(Recommended\) Run the following command to verify that the agent is running\.

   ```
   sudo systemctl status amazon-ssm-agent
   ```

   In most cases, the command reports that the agent is running, as shown in the following example\.

   ```
   amazon-ssm-agent.service - amazon-ssm-agent
   Loaded: loaded (/usr/lib/systemd/system/amazon-ssm-agent.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2021-10-20 19:09:29 UTC; 4min 6s ago
               --truncated--
   ```

   In rare cases, the command reports that the agent is installed but not running, as shown in the following example\.

   ```
   amazon-ssm-agent.service - amazon-ssm-agent
   Loaded: loaded (/usr/lib/systemd/system/amazon-ssm-agent.service; enabled; vendor preset: enabled)
   Active: inactive (dead) since Wed 2021-10-20 22:16:41 UTC; 18s ago
               --truncated--
   ```

   To activate the agent in these cases, run the following command\.

   ```
   sudo systemctl start amazon-ssm-agent
   ```

## Create custom agent installation commands for Amazon Linux 2 in your Region<a name="custom-url-al2"></a>

When you install SSM Agent on multiple instances using a script or template, we recommended using installation files that are stored in the AWS Region you're working in\. 

For the following commands, we provide examples that use a publicly accessible Amazon S3 bucket in the US East \(Ohio\) Region \(`us-east-2`\)\. 

**Tip**  
You can also replace a global URL in the procedure [Quick installation commands for SSM Agent on Amazon Linux](agent-install-al.md#quick-install-al) earlier in this topic with a custom Regional URL you construct\.

*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

**x86\_64**  

```
sudo yum install -y https://s3.region.amazonaws.com/amazon-ssm-region/latest/linux_amd64/amazon-ssm-agent.rpm
```
See the following example\.  

```
sudo yum install -y https://s3.us-east-2.amazonaws.com/amazon-ssm-us-east-2/latest/linux_amd64/amazon-ssm-agent.rpm
```

**ARM64**  

```
sudo yum install -y https://s3.region.amazonaws.com/amazon-ssm-region/latest/linux_arm64/amazon-ssm-agent.rpm
```
See the following example\.  

```
sudo yum install -y https://s3.us-east-2.amazonaws.com/amazon-ssm-us-east-2/latest/linux_arm64/amazon-ssm-agent.rpm
```