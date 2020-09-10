# Resetting passwords on managed instances<a name="managed-instances-password-reset"></a>

You can reset the password for any user on a managed instance\. This includes Amazon EC2 instances, on\-premises servers, and virtual machines \(VMs\) that are managed by AWS Systems Manager\. The password reset functionality is built on the AWS Systems Manager Session Manager capability\. You can use this functionality to connect to instances without opening inbound ports, maintaining bastion hosts, or managing SSH keys\. 

This makes the password reset option useful when a user has forgotten a password, or when you want to quickly update a password without making an RDP or SSH connection to the instance\. 

**Prerequisites**  
Before you can reset the password on an instance, the following requirements must be met:
+ The instance you want to change a password on must be a Systems Manager managed instance\. This means that SSM Agent is installed on the instance\. \(SSM Agent Version 2\.3\.668\.0 or later is required for changing passwords\.\) For information about installing or updating SSM Agent, see [Working with SSM Agent](ssm-agent.md)\.
+ The password reset functionality uses the AWS Session Manager configuration that is set up for your account to connect to the instance\. Therefore, the prerequisites for using Session Manager must have been completed for your account in the current Region\. For more information, see [Getting started with Session Manager](session-manager-getting-started.md)\.
**Note**  
Session Manager support for on\-premises servers is provided for the advanced\-instances tier only\. For information, see [Enabling the advanced\-instances tier](systems-manager-managedinstances-advanced.md)\.
+ The AWS user who is changing the password must have the `ssm:SendCommand` permission for the instance\. For information, see [Restricting Run Command access based on instance tags](sysman-rc-setting-up.md#sysman-rc-setting-up-cmdsec)\.

**Restricting Access**  
You can limit a user's ability to reset passwords to specific instances\. This is done by using identity\-based policies for the Session Manager `ssm:StartSession` action with the `AWS-PasswordReset` SSM document\. For more information, see [Control user session access to instances](session-manager-getting-started-restrict-access.md)\.

**Encrypting Data**  
You must enable AWS Key Management Service \(AWS KMS\) end\-to\-end encryption for Session Manager data to use the password reset option for managed instances\. For more information, see [Enable AWS KMS key encryption of session data \(console\)](session-preferences-enable-encryption.md)\.

## Reset a password on a managed instance<a name="managed-instance-reset-a-password"></a>

You can reset a password on a Systems Manager managed instance using the AWS Systems Manager **Managed Instances** console or the AWS CLI\.

**To change the password on a managed instance \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Managed Instances**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Managed Instances**\.

1. Choose the button next to the instance that needs a new password\.

1. In the **Actions** menu, choose **Reset password**\.

1. For **User name**, type the name of the user for which you are changing the password\. This can be any user name that has an account on the instance\.

1. Choose **Submit**\.

1. Follow the prompts in the **Enter new password** command window to specify the new password\.
**Note**  
If the version of SSM Agent on the instance doesn't support password resets, you are prompted to install a supported version using Run Command\.

**To reset the password on a managed instance \(CLI\)**

1. To reset the password for a user on a managed instance, run the following command\.
**Note**  
To use the AWS CLI to reset a password, the Session Manager plugin must be installed on your local machine\. For information, see [\(Optional\) Install the Session Manager Plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.

------
#### [ Linux ]

   ```
   aws ssm start-session \
       --target instance-id \
       --document-name "AWS-PasswordReset" \
       --parameters "{"username": "user-name"}"
   ```

------
#### [ Windows ]

   ```
   aws ssm start-session ^
       --target instance-id ^
       --document-name "AWS-PasswordReset" ^
       --parameters "{"username": "user-name"}"
   ```

------

   *instance\-id* represents the ID of an instance configured for use with Systems Manager and its Session Manager capability\. 

   *user\-name* represents the name of the user you want to reset password for on the instance\. 

1. Follow the prompts in the **Enter new password** command window to specify the new password\.

## Troubleshoot password resets on managed instances<a name="password-reset-troubleshooting"></a>

Many password reset issues can be resolved by ensuring that you have completed the [password reset prerequisites](#pw-reset-prereqs)\. For other problems, use the following information to help you troubleshoot password reset issues\.

**Topics**
+ [Instance not available](#password-reset-troubleshooting-instances)
+ [SSM Agent not up\-to\-date \(console\)](#password-reset-troubleshooting-ssmagent-console)
+ [Password reset options do not appear \(CLI\)](#password-reset-troubleshooting-ssmagent-cli)
+ [No authorization to run `ssm:SendCommand`](#password-reset-troubleshooting-sendcommand)
+ [Session Manager error message](#password-reset-troubleshooting-session-manager)

### Instance not available<a name="password-reset-troubleshooting-instances"></a>

**Problem**: You want to reset the password for an EC2 instance on the **Managed instances** console page, but the instance is not in the list\.
+ **Solution**: The instance you want to connect to might not be configured to use with the AWS Systems Manager service\. To use an EC2 instance with Systems Manager, an IAM instance profile that gives Systems Manager permission to perform actions on your instances must be attached to the instance\. For information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. To use an on\-premises server or virtual machine \(VM\) that you have activated for use with Systems Manager, you must create an IAM service role that gives Systems Manager permission to perform actions on your machines\. For information, see [Create an IAM service role for a hybrid environment](sysman-service-role.md)\. \(Session Manager support for on\-premises servers and VMs is provided for the advanced\-instances tier only\. For information, see [Enabling the advanced\-instances tier](systems-manager-managedinstances-advanced.md)\.\)

### SSM Agent not up\-to\-date \(console\)<a name="password-reset-troubleshooting-ssmagent-console"></a>

**Problem**: A message reports that the version of SSM Agent doesn't support password reset functionality\.
+ **Solution**: Version 2\.3\.668\.0 or later of SSM Agent is required to perform password resets\. In the console, you can begin the process of updating the agent on the instance by choosing **Update SSM Agent**\. 

  An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

### Password reset options do not appear \(CLI\)<a name="password-reset-troubleshooting-ssmagent-cli"></a>

**Problem**: You connect successfully to an instance using the AWS CLI `[start\-session](https://docs.aws.amazon.com/cli/latest/reference/ssm/start-session.html)` command\. You specified the SSM Document `AWS-PasswordReset` and provided a valid user name, but prompts to change the password do not appear\.
+ **Solution**: The version of SSM Agent on the instance is not up\-to\-date\. Version 2\.3\.668\.0 or later is required to perform password resets\. 

  An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on an instance, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your instances\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

### No authorization to run `ssm:SendCommand`<a name="password-reset-troubleshooting-sendcommand"></a>

**Problem**: You attempt to connect to an instance to change its password but receive an error message saying that you aren't authorized to run `ssm:SendCommand` on the instance\.
+ **Solution**: Your IAM user policy must include permission to run the `ssm:SendCommand` command\. For information, see [Restricting Run Command access based on instance tags](sysman-rc-setting-up.md#sysman-rc-setting-up-cmdsec)\.

### Session Manager error message<a name="password-reset-troubleshooting-session-manager"></a>

**Problem**: You receive an error message related to Session Manager\.
+ **Solution**: Password reset support requires that Session Manager is configured correctly\. For information, see [Getting started with Session Manager](session-manager-getting-started.md) and [Troubleshooting Session Manager](session-manager-troubleshooting.md)\.