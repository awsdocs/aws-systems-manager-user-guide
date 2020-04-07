# Manually Install SSM Agent on Amazon EC2 Linux Instances<a name="sysman-manual-agent-install"></a>

Use one of the following scripts to install SSM Agent on one of the following Linux instances\.
+ [Amazon Linux and Amazon Linux 2](#agent-install-al)
+ [Ubuntu Server](#agent-install-ubuntu)
+ [Debian Server](#agent-install-deb)
+ [Red Hat Enterprise Linux \(RHEL\)](#agent-install-rhel)
+ [Oracle Linux](#agent-install-oracle)
+ [CentOS](#agent-install-centos)
+ [SUSE Linux Enterprise Server \(SLES\) 12](#agent-install-sles)
+ [Raspbian](#agent-install-raspbianjessie)

The URLs in the following scripts let you download SSM Agent from *any* AWS region\. If you want to download the agent from a specific region, see [Download SSM Agent from a Specific Region](#sysman-install-ssm-agent-specific)\.

After you manually install SSM Agent, you can automatically update SSM Agent on your instances when new versions become available by using Systems Manager State Manager\. For more information, see [Automatically Update SSM Agent \(CLI\)](sysman-state-cli.md)\.

**Important**  
These procedures apply to installing or reinstalling SSM Agent on Amazon EC2 Linux instances\. If you need to install the agent on an on\-premises server or a virtual machine \(VM\) so it can be used with Systems Manager, see [Install SSM Agent for a Hybrid Environment \(Linux\)](sysman-install-managed-linux.md)\.

## Amazon Linux and Amazon Linux 2<a name="agent-install-al"></a>

Connect to your Amazon Linux or Amazon Linux 2 instance and perform the following steps to install SSM Agent\. 

**Note**  
If you use a `yum` command to update SSM Agent on a managed instance after the agent has been installed or updated using the SSM document `AWS-UpdateSSMAgent`, you might see the following message: "Warning: RPMDB altered outside of yum\." This message is expected and can be safely ignored\.

Perform these steps on each instance that will run commands using Systems Manager\.

**Important**  
SSM Agent is installed, by default, on Amazon Linux *base* AMIs dated 2017\.09 and later\. SSM Agent is also installed, by default, on Amazon Linux 2 AMIs\.
You must manually install SSM Agent on other versions of Linux\.
Instances created from an Amazon Linux AMI that are using a proxy must be running a current version of the Python `requests` module in order to support Patch Manager operations\. For more information, see [Upgrade the Python Requests Module on Amazon Linux Instances That Use a Proxy Server](sysman-proxy-with-ssm-agent-al-python-requests.md)\.

------
#### [ Amazon Linux ]

**To install SSM Agent on Amazon Linux**

1. Use one of the following commands to download and run the SSM Agent installer\. 
**Note**  
Even though the following download URLs show 'ec2\-downloads\-windows', these are the correct URLs for downloading Amazon Linux and Amazon Linux 2\.

   Intel \(x86\_64\) 64\-bit instances:

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
   ```

   ARM \(arm64\) 64\-bit instances:

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_arm64/amazon-ssm-agent.rpm
   ```

   Intel \(x86\) 32\-bit instances:

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_386/amazon-ssm-agent.rpm
   ```

1. Run the following command to determine if SSM Agent is running\. The command should return the message "amazon\-ssm\-agent is running\."

   Check the status of the agent\.

   ```
   sudo status amazon-ssm-agent
   ```

1. Run the following commands if the previous command returns the message "amazon\-ssm\-agent is stopped\."

   1. Start the service\.

      ```
      sudo start amazon-ssm-agent
      ```

   1. Check the status of the agent\.

      ```
      sudo status amazon-ssm-agent
      ```

------
#### [ Amazon Linux 2 ]

**To install SSM Agent on Amazon Linux 2**

1. Use one of the following commands to download and run the SSM Agent installer\. 
**Note**  
Even though the following download URLs show 'ec2\-downloads\-windows', these are the correct URLs for downloading Amazon Linux and Amazon Linux 2\.

   Intel \(x86\_64\) 64\-bit instances:

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
   ```

   ARM \(arm64\) 64\-bit instances:

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_arm64/amazon-ssm-agent.rpm
   ```

   Intel \(x86\) 32\-bit instances:

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_386/amazon-ssm-agent.rpm
   ```

1. Run the following command to determine if SSM Agent is running\. The command should return the message "amazon\-ssm\-agent is running\."

   Check the status of the agent\.

   ```
   sudo systemctl status amazon-ssm-agent
   ```

1. Run the following commands if the previous command returns the message "amazon\-ssm\-agent is stopped\."

   1. 

      ```
      sudo systemctl enable amazon-ssm-agent
      ```

      ```
      sudo systemctl start amazon-ssm-agent
      ```

   1. Check the status of the agent\.

      ```
      sudo systemctl status amazon-ssm-agent
      ```

------

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate Updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

## Ubuntu Server<a name="agent-install-ubuntu"></a>

Connect to your Ubuntu Server instance and perform the steps in one of following procedures to install SSM Agent on each instance that will run commands using Systems Manager\.

### About SSM Agent installations on 64\-bit Ubuntu Server 16\.04 instances<a name="agent-install-ubuntu-about-v16"></a>

Beginning with instances created from Ubuntu Server 16\.04 AMIs identified with `20180627`, SSM Agent is pre\-installed using Snap packages\. For example: `ubuntu/images/hvm-ssd/ubuntu-xenial-16.04-amd64-server-20180627`\. On instances created from earlier AMIs, you should continue using deb installer packages\. 

**Important**  
Be aware that if an instance has more than one installation of the SSM Agent \(for example, one installed using a Snap, one installed using a deb installer\), your agent operations will not work correctly\.

You can check the source AMI ID for an instance following these steps:

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the left navigation, choose **Instances**\.

1. Select an instance\.

1. On the **Description** tab, locate the value in the **AMI ID** field\.

For instances created from a 64\-bit Ubuntu Server 16\.04 AMI, be sure to follow the correct procedure for your SSM Agent installation type:
+ **Instances created from AMIs with identifier `20180627` or later**: [Install SSM Agent on Ubuntu Server instances](#agent-install-ubuntu-tabs)
+ **Instances created from AMIs earlier than `20180627`**: [Install SSM Agent on Ubuntu Server instances](#agent-install-ubuntu-tabs)

### Install SSM Agent on Ubuntu Server instances<a name="agent-install-ubuntu-tabs"></a>

------
#### [ Ubuntu Server 18\.04 and 16\.04 LTS 64\-bit \(Snap\) ]

**To install SSM Agent on Ubuntu Server 18\.04 and 16\.04 LTS 64\-bit instances \(with Snap package\)**

1. SSM Agent is installed, by default, on Ubuntu Server 18\.04 and on 16\.04 LTS 64\-bit AMIs with an identifier of `20180627` or later\. For more information about version 16\.04 AMIs, see [Ubuntu Server](#agent-install-ubuntu)\.

   You can use the following script if you need to install SSM Agent on an on\-premises server or if you need to reinstall the agent\. You don't need to specify a URL for the download, because the `snap` command automatically downloads the agent from the [Snap app store](https://snapcraft.io/amazon-ssm-agent) at [https://snapcraft\.io](https://snapcraft.io)\.

   ```
   sudo snap install amazon-ssm-agent --classic
   ```
**Note**  
Note the following details about SSM Agent on Ubuntu Server 18\.04 and 16\.04:  
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
On Ubuntu Server 18\.04 and 16\.04, SSM Agent installer files, including agent binaries and config files, are stored in the following directory: `/snap/amazon-ssm-agent/current/`\. If you make changes to the config files \(`amazon-ssm-agent.json.template` and `seelog.xml.template`\) then you must copy these files from the `/snap` folder to the `/etc/amazon/ssm/` folder\. Log and library files have not changed \(`/var/lib/amazon/ssm`, `/var/log/amazon/ssm`\)\.
On Ubuntu Server 18\.04, use Snaps only\. Don't install deb packages\. Also verify that only one instance of the agent is installed and running on your instances\.
On Ubuntu Server 18\.04 and 16\.04, SSM Agent provides support for the arm64 processor architecture\.
On Ubuntu Server 16\.04, SSM Agent is installed using either Snaps or deb installation packages, depending on the version of the 16\.04 AMI\. For more information, see [Ubuntu Server](#agent-install-ubuntu)\.

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

------
#### [ Ubuntu Server 16\.04 and 14\.04 64\-bit \(Deb\) ]

**To install SSM Agent on Ubuntu Server 16\.04 and 14\.04 64\-bit instances \(with deb installer package\)**

1. You can use the following script if you need to install SSM Agent on an on\-premises server or if you need to reinstall the agent\.
**Important**  
SSM Agent is installed by default on instances created from Ubuntu Server 16\.04 LTS 64\-bit AMIs with an identifier of `20180627` or later\. Instances created from AMIs with earlier identifiers, for example `20171121.1` and `20180522`, should continue to use deb installers\.   
If SSM Agent is installed on your instance in conjunction with a Snap and you install or update SSM Agent using a deb installer package, the installation or SSM Agent operations may fail\. For more information, see [Ubuntu Server](#agent-install-ubuntu)\.

   Create a temporary directory on the instance\.

   ```
   mkdir /tmp/ssm
   ```

   Change to the temporary directory\.

   ```
   cd /tmp/ssm
   ```

   Run the following commands\.
**Note**  
Even though the following download URL shows 'ec2\-downloads\-windows', this is the correct URL\.

   ```
   wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb
   ```

   ```
   sudo dpkg -i amazon-ssm-agent.deb
   ```

1. Run one of the following commands to determine if SSM Agent is running\. 

   Ubuntu Server 16\.04:

   ```
   sudo systemctl status amazon-ssm-agent
   ```

   Ubuntu Server 14\.04:

   ```
   sudo status amazon-ssm-agent
   ```

1. Run one of the following commands to start the service if the previous command returned amazon\-ssm\-agent is stopped, inactive, or disabled\.

   Ubuntu Server 16\.04:

   ```
   sudo systemctl enable amazon-ssm-agent
   ```

   Ubuntu Server 14\.04:

   ```
   sudo start amazon-ssm-agent
   ```

1. Run one of the following commands to check the status of the agent\.

   Ubuntu Server 16\.04:

   ```
   sudo systemctl status amazon-ssm-agent
   ```

   Ubuntu Server 14\.04:

   ```
   sudo status amazon-ssm-agent
   ```

------
#### [ Ubuntu Server 16\.04 and 14\.04 32\-bit ]

**To install SSM Agent on Ubuntu Server 16\.04 and 14\.04 32\-bit instances**

1. Create a temporary directory on the instance\.

   ```
   mkdir /tmp/ssm
   ```

   Change to the temporary directory\.

   ```
   cd /tmp/ssm
   ```

   Run the following commands\.
**Note**  
Even though the following download URL shows 'ec2\-downloads\-windows', this is the correct URL\.

   ```
   wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_386/amazon-ssm-agent.deb
   ```

   ```
   sudo dpkg -i amazon-ssm-agent.deb
   ```

1. Run the following command to determine if SSM Agent is running:

   ```
   sudo status amazon-ssm-agent
   ```

1. Run the following commands if the previous command returned amazon\-ssm\-agent is stopped, inactive, or disabled\.

   1. Start the agent:

      ```
      sudo start amazon-ssm-agent
      ```

   1. Check the status of the agent:

      ```
      sudo status amazon-ssm-agent
      ```

------

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate Updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

## Debian Server<a name="agent-install-deb"></a>

Connect to your Debian Server instance and perform the steps in one of following procedures to install SSM Agent on each instance that will run commands using Systems Manager\.

### Install SSM Agent on Debian Server instances<a name="agent-install-debian"></a>

------
#### [ Debian Server 9 64\-bit \(Deb\) ]

**To Install SSM Agent on Debian Server 9 64\-bit instances \(with deb installer package\)**

1. Connect to your Debian Server instance and perform the following steps to install SSM Agent\. Perform these steps on each instance that will run commands using Systems Manager\. 

   Create a temporary directory on the instance\.

   ```
   mkdir /tmp/ssm
   ```

   Change to the temporary directory\.

   ```
   cd /tmp/ssm
   ```

   Run the following commands\.
**Note**  
Even though the following download URL shows 'ec2\-downloads\-windows', this is the correct URL\.

   ```
   wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb
   ```

   ```
   sudo dpkg -i amazon-ssm-agent.deb
   ```

1. Run following command to determine if SSM Agent is running\. 

   ```
   sudo systemctl status amazon-ssm-agent
   ```

1. Run the following command to start the service if the previous command returned amazon\-ssm\-agent is stopped, inactive, or disabled\.

   ```
   sudo systemctl enable amazon-ssm-agent
   ```

1. Run the following command to check the status of the agent\.

   ```
   sudo systemctl status amazon-ssm-agent
   ```

------
#### [ Debian Server 8 64\-bit \(Deb\) ]

**To Install SSM Agent on Debian Server 8 64\-bit instances \(with deb installer package\)**

1. Connect to your Debian Server instance and perform the following steps to install SSM Agent\. Perform these steps on each instance that will run commands using Systems Manager\. 

   Create a temporary directory on the instance\.

   ```
   mkdir /tmp/ssm
   ```

   Change to the temporary directory\.

   ```
   cd /tmp/ssm
   ```

   Run the following commands\.
**Note**  
Even though the following download URL shows 'ec2\-downloads\-windows', this is the correct URL\.

   ```
   wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb
   ```

   ```
   sudo dpkg -i amazon-ssm-agent.deb
   ```

1. Run following command to determine if SSM Agent is running\. 

   ```
   sudo systemctl status amazon-ssm-agent
   ```

1. Run the following command to start the service if the previous command returned amazon\-ssm\-agent is stopped, inactive, or disabled\.

   ```
   sudo systemctl enable amazon-ssm-agent
   ```

1. Run the following command to check the status of the agent\.

   ```
   sudo systemctl status amazon-ssm-agent
   ```

------

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate Updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

## Red Hat Enterprise Linux \(RHEL\)<a name="agent-install-rhel"></a>

Connect to your RHEL instance and perform the following steps to install SSM Agent\. 

**Note**  
If you use a `yum` command to update SSM Agent on a managed instance after the agent has been installed or updated using the SSM document `AWS-UpdateSSMAgent`, you might see the following message: "Warning: RPMDB altered outside of yum\." This message is expected and can be safely ignored\.

Perform these steps on each instance that will run commands using Systems Manager\.

------
#### [ RHEL 7\.x ]

**To install SSM Agent on Red Hat Enterprise Linux 7\.x**

1. Use one of the following commands to download and run the SSM Agent installer\.
**Note**  
Even though the following download URLs show 'ec2\-downloads\-windows', these are the correct URLs\.

   Intel \(x86\_64\) 64\-bit instances:

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
   ```

   ARM \(arm64\) 64\-bit instances::

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_arm64/amazon-ssm-agent.rpm
   ```

   Intel \(x86\) 32\-bit instances:

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_386/amazon-ssm-agent.rpm
   ```

1. Run one of the following commands to determine if SSM Agent is running\. The command should return the message amazon\-ssm\-agent is running\.

   ```
   sudo systemctl status amazon-ssm-agent
   ```

1. Run the following commands if the previous command returned amazon\-ssm\-agent is stopped\.

   1. Start the service\.

      ```
      sudo systemctl enable amazon-ssm-agent
      ```

      ```
      sudo systemctl start amazon-ssm-agent
      ```

   1. Check the status of the agent\.

      ```
      sudo systemctl status amazon-ssm-agent
      ```

------
#### [ RHEL 6\.x ]

**To install SSM Agent on Red Hat Enterprise Linux 6\.x**

1. Use one of the following commands to download and run the SSM Agent installer\.
**Note**  
Even though the following download URLs show 'ec2\-downloads\-windows', these are the correct URLs\.

   Intel \(x86\_64\) 64\-bit instances:

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
   ```

   ARM \(arm64\) 64\-bit instances::

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_arm64/amazon-ssm-agent.rpm
   ```

   Intel \(x86\) 32\-bit instances:

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_386/amazon-ssm-agent.rpm
   ```

1. Run one of the following commands to determine if SSM Agent is running\. The command should return the message amazon\-ssm\-agent is running\.

   ```
   sudo status amazon-ssm-agent
   ```

1. Run the following commands if the previous command returned amazon\-ssm\-agent is stopped\.

   1. Start the service\.

      ```
      sudo start amazon-ssm-agent
      ```

   1. Check the status of the agent\.

      ```
      sudo status amazon-ssm-agent
      ```

------

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate Updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

## Oracle Linux<a name="agent-install-oracle"></a>

Connect to your Oracle Linux 7\.5 or 7\.7 instance and perform the following steps to install SSM Agent\. 

**Note**  
If you use a `yum` command to update SSM Agent on a managed instance after the agent has been installed or updated using the SSM document `AWS-UpdateSSMAgent`, you might see the following message: "Warning: RPMDB altered outside of yum\." This message is expected and can be safely ignored\.

Perform these steps on each instance that will run commands using Systems Manager\.

**To install SSM Agent on Oracle Linux 7\.5 and 7\.7**

1. Use the following command to download and run the SSM Agent installer\.
**Note**  
Even though the following download URLs show 'ec2\-downloads\-windows', these are the correct URLs\.

   Intel \(x86\_64\) 64\-bit instances:

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
   ```

1. Run the following command to determine if SSM Agent is running\. The command should return the message amazon\-ssm\-agent is running\.

   ```
   sudo systemctl status amazon-ssm-agent
   ```

1. Run the following commands if the previous command returned amazon\-ssm\-agent is stopped\.

   1. Start the service\.

      ```
      sudo systemctl enable amazon-ssm-agent
      ```

      ```
      sudo systemctl start amazon-ssm-agent
      ```

   1. Check the status of the agent\.

      ```
      sudo systemctl status amazon-ssm-agent
      ```

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate Updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

## CentOS<a name="agent-install-centos"></a>

Connect to your CentOS instance and perform the following steps to install the SSM Agent\. 

**Note**  
If you use a `yum` command to update SSM Agent on a managed instance after the agent has been installed or updated using the SSM document `AWS-UpdateSSMAgent`, you might see the following message: "Warning: RPMDB altered outside of yum\." This message is expected and can be safely ignored\.

Perform these steps on each instance that will run commands using Systems Manager\.

------
#### [ CentOS 7\.x ]

**To install SSM Agent on CentOS 7\.x**

1. Use one of the following commands to download and run the SSM Agent installer\.
**Note**  
Even though the following download URLs show 'ec2\-downloads\-windows', these are the correct URLs\.

   64\-bit instances:

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
   ```

   32\-bit instances:

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_386/amazon-ssm-agent.rpm
   ```

1. Run one of the following commands to determine if SSM Agent is running\. The command should return the message amazon\-ssm\-agent is running\.

   ```
   sudo systemctl status amazon-ssm-agent
   ```

1. Run the following commands if the previous command returned amazon\-ssm\-agent is stopped\.

   1. Start the service\.

      ```
      sudo systemctl enable amazon-ssm-agent
      ```

      ```
      sudo systemctl start amazon-ssm-agent
      ```

   1. Check the status of the agent\.

      ```
      sudo systemctl status amazon-ssm-agent
      ```

------
#### [ CentOS 6\.x ]

**To install SSM Agent on CentOS 6\.x**

1. Use one of the following commands to download and run the SSM Agent installer\.
**Note**  
Even though the following download URLs show 'ec2\-downloads\-windows', these are the correct URLs\.

   64\-bit instances:

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
   ```

   32\-bit instances:

   ```
   sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_386/amazon-ssm-agent.rpm
   ```

1. Run one of the following commands to determine if SSM Agent is running\. The command should return the message amazon\-ssm\-agent is running\.

   ```
   sudo status amazon-ssm-agent
   ```

1. Run the following commands if the previous command returned amazon\-ssm\-agent is stopped\.

   1. Start the service\.

      ```
      sudo start amazon-ssm-agent
      ```

   1. Check the status of the agent\.

      ```
      sudo status amazon-ssm-agent
      ```

------

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate Updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

## SUSE Linux Enterprise Server \(SLES\) 12<a name="agent-install-sles"></a>

Connect to your SLES instance and perform the following steps to install the SSM Agent\. Perform these steps on each instance that will run commands using Systems Manager\.

**To install SSM Agent on SUSE Linux Enterprise Server**

1. Create a temporary directory on the instance\.

   ```
   mkdir /tmp/ssm
   ```

1. Change to the temporary directory\.

   ```
   cd /tmp/ssm
   ```

1. Run the following commands one at a time to download and run the SSM Agent installer\. 
**Note**  
Even though the following download URL shows 'ec2\-downloads\-windows', this is the correct URL\.

   64\-bit instances:

   ```
   wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
   ```

   ```
   sudo rpm --install amazon-ssm-agent.rpm
   ```

1. Run the following command to determine if SSM Agent is running\. The command should return the message amazon\-ssm\-agent is running\.

   ```
   sudo systemctl status amazon-ssm-agent
   ```

1. Run the following commands if the previous command returns the message amazon\-ssm\-agent is stopped\.

   1. Start the service\.

      ```
      sudo systemctl enable amazon-ssm-agent
      ```

      ```
      sudo systemctl start amazon-ssm-agent
      ```

   1. Check the status of the agent\.

      ```
      sudo systemctl status amazon-ssm-agent
      ```

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate Updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

## Raspbian<a name="agent-install-raspbianjessie"></a>

This section includes information about how to install SSM Agent on Raspbian Jessie and Raspbian Stretch, including Raspberry Pi \(32\-bit\) devices\.

**Before You Begin**  
To set up your Raspbian devices as Systems Manager managed instances, you need to create a managed\-instance activation\. After you complete the activation, you receive an activation code and ID\. This code/ID combination functions like an Amazon EC2 access ID and secret key to provide secure access to the Systems Manager service from your managed instances\. Store the activation code and ID in a safe place\. For more information about the activation process, see [Setting Up AWS Systems Manager for Hybrid Environments](systems-manager-managedinstances.md)\.

Connect to your Raspbian device and perform the following steps to install the SSM Agent\. Perform these steps on each instance that will run commands using Systems Manager\.

**To install SSM Agent on Raspbian devices**

1. Create a temporary directory on the instance\.

   ```
   mkdir /tmp/ssm
   ```

1. Use the following command to download and run the SSM Agent installer\.

   ```
   sudo curl https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_arm/amazon-ssm-agent.deb -o /tmp/ssm/amazon-ssm-agent.deb
   ```

1. Run the following command to install SSM Agent\.

   ```
   sudo dpkg -i /tmp/ssm/amazon-ssm-agent.deb
   ```

1. Run the following command to stop SSM Agent\.

   ```
   sudo service amazon-ssm-agent stop
   ```

1. Run the following command to register the agent using the managed\-instance activation code and ID you received when you completed the managed\-instance activation process\.

   ```
   sudo amazon-ssm-agent -register -code "code" -id "ID" -region "region"
   ```

1. Run the following command to start SSM Agent\.

   ```
   sudo service amazon-ssm-agent start
   ```

**Note**  
If you see the following error in the SSM Agent error logs, then the machine ID did not persist after a reboot:  
`Unable to load instance associations, unable to retrieve associations unable to retrieve associations error occurred in RequestManagedInstanceRoleToken: MachineFingerprintDoesNotMatch: Fingerprint does not match`  
Run the following command to make the machine ID persist after a reboot\.  

  ```
  umount /etc/machine-id
  systemd-machine-id-setup
  ```
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate Updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

## Download SSM Agent from a Specific Region<a name="sysman-install-ssm-agent-specific"></a>

If you want to download the agent from a *specific* region, copy the URL for your operating system, and then replace *region* with an appropriate value\.

*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager Service Endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

For example, to download SSM Agent for Amazon Linux, RHEL, CentOS, and SLES 64\-bit from the US West 1 Region, use the following URL:

```
https://s3.us-west-1.amazonaws.com/amazon-ssm-us-west-1/latest/linux_amd64/amazon-ssm-agent.rpm
```
+ Amazon Linux, RHEL, CentOS, and SLES 64\-bit:

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/linux\_amd64/amazon\-ssm\-agent\.rpm 
+ Amazon Linux, RHEL, and CentOS 32\-bit:

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/linux\_386/amazon\-ssm\-agent\.rpm
+ Ubuntu Server 64\-bit:

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_amd64/amazon\-ssm\-agent\.deb
+ Ubuntu Server 32\-bit:

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_386/amazon\-ssm\-agent\.deb
+ Raspbian:

  https://s3\.*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_arm/amazon\-ssm\-agent\.deb

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate Updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.