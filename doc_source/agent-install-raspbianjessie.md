# Manually install SSM Agent on Raspbian instances<a name="agent-install-raspbianjessie"></a>

This section includes information about how to install SSM Agent on Raspbian Jessie and Raspbian Stretch, including Raspberry Pi \(32\-bit\) devices\.

**Before You Begin**  
To set up your Raspbian devices as Systems Manager managed instances, you need to create a managed\-instance activation\. After you complete the activation, you receive an activation code and ID\. This code/ID combination functions like an Amazon EC2 access ID and secret key to provide secure access to the Systems Manager service from your managed instances\. Store the activation code and ID in a safe place\. For more information about the activation process, see [Setting up AWS Systems Manager for hybrid environments](systems-manager-managedinstances.md)\.

Connect to your Raspbian device and perform the following steps to install the SSM Agent\. Perform these steps on each instance that will run commands using Systems Manager\.

**To install SSM Agent on Raspbian devices**

1. Create a temporary directory on the instance\.

   ```
   mkdir /tmp/ssm
   ```

1. Use the following command to download and run the SSM Agent installer\.

   ```
   sudo curl https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/debian_arm/amazon-ssm-agent.deb -o /tmp/ssm/amazon-ssm-agent.deb
   ```

1. Run the following command to install SSM Agent\.

   ```
   sudo dpkg -i /tmp/ssm/amazon-ssm-agent.deb
   ```

1. Run the following command to stop SSM Agent\.

   ```
   sudo service amazon-ssm-agent stop
   ```

1. Run the following command to register the agent using the managed\-instance activation code and ID you received when you completed the managed\-instance activation process\.

   ```
   sudo amazon-ssm-agent -register -code "code" -id "ID" -region "region"
   ```

1. Run the following command to start SSM Agent\.

   ```
   sudo service amazon-ssm-agent start
   ```

**Note**  
If you see the following error in the SSM Agent error logs, then the machine ID did not persist after a reboot:  
`Unable to load instance associations, unable to retrieve associations unable to retrieve associations error occurred in RequestManagedInstanceRoleToken: MachineFingerprintDoesNotMatch: Fingerprint does not match`  
Run the following command to make the machine ID persist after a reboot\.  

  ```
  umount /etc/machine-id
  systemd-machine-id-setup
  ```
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automate updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.