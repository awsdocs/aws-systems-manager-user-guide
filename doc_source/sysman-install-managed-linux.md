# Step 5: Install SSM Agent for a hybrid environment \(Linux\)<a name="sysman-install-managed-linux"></a>

This topic describes how to install SSM Agent on Linux machines in a hybrid environment\. If you plan to use Windows Server machines in a hybrid environment, see the next step, [Step 6: Install SSM Agent for a hybrid environment \(Windows\)](sysman-install-managed-win.md)\.

**Important**  
This procedure is for servers and virtual machines \(VMs\) in an on\-premises or hybrid environment\. To download and install SSM Agent on an EC2 instance for Linux, see [Installing and configuring SSM Agent on EC2 instances for Linux](sysman-install-ssm-agent.md)\.

Before you begin, locate the Activation Code and Activation ID that were sent to you after you completed the managed\-instance activation earlier in [Step 4: Create a managed\-instance activation for a hybrid environment](sysman-managed-instance-activation.md)\. You specify the Code and ID in the following procedure\.

The URLs in the following scripts let you download SSM Agent from *any* AWS region\. If you want to download the agent from a *specific* region, copy the URL for your operating system, and then replace *region* with an appropriate value\.

*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

For example, to download SSM Agent for Amazon Linux, RHEL, CentOS, and SLES 64\-bit from the US West \(N\. California\) Region \(us\-west\-1\), use the following URL:

```
https://s3.us-west-1.amazonaws.com/amazon-ssm-us-west-1/latest/linux_amd64/amazon-ssm-agent.rpm
```

------
#### [ Amazon Linux 2, Amazon Linux, RHEL, Oracle Linux, CentOS, and SLES ]
+ **Intel 64\-bit \(x86\_64\)**

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/linux\_amd64/amazon\-ssm\-agent\.rpm 
+ **Intel 32\-bit \(x86\)**

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/linux\_386/amazon\-ssm\-agent\.rpm
+ **ARM 64\-bit \(arm64\)**

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/linux\_arm64/amazon\-ssm\-agent\.rpm

------
#### [ Ubuntu Server ]
+ **Intel 64\-bit \(x86\_64\)**

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_amd64/amazon\-ssm\-agent\.deb
+ **Intel 32\-bit \(x86\)**

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_386/amazon\-ssm\-agent\.deb

------
#### [ Debian Server ]
+ **Intel 64\-bit \(x86\_64\)**

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_amd64/amazon\-ssm\-agent\.deb

------
#### [ Raspbian ]
+ https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_arm/amazon\-ssm\-agent\.deb

------

**To install SSM Agent on servers and VMs in your hybrid environment**

1. Log on to a server or VM in your hybrid environment\.

1. If you use an HTTP or HTTPS proxy, you must set the `http_proxy` or `https_proxy` environment variables in the current shell session\. If you aren't using a proxy, you can skip this step\.

   For an HTTP proxy server, enter the following commands at the command line:

   ```
   export http_proxy=http://hostname:port
   export https_proxy=http://hostname:port
   ```

   For an HTTPS proxy server, enter the following commands at the command line:

   ```
   export http_proxy=http://hostname:port
   export https_proxy=https://hostname:port
   ```

1. Copy and paste one of the following command blocks into SSH\. Replace the placeholder values with the Activation Code and Activation ID generated when you create a managed\-instance activation, and with the identifier of the AWS Region you want to download SSM Agent from, then press Enter\.
**Note**  
Note the following important details:  
`sudo` is not necessary if you are a root user\.
Each command block specifies `sudo -E amazon-ssm-agent`\. The `-E` is only necessary if you set an HTTP or HTTPS proxy environment variable\.

   *region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

## Amazon Linux, RHEL 6\.x, and CentOS 6\.x<a name="cent-6"></a>

```
mkdir /tmp/ssm
curl https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm -o /tmp/ssm/amazon-ssm-agent.rpm
sudo yum install -y /tmp/ssm/amazon-ssm-agent.rpm
sudo stop amazon-ssm-agent
sudo -E amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region"
sudo start amazon-ssm-agent
```

## Amazon Linux 2, RHEL 7\.x, Oracle Linux, and CentOS 7\.x<a name="cent-7"></a>

```
mkdir /tmp/ssm
curl https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm -o /tmp/ssm/amazon-ssm-agent.rpm
sudo yum install -y /tmp/ssm/amazon-ssm-agent.rpm
sudo systemctl stop amazon-ssm-agent
sudo -E amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region"
sudo systemctl start amazon-ssm-agent
```

## RHEL 8\.x and CentOS 8\.x<a name="cent-8"></a>

```
mkdir /tmp/ssm
curl https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm -o /tmp/ssm/amazon-ssm-agent.rpm
sudo dnf install -y /tmp/ssm/amazon-ssm-agent.rpm
sudo systemctl stop amazon-ssm-agent
sudo -E amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region"
sudo systemctl start amazon-ssm-agent
```

## Debian Server<a name="deb"></a>

```
mkdir /tmp/ssm
wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb -O /tmp/ssm/amazon-ssm-agent.deb
sudo dpkg -i /tmp/ssm/amazon-ssm-agent.deb
sudo service amazon-ssm-agent stop
sudo -E amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region" 
sudo service amazon-ssm-agent start
```

## Raspbian<a name="rasp"></a>

```
mkdir /tmp/ssm
sudo curl https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_arm/amazon-ssm-agent.deb -o /tmp/ssm/amazon-ssm-agent.deb
sudo dpkg -i /tmp/ssm/amazon-ssm-agent.deb
sudo service amazon-ssm-agent stop
sudo -E amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region" 
sudo service amazon-ssm-agent start
```

## SLES<a name="suse"></a>

```
mkdir /tmp/ssm
sudo wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
sudo rpm --install amazon-ssm-agent.rpm
sudo systemctl stop amazon-ssm-agent
sudo -E amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region"
sudo systemctl enable amazon-ssm-agent
sudo systemctl start amazon-ssm-agent
```

## Ubuntu Server<a name="ubu"></a>
+ **Using \.deb packages**

  ```
  mkdir /tmp/ssm
  curl https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb -o /tmp/ssm/amazon-ssm-agent.deb
  sudo dpkg -i /tmp/ssm/amazon-ssm-agent.deb
  sudo service amazon-ssm-agent stop
  sudo -E amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region" 
  sudo service amazon-ssm-agent start
  ```
+ **Using Snap packages**

  You don't need to specify a URL for the download, because the `snap` command automatically downloads the agent from the [Snap app store](https://snapcraft.io/amazon-ssm-agent) at [https://snapcraft\.io](https://snapcraft.io)\.

  ```
  sudo snap install amazon-ssm-agent --classic
  sudo systemctl stop snap.amazon-ssm-agent.amazon-ssm-agent.service
  sudo /snap/amazon-ssm-agent/current/amazon-ssm-agent -register -code "activation-code" -id "activation-id" -region "region" 
  sudo systemctl start snap.amazon-ssm-agent.amazon-ssm-agent.service
  ```
**Important**  
The *candidate* channel in the Snap store contains the latest version of SSM Agent; not the stable channel\. If you want to track SSM Agent version information on the candidate channel, run the following command on your Ubuntu Server 18\.04 and 16\.04 LTS 64\-bit instances\.  

  ```
  sudo snap switch --channel=candidate amazon-ssm-agent
  ```

The command downloads and installs SSM Agent onto the server or VM in your hybrid environment\. The command stops SSM Agent, and then registers the server or VM with the SSM service\. The server or VM is now a managed instance\. EC2 instances configured for Systems Manager are also managed instances\. In the Systems Manager console, however, your on\-premises instances are distinguished from EC2 instances with the prefix "mi\-"\.

Continue to [Step 6: Install SSM Agent for a hybrid environment \(Windows\)](sysman-install-managed-win.md)\.

## Deregister and reregister a managed instance<a name="systems-manager-install-managed-linux-deregister-reregister"></a>

You can deregister a managed instance by calling the [DeregisterManagedInstance](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeregisterManagedInstance.html) API action from either the AWS CLI or Tools for Windows PowerShell\. Here's an example CLI command:

```
aws ssm deregister-managed-instance --instance-id "mi-1234567890"
```

You can reregister a managed instance after you deregistered it\. Use the following procedure to reregister a managed instance\. After you complete the procedure, your managed instance reappears in the list of managed instances\.

**To reregister a managed instance on a Linux hybrid machine**

1. Connect to your instance\.

1. Search for and run `amazon-ssm-agent`\.

1. Choose the option to uninstall SSM Agent\.

1. Repeat the procedure in this topic to install SSM Agent\. 

## Troubleshooting SSM Agent installation in a Linux hybrid environment<a name="systems-manager-install-managed-linux-troubleshooting"></a>

Use the following information to help you troubleshoot problems installing SSM Agent in a Linux hybrid environment\.

### You receive DeliveryTimedOut error<a name="systems-manager-install-managed-linux-troubleshooting-delivery-timed-out"></a>

**Problem**: While configuring an Amazon EC2 instance in one AWS account as a managed instance for a separate AWS account, you receive `DeliveryTimedOut` after running the commands to install SSM Agent on the target instance\.

**Solution**: `DeliveryTimedOut` is the expected response code for this scenario\. The command to install SSM Agent on the target instance changes the instance ID of the source instance\. Because the instance ID has changed, the source instance is not able to reply to the target instance that the command failed, completed, or timed out while executing\.

### Unable to load instance associations<a name="systems-manager-install-managed-linux-troubleshooting-associations"></a>

**Problem**: After running the install commands, you see the following error in the SSM Agent error logs:

`Unable to load instance associations, unable to retrieve associations unable to retrieve associations error occurred in RequestManagedInstanceRoleToken: MachineFingerprintDoesNotMatch: Fingerprint does not match`

You see this error when the machine ID doesn't persist after a reboot\.

**Solution**: To solve this problem, run the following command\. This command forces the machine ID to persist after a reboot\.

```
umount /etc/machine-id
systemd-machine-id-setup
```