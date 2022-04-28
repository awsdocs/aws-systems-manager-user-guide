# Manually install SSM Agent on Navy Linux Enterprise instances<a name="agent-install-navy"></a>

Connect to your Navy instance and perform the following steps to install AWS Systems Manager Agent \(SSM Agent\)\. Perform these steps on each instance that will run commands using Systems Manager\.

**Note** 
If you use a `yum` command to update SSM Agent on a managed node after the agent has been installed or updated using the SSM document `AWS-UpdateSSMAgent`, you might see the following message: "Warning: RPMDB altered outside of yum\." This message is expected and can be safely ignored\.

------
#### [ NAVY 8\.x ]

**Method 1\: To install SSM Agent on Navy Linux Enterprise 8\.x**

1. Ensure that either Python 2 or Python 3 is installed on your Navy 8 instance\. This is required in order for SSM Agent to work properly\.
  
2. Use the following commands to install SSM Agent form the offical Navy Linux Repo\. 

  ```
   sudo yum install amazon-ssm-agent -y
   
   ```

**Method 2\: To install SSM Agent on Navy Linux Enterprise 8\.x**



1. Use the following commands to download and run the SSM Agent installer\.

   Intel 64\-bit \(x86\_64\) instances:

   ```
   ec2_availability_zone=`curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone`
   ec2_region="`echo \"$EC2_AVAIL_ZONE\" | sed 's/[a-z]$//'`"
   sudo dnf install -y https://s3.$ec2_region.amazonaws.com/amazon-ssm-$ec2_region/latest/linux_amd64/amazon-ssm-agent.rpm
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



**Important** 
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on a managed node, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your machines\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. Subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub to get notifications about SSM Agent updates\.
