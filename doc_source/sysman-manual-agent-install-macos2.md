# Manually install SSM Agent on EC2 instances for macOS<a name="sysman-manual-agent-install-macos2"></a>

Connect to your macOS instance and perform the following steps to install SSM Agent\. Perform these steps on *each* instance that will run commands using Systems Manager\.

**To install SSM Agent on macOS**

1. Download the agent installer file using the following command\.
**Note**  
Even though the following download URL shows 'ec2\-downloads\-windows', this is the correct URL for downloading the agent for macOS\.

   ```
   sudo wget https://s3.region.amazonaws.com/amazon-ssm-region/latest/darwin_amd64/amazon-ssm-agent.pkg
   ```

   Here is an example\.

   ```
   sudo wget https://s3.us-east-2.amazonaws.com/amazon-ssm-us-east-2/latest/darwin_amd64/amazon-ssm-agent.pkg
   ```

1. Use the following command to run the SSM Agent installer\. 

   Intel \(x86\_64\) 64\-bit instances:

   ```
   sudo installer -pkg amazon-ssm-agent.pkg -target 
   ```

1. Check the status of the agent\.

   To determine if SSM Agent is running, check the agent log at `var/log/amazon/ssm/amazon-ssm-agent.log` 

1. Run the following commands if the previous command returns the message "amazon\-ssm\-agent is stopped\."

   1. Start the service\.

     ```
     sudo launchctl load -w /Library/LaunchDaemons/com.amazon.aws.ssm.plist && sudo launchctl start com.amazon.aws.ssm
     ```

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub\.