# Manually install SSM Agent on CentOS instances<a name="agent-install-centos"></a>

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
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.