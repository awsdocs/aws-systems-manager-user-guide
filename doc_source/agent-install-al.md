# Manually install SSM Agent on Amazon Linux instances<a name="agent-install-al"></a>

Connect to your Amazon Linux instance and perform the following steps to install SSM Agent\. 

**Note**  
If you use a `yum` command to update SSM Agent on a managed instance after the agent has been installed or updated using the SSM document `AWS-UpdateSSMAgent`, you might see the following message: "Warning: RPMDB altered outside of yum\." This message is expected and can be safely ignored\.

Perform these steps on each instance that will run commands using Systems Manager\.

**Note**  
SSM Agent is installed, by default, on Amazon Linux *base* AMIs dated 2017\.09 and later\. SSM Agent is also installed, by default, on Amazon Linux 2 AMIs\. You must manually install SSM Agent on other versions of Linux\.  
Instances created from an Amazon Linux AMI that are using a proxy must be running a current version of the Python `requests` module in order to support Patch Manager operations\. For more information, see [Upgrade the Python requests module on Amazon Linux instances that use a proxy server](sysman-proxy-with-ssm-agent-al-python-requests.md)\.

**To install SSM Agent on Amazon Linux**

1. Use one of the following commands to download and run the SSM Agent installer\. 
**Note**  
Even though the following download URLs show 'ec2\-downloads\-windows', these are the correct URLs for downloading Amazon Linux\.

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

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.