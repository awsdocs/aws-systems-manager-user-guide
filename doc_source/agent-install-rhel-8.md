# Install SSM Agent on RHEL 8\.x<a name="agent-install-rhel-8"></a>

The Amazon Machine Images \(AMIs\) for RHEL 8 that are provided by AWS do not come with AWS Systems Manager Agent \(SSM Agent\) preinstalled by default\. Use the information on this page to help you install or reinstall the agent on RHEL 8 instances\.

**Before you begin**  
Before you install SSM Agent on a RHEL 8 instance, note the following:
+ Ensure that either Python 2 or Python 3 is installed on your RHEL 8 instance\. This is required in order for SSM Agent to work properly\.

**Topics**
+ [Quick installation commands for SSM Agent on RHEL 8](#quick-install-rhel-8)
+ [Create custom agent installation commands for RHEL 8 in your Region](#custom-url-rhel-8)

## Quick installation commands for SSM Agent on RHEL 8<a name="quick-install-rhel-8"></a>

Use the following steps to manually install SSM Agent on a single instance\. This procedure uses globally available installation files\. 

**To install SSM Agent on RHEL 8\.x**

1. Connect to your RHEL 8 instance using your preferred method, such as SSH\. 

1. Copy the command for your instance’s architecture and run it on the instance\.
**Note**  
Even though URLs in the following commands include an `ec2-downloads-windows` directory, these are the correct global installation files for RHEL 8\.   
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

## Create custom agent installation commands for RHEL 8 in your Region<a name="custom-url-rhel-8"></a>

When you install SSM Agent on multiple instances using a script or template, we recommended using installation files that are stored in the AWS Region you're working in\. 

For the following commands, we provide examples that use a publicly accessible Amazon S3 bucket in the US East \(Ohio\) Region \(`us-east-2`\)\. 

**Tip**  
You can also replace a global URL in the procedure [Quick installation commands for SSM Agent on RHEL 8](#quick-install-rhel-8) earlier in this topic with a custom Regional URL you construct\.

In the following command, replace *region* with your own information\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

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