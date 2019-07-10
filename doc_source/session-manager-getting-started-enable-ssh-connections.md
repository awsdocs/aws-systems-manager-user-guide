# Step 7: \(Optional\) Enable SSH Session Manager Sessions<a name="session-manager-getting-started-enable-ssh-connections"></a>

You can enable users in your AWS account to use the AWS CLI to start sessions on managed instances using SSH\. Users who connect using SSH can also copy files between their local machines and managed instances using SCP\.

**To enable SSH Session Manager sessions on managed instances**

1. On the managed instance to which you want to enable SSH connections, do the following:
   + Ensure that the SSH port is open internally\. You can remove the inbound port on the instance\.

     For information, see [Authorizing Inbound Traffic for Your Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html) in the *Amazon EC2 User Guide for Linux Instances*\.
   + Ensure that SSM Agent version 2\.3\.672\.0 or later is installed on the instance\.

     For information about installing or updating SSM Agent on an instance, see the following topics:
     + [Installing and Configuring SSM Agent on Windows Instances](sysman-install-ssm-win.md)\.
     + [Installing and Configuring SSM Agent on Amazon EC2 Linux Instances](sysman-install-ssm-agent.md)
     + [Install SSM Agent for a Hybrid Environment \(Windows\)](sysman-install-managed-win.md)
     + [Install SSM Agent for a Hybrid Environment \(Linux\)](sysman-install-managed-linux.md)
**Note**  
To use Session Manager with on\-premises servers and virtual machines \(VMs\) that you activated as managed instances, you must use the Advanced\-Instances Tier\. For more information about advanced instances, see [\(Optional\) Enable the Advanced\-Instances Tier](systems-manager-managedinstances-advanced.md)\.

1. On the local machine from which you want to connect to a managed instance using SSH, do the following:
   + Ensure that version 1\.1\.23\.0 or later of the Session Manager plugin is installed\.

     For information about installing the Session Manager plugin, see [\(Optional\) Install the Session Manager Plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.
   + Update the SSH config to enable running a proxy command that starts a Session Manager session and transfer all data through the connection\.
**Tip**  
The SSH config file is typically located at `~/.ssh/config`\.

     Add the following to the config file on the local machine:

     ```
     # SSH over Session Manager
     host i-* mi-*
         ProxyCommand sh -c "aws ssm start-session --target %h --document-name AWS-StartSSHSession --parameters 'portNumber=%p'"
     ```

For information about starting a session using SSH, see [Starting a Session \(SSH\)](session-manager-working-with-sessions-start.md#sessions-start-ssh)\. 