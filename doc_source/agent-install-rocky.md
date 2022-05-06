# Manually install SSM Agent on Rocky Linux instances<a name="agent-install-rocky"></a>

Connect to your Rocky instance and perform the following steps to install AWS Systems Manager Agent \(SSM Agent\)\. Perform these steps on each instance that will run commands using Systems Manager\.

**Note**  
If you use a `yum` command to update SSM Agent on a managed node after the agent has been installed or updated using the SSM document `AWS-UpdateSSMAgent`, you might see the following message: "Warning: RPMDB altered outside of yum\." This message is expected and can be safely ignored\.

**To install SSM Agent on Rocky Linux 8**

1. Ensure that either Python 2 or Python 3 is installed on your Rocky 8\.4 or 8\.5 instance\. This is required in order for SSM Agent to work properly\.

1. Use one of the following commands to download and run the SSM Agent installer\.

   *region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

   x86\_64 instances:

   ```
   sudo dnf install -y https://s3.region.amazonaws.com/amazon-ssm-region/latest/linux_amd64/amazon-ssm-agent.rpm
   ```

   arm64 instances::

   ```
   sudo dnf install -y https://s3.region.amazonaws.com/amazon-ssm-region/latest/linux_arm64/amazon-ssm-agent.rpm
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

**Note**  
If you're unable to download the agent from the AWS Region you specify, use one of the following global URLs\. Even though the following URLs show 'ec2\-downloads\-windows', these are the correct URLs for Linux operating systems\.  
x86\_64  

  ```
  https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
  ```
arm64  

  ```
  https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_arm64/amazon-ssm-agent.rpm
  ```

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on a managed node, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your machines\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. Subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub to get notifications about SSM Agent updates\.