# Step 8: \(Optional\) Enabling and controlling permissions for SSH connections through Session Manager<a name="session-manager-getting-started-enable-ssh-connections"></a>

You can allow users in your AWS account to use the AWS Command Line Interface \(AWS CLI\) to establish Secure Shell \(SSH\) connections to managed nodes using AWS Systems Manager Session Manager\. Users who connect using SSH can also copy files between their local machines and managed nodes using Secure Copy Protocol \(SCP\)\. You can use this functionality to connect to managed nodes without opening inbound ports or maintaining bastion hosts\.

After enabling SSH connections, you can use AWS Identity and Access Management \(IAM\) policies to explictly allow or deny users, groups, or roles to make SSH connections using Session Manager\.

**Note**  
Logging isn't available for Session Manager sessions that connect through port forwarding or SSH\. This is because SSH encrypts all session data, and Session Manager only serves as a tunnel for SSH connections\.

**Topics**
+ [Enabling SSH connections for Session Manager](#ssh-connections-enable)
+ [Controlling user permissions for SSH connections through Session Manager](#ssh-connections-permissions)

## Enabling SSH connections for Session Manager<a name="ssh-connections-enable"></a>

Use the following steps to enable SSH connections through Session Manager on a managed node\. 

**To enable SSH connections for Session Manager**

1. On the managed node to which you want to allow SSH connections, do the following:
   + Ensure that SSH is running on the managed node\. \(You can close inbound ports on the node\.\)
   + Ensure that SSM Agent version 2\.3\.672\.0 or later is installed on the managed node\.

     For information about installing or updating SSM Agent on a managed node, see the following topics:
     + [Installing and configuring SSM Agent on EC2 instances for Windows Server](sysman-install-ssm-win.md)\.
     +  [Installing and configuring SSM Agent on EC2 instances for Linux](sysman-install-ssm-agent.md) 
     + [Installing and configuring SSM Agent on EC2 instances for macOS](install-ssm-agent-macos.md) 
     +  [Install SSM Agent for a hybrid environment \(Windows\)](sysman-install-managed-win.md) 
     + [Install SSM Agent for a hybrid environment \(Linux\)](sysman-install-managed-linux.md)
**Note**  
To use Session Manager with on\-premises servers, edge devices, and virtual machines \(VMs\) that you activated as managed nodes, you must use the Advanced\-Instances Tier\. For more information about advanced instances, see [Turning on the advanced\-instances tier](systems-manager-managedinstances-advanced.md)\.

1. On the local machine from which you want to connect to a managed node using SSH, do the following:
   + Ensure that version 1\.1\.23\.0 or later of the Session Manager plugin is installed\.

     For information about installing the Session Manager plugin, see [\(Optional\) Install the Session Manager plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.
   + Update the SSH configuration file to allow running a proxy command that starts a Session Manager session and transfer all data through the connection\.

     **Linux and macOS**
**Tip**  
The SSH configuration file is typically located at `~/.ssh/config`\.

     Add the following to the configuration file on the local machine\.

     ```
     # SSH over Session Manager
     host i-* mi-*
         ProxyCommand sh -c "aws ssm start-session --target %h --document-name AWS-StartSSHSession --parameters 'portNumber=%p'"
     ```

      **Windows** 
**Tip**  
The SSH configuration file is typically located at `C:\Users\username\.ssh\config`\.

     Add the following to the configuration file on the local machine\.

     ```
     # SSH over Session Manager
     host i-* mi-*
         ProxyCommand C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe "aws ssm start-session --target %h --document-name AWS-StartSSHSession --parameters portNumber=%p"
     ```
   + Create or verify that you have a Privacy Enhanced Mail certificate \(a PEM file\), or at minimum a public key, to use when establishing connections to managed nodes\. This must be a key that is already associated with the managed node\. 

     For example, for an Amazon Elastic Compute Cloud \(Amazon EC2\) instance, the key pair file you created or selected when you created the instance\. \(You specify the path to the certificate or key as part of the command to start a session\. For information about starting a session using SSH, see [Starting a session \(SSH\)](session-manager-working-with-sessions-start.md#sessions-start-ssh)\.\)

## Controlling user permissions for SSH connections through Session Manager<a name="ssh-connections-permissions"></a>

After you enable SSH connections through Session Manager on a managed node, you can use IAM policies to allow or deny users, groups, or roles the ability to make SSH connections through Session Manager\. 

**To use an IAM policy to allow SSH connections through Session Manager**
+ Use one of the following options:
  + **Option 1**: Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\. 

    In the navigation pane, choose **Policies**, and then update the permissions policy for the user or role you want to allow to start SSH connections through Session Manager\. 

    For example, add the following element to the Quickstart policy you created in [Quickstart end user policies for Session Manager](getting-started-restrict-access-quickstart.md#restrict-access-quickstart-end-user)\.\.

    ```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": "ssm:StartSession",
                "Resource": [
                    "arn:aws:ec2:region:987654321098:instance/i-02573cafcfEXAMPLE",
                    "arn:aws:ssm:*:*:document/AWS-StartSSHSession"
                ]
            }
        ]
    }
    ```
  + **Option 2**: Attach an inline policy to a user policy by using the AWS Management Console, the AWS CLI, or the AWS API\.

    Using the method of your choice, attach the policy statement in **Option 1** to the policy for an AWS user, group, or role\.

    For information, see [Adding and Removing IAM Identity Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *IAM User Guide*\.

**To use an IAM policy to deny SSH connections through Session Manager**
+ Use one of the following options:
  + **Option 1**: Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\. In the navigation pane, choose **Policies**, and then update the permissions policy for the user or role to block from starting Session Manager sessions\. 

    For example, add the following element to the Quickstart policy you created in [Quickstart end user policies for Session Manager](getting-started-restrict-access-quickstart.md#restrict-access-quickstart-end-user)\.

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
  + **Option 2**: Attach an inline policy to a user policy by using the AWS Management Console, the AWS CLI, or the AWS API\.

    Using the method of your choice, attach the policy statement in **Option 1** to the policy for an AWS user, group, or role\.

    For information, see [Adding and Removing IAM Identity Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) in the *IAM User Guide*\.