# Install SSM Agent on Amazon EC2 Linux Instances at Launch<a name="sysman-install-startup-linux"></a>

You can install SSM Agent when you launch an instance for the first time by using Amazon EC2 User Data\. On the **Configure Instance Details** page of the launch wizard, expand **Advanced Details** and then copy and paste one of the following scripts into the **User data** field\. For example:

![\[Install SSM Agent at start-up\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/runcommand-linux-userdata.png)![\[Install SSM Agent at start-up\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/)![\[Install SSM Agent at start-up\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/)

**Note**  
The process for installing SSM Agent on Raspbian Jessie devices is different than other Linux versions\. For more information, see [Raspbian](sysman-manual-agent-install.md#agent-install-raspbianjessie)\.

The URLs in the following scripts let you download SSM Agent from *any* AWS region\. If you want to download the agent from a specific region, see [Download SSM Agent from a Specific Region](sysman-manual-agent-install.md#sysman-install-ssm-agent-specific)\.

**Topics**
+ [Amazon Linux, RHEL, and CentOS](#sysman-install-startup-linux-rhel)
+ [Ubuntu Server](#sysman-install-startup-ubuntu)
+ [SLES](#sysman-install-startup-sles)

## Amazon Linux, RHEL, and CentOS<a name="sysman-install-startup-linux-rhel"></a>

Copy and paste the following scripts into the Amazon EC2 User data field to automatically install SSM Agent\.

**RHEL 7\.x and CentOS 7\.x, 64\-bit**

```
#!/bin/bash
cd /tmp
sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
sudo systemctl start amazon-ssm-agent
```

**RHEL 7\.x and CentOS 7\.x 32\-bit**

```
#!/bin/bash
cd /tmp
sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_386/amazon-ssm-agent.rpm
sudo systemctl start amazon-ssm-agent
```

**Amazon Linux, RHEL 6\.x, and CentOS 6\.x, 64\-bit**

Copy and paste the following scripts into the Amazon EC2 User data field to automatically install SSM Agent\.

**Important**  
SSM Agent is installed, by default, on Amazon Linux *base* AMIs dated 2017\.09 and later\.
You must manually install SSM Agent on other versions of Linux, including non\-base images like *Amazon ECS\-Optimized AMIs*\.

```
#!/bin/bash
cd /tmp
sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
sudo start amazon-ssm-agent
```

**Amazon Linux, RHEL 6\.x, and CentOS 6\.x, 32\-bit**

**Important**  
SSM Agent is installed, by default, on Amazon Linux *base* AMIs dated 2017\.09 and later\.
You must manually install SSM Agent on other versions of Linux, including non\-base images like *Amazon ECS\-Optimized AMIs*\.

```
#!/bin/bash
cd /tmp
sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_386/amazon-ssm-agent.rpm
sudo start amazon-ssm-agent
```

## Ubuntu Server<a name="sysman-install-startup-ubuntu"></a>

Copy and paste the following scripts into the Amazon EC2 User data field to automatically install SSM Agent\.

**Ubuntu Server 18\.04 LTS 64\-bit**

SSM Agent is installed, by default, on Ubuntu Server 18\.04 LTS 64\-bit AMIs\. You can use the following script if you need to install SSM Agent on an on\-premises server or if you need to reinstall the agent\. You don't need to specify a URL for the download, because the `snap` command automatically downloads the agent from the Snap app store \(https://snapcraft\.io/amazon\-ssm\-agent\)\. 

```
#!/bin/bash
cd /tmp
sudo snap install amazon-ssm-agent --classic
sudo snap start amazon-ssm-agent
```

**Note**  
Note the following details about SSM Agent on Ubuntu Server 18\.04:  
Because of a known issue with snap, you might see a `Maximum timeout exceeded` error with snap commands\. If you get this error, use the following commands to start the agent, stop it, and check its status:   
systemctl start snap\.amazon\-ssm\-agent\.amazon\-ssm\-agent\.service
systemctl stop snap\.amazon\-ssm\-agent\.amazon\-ssm\-agent\.service
systemctl status snap\.amazon\-ssm\-agent\.amazon\-ssm\-agent\.service
On Ubuntu Server 18\.04, SSM Agent installer files, including agent binaries and config files, are stored in the following directory: /snap/amazon\-ssm\-agent/current/\. If you make changes to the config files \(amazon\-ssm\-agent\.json\.template and seelog\.xml\.template\) then you must copy these files from the /snap folder to the /etc/amazon/ssm/ folder\. Log and library files have not changed \(/var/lib/amazon/ssm, /var/log/amazon/ssm\)\.
Don't install snaps and debs on your Ubuntu Server instances\. This configuration can cause undesired output\. Also verify that only one instance of the agent is installed and running on your instances\.

**Ubuntu Server 16\.04 64\-bit**

```
#!/bin/bash
cd /tmp			
wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb
sudo dpkg -i amazon-ssm-agent.deb
sudo systemctl enable amazon-ssm-agent
```

**Ubuntu Server 16\.04 32\-bit**

```
#!/bin/bash
cd /tmp			
wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_386/amazon-ssm-agent.deb
sudo dpkg -i amazon-ssm-agent.deb
sudo systemctl enable amazon-ssm-agent
```

**Ubuntu Server 14\.04 64\-bit**

```
#!/bin/bash
cd /tmp			
wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb
sudo dpkg -i amazon-ssm-agent.deb
sudo start amazon-ssm-agent
```

**Ubuntu Server 14\.04 32\-bit**

```
#!/bin/bash
cd /tmp			
wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_386/amazon-ssm-agent.deb
sudo dpkg -i amazon-ssm-agent.deb
sudo start amazon-ssm-agent
```

## SLES<a name="sysman-install-startup-sles"></a>

**SLES 12 64\-bit**

```
#!/bin/bash
cd /tmp
wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
sudo rpm --install amazon-ssm-agent.rpm
sudo systemctl start amazon-ssm-agent
```

Save your changes, and complete the wizard\. When the instance launches, the system copies SSM Agent to the instance and starts it\. When the instance is online, you can configure it using Run Command\. For more information, see [Running Commands Using Systems Manager Run Command](run-command.md)\.

**Note**  
You can automatically update SSM Agent on your instances when new versions become available by using Systems Manager State Manager\. For more information, see [Walkthrough: Automatically Update the SSM Agent \(CLI\)](sysman-state-cli.md)\.