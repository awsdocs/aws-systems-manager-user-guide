# Manually install SSM Agent on Debian Server instances<a name="agent-install-deb"></a>

Connect to your Debian Server instance and perform the following steps to install AWS Systems Manager Agent \(SSM Agent\)\. Perform these steps on each instance that will run commands using Systems Manager\.

## Install SSM Agent on Debian Server instances<a name="agent-install-debian"></a>

------
#### [ Debian Server 9 and 10 64\-bit \(deb\) ]

**To install SSM Agent on Debian Server 9 and 10 64\-bit instances \(with deb installer package\)**

1. Connect to your Debian Server instance and perform the following steps to install SSM Agent\. Perform these steps on each instance that will run commands using Systems Manager\. 

   Create a temporary directory on the instance\.

   ```
   mkdir /tmp/ssm
   ```

   Change to the temporary directory\.

   ```
   cd /tmp/ssm
   ```

   Run one of the following commands, depending on your instance type\.

   *region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.
   + Intel 64\-bit \(x86\_64\) instances:

     ```
     wget https://s3.region.amazonaws.com/amazon-ssm-region/latest/debian_amd64/amazon-ssm-agent.deb
     ```
   + ARM 64\-bit \(arm64\) instances:

     ```
     wget https://s3.region.amazonaws.com/amazon-ssm-region/latest/debian_arm64/amazon-ssm-agent.deb
     ```

1. Run the following command\.

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

   *region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

   ```
   wget https://s3.region.amazonaws.com/amazon-ssm-region/latest/debian_amd64/amazon-ssm-agent.deb
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

**Note**  
If you're unable to download the agent from the AWS Region you specify, use one of the following global URLs\. Even though the following URLs show 'ec2\-downloads\-windows', these are the correct URLs for Linux operating systems\.  
Intel 64\-bit \(x86\_64\)  

  ```
  https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_amd64/amazon-ssm-agent.deb
  ```
ARM 64\-bit \(arm64\)  

  ```
  https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_arm64/amazon-ssm-agent.deb
  ```
We don't officially support Debian Server 8 on ARM 64\-bit \(arm64\)\.

**Important**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on a managed node, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your machines\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. Subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub to get notifications about SSM Agent updates\.