# Step 8: \(Optional\) Enable SSH connections through Session Manager<a name="session-manager-getting-started-enable-ssh-connections"></a>

You can enable users in your AWS account to use the AWS CLI to establish Secure Shell \(SSH\) connections to instances using Session Manager\. Users who connect using SSH can also copy files between their local machines and managed instances using Secure Copy Protocol \(SCP\)\. You can use this functionality to connect to instances without opening inbound ports or maintaining bastion hosts\. You can also choose to explicitly disable SSH connections to your instances through Session Manager\.

**Note**  
Logging and auditing are not available for Session Manager sessions that connect through port forwarding or SSH\. This is because SSH encrypts all session data, and Session Manager only serves as a tunnel for SSH connections\.

**To enable SSH connections through Session Manager**

1. On the managed instance to which you want to enable SSH connections, do the following:
   + Ensure that SSH is running on the instance\. \(You can close inbound ports on the instance\.\)
   + Ensure that SSM Agent version 2\.3\.672\.0 or later is installed on the instance\.

     For information about installing or updating SSM Agent on an instance, see the following topics:
     + [Installing and configuring SSM Agent on EC2 instances for Windows Server](sysman-install-ssm-win.md)\.
     +  [Installing and configuring SSM Agent on EC2 instances for Linux](sysman-install-ssm-agent.md) 
     +  [Install SSM Agent for a hybrid environment \(Windows\)](sysman-install-managed-win.md) 
     +  [Install SSM Agent for a hybrid environment \(Linux\)](sysman-install-managed-linux.md) 
**Note**  
To use Session Manager with on\-premises servers and virtual machines \(VMs\) that you activated as managed instances, you must use the Advanced\-Instances Tier\. For more information about advanced instances, see [Enabling the advanced\-instances tier](systems-manager-managedinstances-advanced.md)\.

1. On the local machine from which you want to connect to a managed instance using SSH, do the following:
   + Ensure that version 1\.1\.23\.0 or later of the Session Manager plugin is installed\.

     For information about installing the Session Manager plugin, see [\(Optional\) Install the Session Manager Plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.
   + Update the SSH configuration file to enable running a proxy command that starts a Session Manager session and transfer all data through the connection\.

      **Linux** 
**Tip**  
The SSH configuration file is typically located at `~/.ssh/config`\.

     Add the following to the configuration file on the local machine:

     ```
     # SSH over Session Manager
     host i-* mi-*
         ProxyCommand sh -c "aws ssm start-session --target %h --document-name AWS-StartSSHSession --parameters 'portNumber=%p'"
     ```

      **Windows** 
**Tip**  
The SSH configuration file is typically located at `C:\Users\username\.ssh\config`\.

     Add the following to the configuration file on the local machine:

     ```
     # SSH over Session Manager
     host i-* mi-*
         ProxyCommand C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe "aws ssm start-session --target %h --document-name AWS-StartSSHSession --parameters portNumber=%p"
     ```
   + Create or verify that you have a Privacy Enhanced Mail Certificate \(a PEM file\), or at minimum a public key, to use when establishing connections to managed instances\. This must be a key that is already associated with the instance\. For example, for an EC2 instance, the key\-pair file you created or selected when you created the instance\. \(You specify the path to the certificate or key as part of the command to start a session\. For information about starting a session using SSH, see [Starting a session \(SSH\)](session-manager-working-with-sessions-start.md#sessions-start-ssh)\.\)

**To enable SSH connections through Session Manager**
+ Option 1: Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\. In the navigation pane, choose **Policies**, and then update the permissions policy for the user or role you want to allow to start SSH connections through Session Manager\. For example, prepare to modify the user quickstart policy you created in [Quickstart end user policies for Session Manager](getting-started-restrict-access-quickstart.md#restrict-access-quickstart-end-user)\. Add the following element to the policy\.

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": "ssm:StartSession",
              "Resource": [
                  "arn:aws:ec2:*:*:instance/instance-id",
                  "arn:aws:ssm:*:*:document/AWS-StartSSHSession"
              ]
          }
      ]
  }
  ```

  Option 2: Attach an inline policy to a user policy by using the AWS Management Console, the AWS CLI, or the AWS API\.

  Using the method of your choice, attach the policy statement in Option 1 to the policy for an AWS user, group, or role\.

  For information, see [Adding and Removing IAM Identity Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *IAM User Guide*\.

**To disable SSH connections through Session Manager**
+ Option 1: Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\. In the navigation pane, choose **Policies**, and then update the permissions policy for the user or role to block from starting Session Manager sessions\. For example, prepare to modify the user quickstart policy you created in [Quickstart end user policies for Session Manager](getting-started-restrict-access-quickstart.md#restrict-access-quickstart-end-user)\. Add the following element to the policy, or replace any permissions that allow a user to start a session:

  ```
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Sid": "VisualEditor1",
              "Effect": "Deny",
              "Action": "ssm:StartSession",
              "Resource": "arn:aws:ssm:*:*:document/AWS-StartSSHSession"
          }
      ]
  }
  ```

  Option 2: Attach an inline policy to a user policy by using the AWS Management Console, the AWS CLI, or the AWS API\.

  Using the method of your choice, attach the policy statement in Option 1 to the policy for an AWS user, group, or role\.

  For information, see [Adding and Removing IAM Identity Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *IAM User Guide*\.