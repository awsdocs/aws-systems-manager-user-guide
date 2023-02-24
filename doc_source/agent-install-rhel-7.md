# Install SSM Agent on RHEL 7\.x<a name="agent-install-rhel-7"></a>

The Amazon Machine Images \(AMIs\) for RHEL 7 that are provided by AWS do not come with AWS Systems Manager Agent \(SSM Agent\) preinstalled by default\. Use the information on this page to help you install or reinstall the agent on RHEL 7 instances\.

**Topics**
+ [Quick installation commands for SSM Agent on RHEL 7](#quick-install-rhel-7)
+ [Create custom agent installation commands for RHEL 7 in your Region](#custom-url-rhel-7)

## Quick installation commands for SSM Agent on RHEL 7<a name="quick-install-rhel-7"></a>

Use the following steps to manually install SSM Agent on a single instance\. This procedure uses globally available installation files\. 

**To install SSM Agent on RHEL 7\.x**

1. Connect to your RHEL 7 instance using your preferred method, such as SSH\. 

1. Copy the command for your instance’s architecture and run it on the instance\.
**Note**  
Even though URLs in the following commands include an `ec2-downloads-windows` directory, these are the correct global installation files for RHEL 7\.   
x86\_64 instances  

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
   ```  
ARM64 instances  

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_arm64/amazon-ssm-agent.rpm
   ```

1. \(Recommended\) Run the following command to verify that the agent is running\.

   ```
   sudo systemctl status amazon-ssm-agent
   ```

   In most cases, the command reports that the agent is running, as shown in the following example\.

   ```
   ● amazon-ssm-agent.service - amazon-ssm-agent
      Loaded: loaded (/etc/systemd/system/amazon-ssm-agent.service; enabled; vendor preset: disabled)
      Active: active (running) since Tue 2022-04-19 16:47:36 UTC; 22s ago
    Main PID: 1342 (amazon-ssm-agen)
      CGroup: /system.slice/amazon-ssm-agent.service
              ├─1342 /usr/bin/amazon-ssm-agent
              └─1362 /usr/bin/ssm-agent-worker
               --truncated--
   ```

   In rare cases, the command reports that the agent is installed but not running, as shown in the following example\.

   ```
   ● amazon-ssm-agent.service - amazon-ssm-agent
      Loaded: loaded (/etc/systemd/system/amazon-ssm-agent.service; enabled; vendor preset: disabled)
      Active: inactive (dead) since Tue 2022-04-19 16:48:56 UTC; 5s ago
     Process: 1342 ExecStart=/usr/bin/amazon-ssm-agent (code=exited, status=0/SUCCESS)
    Main PID: 1342 (code=exited, status=0/SUCCESS)
               --truncated--
   ```

   To activate the agent in these cases, run the following commands\.

   ```
   sudo systemctl enable amazon-ssm-agent
   ```

   ```
   sudo systemctl start amazon-ssm-agent
   ```

## Create custom agent installation commands for RHEL 7 in your Region<a name="custom-url-rhel-7"></a>

When you install SSM Agent on multiple instances using a script or template, we recommended using installation files that are stored in the AWS Region you're working in\. 

For the following commands, we provide examples that use a publicly accessible S3 bucket in the US East \(Ohio\) Region \(`us-east-2`\)\. 

**Tip**  
You can also replace a global URL in the procedure [Quick installation commands for SSM Agent on RHEL 7](#quick-install-rhel-7) earlier in this topic with a custom Regional URL you construct\.

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