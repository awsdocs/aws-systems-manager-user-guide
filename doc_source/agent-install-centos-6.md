# Install SSM Agent on CentOS 6\.x<a name="agent-install-centos-6"></a>

The Amazon Machine Images \(AMIs\) for CentOS 6 that are provided by AWS do not come with AWS Systems Manager Agent \(SSM Agent\) preinstalled by default\. Use the information on this page to help you install or reinstall the agent on CentOS 6 instances\.

**Topics**
+ [Quick installation commands for SSM Agent on CentOS 6](#quick-install-centos-6)
+ [Create custom agent installation commands for CentOS 6 in your Region](#custom-url-centos-6)

## Quick installation commands for SSM Agent on CentOS 6<a name="quick-install-centos-6"></a>

Use the following steps to manually install SSM Agent on a single instance\. This procedure uses globally available installation files\. 

**To install SSM Agent on CentOS 6\.x**

1. Connect to your CentOS 6 instance using your preferred method, such as SSH\. 

1. Copy the command for your instance’s architecture and run it on the instance\.
**Note**  
Even though URLs in the following commands include an `ec2-downloads-windows` directory, these are the correct global installation files for CentOS 6\.   
The following commands specify the version directory `3.0.1479.0` instead of a `latest` directory\. This is because SSM Agent version 3\.1 and later are not supported for CentOS 6\.  
x86\_64 instances  

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/3.0.1479.0/linux_amd64/amazon-ssm-agent.rpm
   ```  
x86 instances  

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/3.0.1479.0/linux_386/amazon-ssm-agent.rpm
   ```

1. \(Recommended\) Run the following command to verify that the agent is running\.

   ```
   sudo status amazon-ssm-agent
   ```

   In most cases, the command reports that the agent is running, as shown in the following example\.

   ```
   amazon-ssm-agent start/running, process 1744
   ```

   In rare cases, the command reports that the agent is installed but not running, as shown in the following example\.

   ```
   amazon-ssm-agent stop/waiting
   ```

   To activate the agent in these cases, run the following command\.

   ```
   sudo start amazon-ssm-agent
   ```

## Create custom agent installation commands for CentOS 6 in your Region<a name="custom-url-centos-6"></a>

When you install SSM Agent on multiple instances using a script or template, we recommended using installation files that are stored in the AWS Region you're working in\. 

For the following commands, we provide examples that use a publicly accessible S3 bucket in the US East \(Ohio\) Region \(`us-east-2`\)\. 

**Tip**  
You can also replace a global URL in the procedure [Quick installation commands for SSM Agent on CentOS 6](#quick-install-centos-6) earlier in this topic with a custom Regional URL you construct\.

In the following command, replace *region* with your own information\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

**Note**  
The following commands specify the version directory `3.0.1390.0` instead of a `latest` directory\. This is because SSM Agent version 3\.1 and later are not supported for CentOS 6\.

x86\_64  

```
sudo yum install -y https://s3.region.amazonaws.com/amazon-ssm-region/3.0.1479.0/linux_amd64/amazon-ssm-agent.rpm
```
See the following example\.  

```
sudo yum install -y https://s3.us-east-2.amazonaws.com/amazon-ssm-us-east-2/3.0.1479.0/linux_amd64/amazon-ssm-agent.rpm
```

x86  

```
sudo yum install -y https://s3.region.amazonaws.com/amazon-ssm-region/3.0.1479.0/linux_386/amazon-ssm-agent.rpm
```
See the following example\.  

```
sudo yum install -y https://s3.us-east-2.amazonaws.com/amazon-ssm-us-east-2/3.0.1479.0/linux_386/amazon-ssm-agent.rpm
```