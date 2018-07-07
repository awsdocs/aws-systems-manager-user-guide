# Install SSM Agent on Servers and VMs in a Linux Hybrid Environment<a name="sysman-install-managed-linux"></a>

Before you begin, locate the Activation Code and Activation ID that were sent to you after you completed the managed\-instance activation\. You will specify the Code and ID in the following procedure\.

**Important**  
This procedure is for servers and VMs in an on\-premises or hybrid environment\. To download and install SSM Agent on an Amazon EC2 Linux instance, see [Installing and Configuring SSM Agent on Linux Instances](sysman-install-ssm-agent.md)\.

The URLs in the following scripts let you download SSM Agent from *any* AWS region\. If you want to download the agent from a *specific* region, copy the URL for your operating system, and then replace *region* with an appropriate value\.

*region* represents the Region identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager table of regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

For example, to download SSM Agent for Amazon Linux, RHEL, CentOS, and SLES 64\-bit from the US West \(N\. California\) Region \(us\-west\-1\), use the following URL:

```
https://s3-us-west-1.amazonaws.com/amazon-ssm-us-west-1/latest/linux_amd64/amazon-ssm-agent.rpm 
```

If the download fails, try replacing https://s3\-*region* with https://s3\.*region*\.
+ **Amazon Linux, RHEL, CentOS, and SLES 64\-bit**

   https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/linux\_amd64/amazon\-ssm\-agent\.rpm 
+ **Amazon Linux, RHEL, and CentOS 32\-bit**

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/linux\_386/amazon\-ssm\-agent\.rpm
+ **Ubuntu Server 64\-bit**

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_amd64/amazon\-ssm\-agent\.deb
+ **Ubuntu Server 32\-bit**

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_386/amazon\-ssm\-agent\.deb
+ **Raspbian**

  https://s3\-*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_arm/amazon\-ssm\-agent\.deb

**To install SSM Agent on servers and VMs in your hybrid environment**

1. Log on to a server or VM in your hybrid environment\.

1. Copy and paste one of the following command blocks into SSH\. Replace the placeholder values with the Activation Code and Activation ID generated when you create a managed\-instance activation, and with the identifier of the AWS Region you want to download SSM Agent from\. 

    Note that `sudo` is not necessary if you are a root user\.

   *region* represents the Region identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager table of regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

   **On Amazon Linux, RHEL 6\.x, and CentOS 6\.x**

   ```
   mkdir /tmp/ssm
   curl https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm -o /tmp/ssm/amazon-ssm-agent.rpm
   sudo yum install -y /tmp/ssm/amazon-ssm-agent.rpm
   sudo stop amazon-ssm-agent
   sudo amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region"
   sudo start amazon-ssm-agent
   ```

   **On RHEL 7\.x and CentOS 7\.x**

   ```
   mkdir /tmp/ssm
   curl https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm -o /tmp/ssm/amazon-ssm-agent.rpm
   sudo yum install -y /tmp/ssm/amazon-ssm-agent.rpm
   sudo systemctl stop amazon-ssm-agent
   sudo amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region"
   sudo systemctl start amazon-ssm-agent
   ```

   **On SLES**

   ```
   mkdir /tmp/ssm
   sudo wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
   sudo rpm --install amazon-ssm-agent.rpm
   sudo systemctl stop amazon-ssm-agent
   sudo amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region"
   sudo systemctl enable amazon-ssm-agent
   sudo systemctl start amazon-ssm-agent
   ```

   **On Ubuntu**

   ```
   mkdir /tmp/ssm
   curl https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb -o /tmp/ssm/amazon-ssm-agent.deb
   sudo dpkg -i /tmp/ssm/amazon-ssm-agent.deb
   sudo service amazon-ssm-agent stop
   sudo amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region" 
   sudo service amazon-ssm-agent start
   ```

   **On Raspbian**

   ```
   mkdir /tmp/ssm
   sudo curl https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_arm/amazon-ssm-agent.deb -o /tmp/ssm/amazon-ssm-agent.deb
   sudo dpkg -i /tmp/ssm/amazon-ssm-agent.deb
   sudo service amazon-ssm-agent stop
   sudo amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region" 
   sudo service amazon-ssm-agent start
   ```
**Note**  
If you see the following error in the SSM Agent error logs, then the machine ID did not persist after a reboot:  
`Unable to load instance associations, unable to retrieve associations unable to retrieve associations error occurred in RequestManagedInstanceRoleToken: MachineFingerprintDoesNotMatch: Fingerprint does not match`  
Execute the following command to make the machine ID persist after a reboot\.  

   ```
   umount /etc/machine-id
   systemd-machine-id-setup
   ```

1. Press Enter\.

The command downloads and installs SSM Agent onto the server or VM in your hybrid environment\. The command stops SSM Agent, and then registers the server or VM with the SSM service\. The server or VM is now a managed instance\. Amazon EC2 instances configured for Systems Manager are also managed instances\. In the Amazon EC2 console, however, your on\-premises instances are distinguished from Amazon EC2 instances with the prefix "mi\-"\.