# Manually install SSM Agent on Debian Server instances<a name="agent-install-deb"></a>

Connect to your Debian Server instance and perform the steps in one of following procedures to install SSM Agent on each instance that will run commands using Systems Manager\.

## Install SSM Agent on Debian Server instances<a name="agent-install-debian"></a>

------
#### [ Debian Server 9 and 10 64\-bit \(deb\) ]

**To install SSM Agent on Debian Server 9 64\-bit instances \(with deb installer package\)**

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
#### [ Debian Server 8 64\-bit \(deb\) ]

**To install SSM Agent on Debian Server 8 64\-bit instances \(with deb installer package\)**

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
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.