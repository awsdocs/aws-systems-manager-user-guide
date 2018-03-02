# Install SSM Agent on Amazon EC2 Linux Instances at Launch<a name="sysman-install-startup-linux"></a>

You can install SSM Agent when you launch an instance for the first time by using Amazon EC2 User Data\. On the **Configure Instance Details** page of the launch wizard, expand **Advanced Details** and then copy and paste one of the following scripts into the **User data** field\. For example:

![\[Install SSM Agent at start-up\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/runcommand-linux-userdata.png)![\[Install SSM Agent at start-up\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/)![\[Install SSM Agent at start-up\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/)

**Note**  
The process for installing SSM Agent on Raspbian Jessie devices is different than other Linux versions\. For more information, see [Raspbian](sysman-manual-agent-install.md#agent-install-raspbianjessie)\.

The URLs in the following scripts let you download the SSM Agent from *any* AWS region\. If you want to download the agent from a *specific* region, copy the URL for your operating system, and then replace *region* with an appropriate value\.

*region* represents the region identifier for an AWS region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager table of regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

For example, to download the SSM Agent for Amazon Linux, RHEL, CentOS, and SLES 64\-bit from the US West 1 Region, use the following URL:

```
https://s3-us-west-1.amazonaws.com/amazon-ssm-us-west-1/latest/linux_amd64/amazon-ssm-agent.rpm 
```

If the download fails, try replacing https://s3\-*region* with https://s3\.*region*\.

+ Amazon Linux, RHEL, CentOS, and SLES 64\-bit:

  https://s3\-*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/linux\_amd64/amazon\-ssm\-agent\.rpm 

+ Amazon Linux, RHEL, and CentOS 32\-bit:

  https://s3\-*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/linux\_386/amazon\-ssm\-agent\.rpm

+ Ubuntu Server 64\-bit:

  https://s3\-*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_amd64/amazon\-ssm\-agent\.deb

+ Ubuntu Server 32\-bit:

  https://s3\-*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_386/amazon\-ssm\-agent\.deb

+ Raspbian:

  https://s3\-*region*\.amazonaws\.com/amazon\-ssm\-*region*/latest/debian\_arm/amazon\-ssm\-agent\.deb

**RHEL 7\.x and CentOS 7\.x, 64\-bit**

```
#!/bin/bash
cd /tmp
sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
sudo systemctl start amazon-ssm-agent
```

**RHEL 7\.x and CentOS 7\.x, 32\-bit**

```
#!/bin/bash
cd /tmp
sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_386/amazon-ssm-agent.rpm
sudo systemctl start amazon-ssm-agent
```

**Amazon Linux, RHEL 6\.x, and CentOS 6\.x, 64\-bit**

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

**Ubuntu Server 16 64\-bit**

```
#!/bin/bash
cd /tmp			
wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb
sudo dpkg -i amazon-ssm-agent.deb
sudo systemctl enable amazon-ssm-agent
```

**Ubuntu Server 16 32\-bit**

```
#!/bin/bash
cd /tmp			
wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_386/amazon-ssm-agent.deb
sudo dpkg -i amazon-ssm-agent.deb
sudo systemctl enable amazon-ssm-agent
```

**SLES 12 64\-bit**

```
#!/bin/bash
cd /tmp
wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
sudo rpm --install amazon-ssm-agent.rpm
sudo systemctl start amazon-ssm-agent
```

**Ubuntu Server 14 64\-bit**

```
#!/bin/bash
cd /tmp			
wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb
sudo dpkg -i amazon-ssm-agent.deb
sudo start amazon-ssm-agent
```

**Ubuntu Server 14 32\-bit**

```
#!/bin/bash
cd /tmp			
wget https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_386/amazon-ssm-agent.deb
sudo dpkg -i amazon-ssm-agent.deb
sudo start amazon-ssm-agent
```

Save your changes, and complete the wizard\. When the instance launches, the system copies SSM Agent to the instance and starts it\. When the instance is online, you can configure it using Run Command\. For more information, see [Executing Commands Using Systems Manager Run Command](run-command.md)\.

**Note**  
You can automatically update SSM Agent on your instances when new versions become available by using Systems Manager State Manager\. For more information, see [Walkthrough: Automatically Update the SSM Agent \(CLI\)](sysman-state-cli.md)\.