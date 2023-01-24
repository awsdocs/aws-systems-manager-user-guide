# Resetting passwords on managed nodes<a name="managed-instances-password-reset"></a>

You can reset the password for any user on a managed node\. This includes Amazon Elastic Compute Cloud \(Amazon EC2\) instances; AWS IoT Greengrass core devices; and on\-premises servers, edge devices, and virtual machines \(VMs\) that are managed by AWS Systems Manager\. The password reset functionality is built on Session Manager, a capability of AWS Systems Manager\. You can use this functionality to connect to managed nodes without opening inbound ports, maintaining bastion hosts, or managing SSH keys\. 

Password reset is useful when a user has forgotten a password, or when you want to quickly update a password without making an RDP or SSH connection to a managed node\. 

**Prerequisites**  
Before you can reset the password on a managed node, the following requirements must be met:
+ The managed node on which you want to change a password must be a Systems Manager managed node\. Also, SSM Agent version 2\.3\.668\.0 or later must be installed on the managed node\.\) For information about installing or updating SSM Agent, see [Working with SSM Agent](ssm-agent.md)\.
+ The password reset functionality uses the Session Manager configuration that is set up for your account to connect to the managed node\. Therefore, the prerequisites for using Session Manager must have been completed for your account in the current AWS Region\. For more information, see [Setting up Session Manager](session-manager-getting-started.md)\.
**Note**  
Session Manager support for on\-premises machines is provided for the advanced\-instances tier only\. For more information, see [Turning on the advanced\-instances tier](systems-manager-managedinstances-advanced.md)\.
+ The AWS user who is changing the password must have the `ssm:SendCommand` permission for the managed node\. For more information, see [Restricting Run Command access based on tags](run-command-setting-up.md#tag-based-access)\.

**Restricting access**  
You can limit a user's ability to reset passwords to specific managed nodes\. This is done by using identity\-based policies for the Session Manager `ssm:StartSession` operation with the `AWS-PasswordReset` SSM document\. For more information, see [Control user session access to instances](session-manager-getting-started-restrict-access.md)\.

**Encrypting data**  
Turn on AWS Key Management Service \(AWS KMS\) complete encryption for Session Manager data to use the password reset option for managed nodes\. For more information, see [Turn on KMS key encryption of session data \(console\)](session-preferences-enable-encryption.md)\.

## Reset a password on a managed node<a name="managed-instance-reset-a-password"></a>

You can reset a password on a Systems Manager managed node using the Systems Manager **Fleet Manager** console or the AWS Command Line Interface \(AWS CLI\)\.

**To change the password on a managed node \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose the button next to the node that needs a new password\.

1. In the **Instance actions** menu, choose **Reset password**\.

1. For **User name**, enter the name of the user for which you're changing the password\. This can be any user name that has an account on the node\.

1. Choose **Submit**\.

1. Follow the prompts in the **Enter new password** command window to specify the new password\.
**Note**  
If the version of SSM Agent on the managed node doesn't support password resets, you're prompted to install a supported version using Run Command, a capability of AWS Systems Manager\.

**To reset the password on a managed node \(AWS CLI\)**

1. To reset the password for a user on a managed node, run the following command\. Replace each *example resource placeholder* with your own information\.
**Note**  
To use the AWS CLI to reset a password, the Session Manager plugin must be installed on your local machine\. For information, see [\(Optional\) Install the Session Manager plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.

------
#### [ Linux & macOS ]

   ```
   aws ssm start-session \
       --target instance-id \
       --document-name "AWS-PasswordReset" \
       --parameters '{"username": ["user-name"]}'
   ```

------
#### [ Windows ]

   ```
   aws ssm start-session ^
       --target instance-id ^
       --document-name "AWS-PasswordReset" ^
       --parameters username="user-name"
   ```

------

1. Follow the prompts in the **Enter new password** command window to specify the new password\.

## Troubleshoot password resets on managed nodes<a name="password-reset-troubleshooting"></a>

Many password reset issues can be resolved by ensuring that you have completed the [password reset prerequisites](#pw-reset-prereqs)\. For other problems, use the following information to help you troubleshoot password reset issues\.

**Topics**
+ [Managed node not available](#password-reset-troubleshooting-instances)
+ [SSM Agent not up\-to\-date \(console\)](#password-reset-troubleshooting-ssmagent-console)
+ [Password reset options aren't provided \(AWS CLI\)](#password-reset-troubleshooting-ssmagent-cli)
+ [No authorization to run `ssm:SendCommand`](#password-reset-troubleshooting-sendcommand)
+ [Session Manager error message](#password-reset-troubleshooting-session-manager)

### Managed node not available<a name="password-reset-troubleshooting-instances"></a>

**Problem**: You want to reset the password for a managed node on the **Managed instances** console page, but the node isn't in the list\.
+ **Solution**: The managed node you want to connect to might not be configured for Systems Manager\. To use an EC2 instance with Systems Manager, an AWS Identity and Access Management \(IAM\) instance profile that gives Systems Manager permission to perform actions on your instances must be attached to the instance\. For information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. 

  To use an on\-premises server, edge device, or virtual machine \(VM\) with Systems Manager, create an IAM service role that gives Systems Manager permission to perform actions on your machines\. For more information, see [Create an IAM service role for a hybrid environment](sysman-service-role.md)\. \(Session Manager support for on\-premises servers and VMs is provided for the advanced\-instances tier only\. For more information, see [Turning on the advanced\-instances tier](systems-manager-managedinstances-advanced.md)\.\)

### SSM Agent not up\-to\-date \(console\)<a name="password-reset-troubleshooting-ssmagent-console"></a>

**Problem**: A message reports that the version of SSM Agent doesn't support password reset functionality\.
+ **Solution**: Version 2\.3\.668\.0 or later of SSM Agent is required to perform password resets\. In the console, you can update the agent on the managed node by choosing **Update SSM Agent**\. 

  An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. Failing to use the latest version of the agent can prevent your managed node from using various Systems Manager capabilities and features\. For that reason, we recommend that you automate the process of keeping SSM Agent up to date on your machines\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. Subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub to get notifications about SSM Agent updates\.

### Password reset options aren't provided \(AWS CLI\)<a name="password-reset-troubleshooting-ssmagent-cli"></a>

**Problem**: You connect successfully to a managed node using the AWS CLI `[start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html)` command\. You specified the SSM Document `AWS-PasswordReset` and provided a valid user name, but prompts to change the password aren't displayed\.
+ **Solution**: The version of SSM Agent on the managed node isn't up\-to\-date\. Version 2\.3\.668\.0 or later is required to perform password resets\. 

  An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. Failing to use the latest version of the agent can prevent your managed node from using various Systems Manager capabilities and features\. For that reason, we recommend that you automate the process of keeping SSM Agent up to date on your machines\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. Subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub to get notifications about SSM Agent updates\.

### No authorization to run `ssm:SendCommand`<a name="password-reset-troubleshooting-sendcommand"></a>

**Problem**: You attempt to connect to a managed node to change the password but receive an error message saying that you aren't authorized to run `ssm:SendCommand` on the managed node\.
+ **Solution**: Your IAM user policy must include permission to run the `ssm:SendCommand` command\. For information, see [Restricting Run Command access based on tags](run-command-setting-up.md#tag-based-access)\.

### Session Manager error message<a name="password-reset-troubleshooting-session-manager"></a>

**Problem**: You receive an error message related to Session Manager\.
+ **Solution**: Password reset support requires that Session Manager is configured correctly\. For information, see [Setting up Session Manager](session-manager-getting-started.md) and [Troubleshooting Session Manager](session-manager-troubleshooting.md)\.