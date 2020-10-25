# Manually install SSM Agent on SUSE Linux Enterprise Server 12 instances<a name="agent-install-sles"></a>

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
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.