# Troubleshooting Session Manager<a name="session-manager-troubleshooting"></a>

Use the following information to help you troubleshoot problems with Session Manager\.

**Topics**
+ [No Permission to Start a Session](#session-manager-troubleshooting-start-permissions)
+ [No Permission to Change Session Preferences](#session-maner-troubleshooting-preferences-permissions)
+ [Instance Not Available or Not Configured for Session Manager](#session-manager-troubleshooting-instances)
+ [Session Manager Plugin Not Found](#plugin-not-found)
+ [Session Manager Plugin Not Automatically Added to Command Line Path \(Windows\)](#windows-plugin-env-var-not-set)

## No Permission to Start a Session<a name="session-manager-troubleshooting-start-permissions"></a>

**Problem**: You attempt to start a session, but the system tells you that you do not have the necessary permissions\.
+ **Solution**: A system administrator has not granted you IAM policy permissions for starting Session Manager sessions\. For information, see [Control User Session Access to Instances](session-manager-getting-started-restrict-access.md)\.

## No Permission to Change Session Preferences<a name="session-maner-troubleshooting-preferences-permissions"></a>

**Problem**: You attempt to update global session preferences for your organization, but the system tells you that you do not have the necessary permissions\.
+ **Solution**: A system administrator has not granted you IAM policy permissions for setting Session Manager preferences\. For information, see [Grant or Deny a User Permissions to Update Session Manager Preferences](preference-setting-permissions.md)\.

## Instance Not Available or Not Configured for Session Manager<a name="session-manager-troubleshooting-instances"></a>

**Problem 1**: You want to start a session on the **Start a session** console page, but an instance is not in the list\.
+ **Solution**: The instance you want to connect to might not have been configured to use with the AWS Systems Manager service\. To use an instance with Systems Manager, an IAM instance profile that gives Systems Manager permission to perform actions on your instances must be attached to the instance\. For information, see [Create an IAM Instance Profile for Systems Manager](setup-instance-profile.md)\. 
**Note**  
If SSM Agent is already running on an instance when you attach the IAM instance profile, you might need to restart the agent before the instance is listed on the **Start a session** console page\.

**Problem 2**: An instance you want to connect is in the list on the **Start a session** console page, but the page reports that "The instance you selected is not configured to use Session Manager\." 
+ **Solution A**: The instance has been configured for use with the AWS Systems Manager service, but the IAM instance profile attached to the instance might not include permissions for the Session Manager capability\. For information, see [Verify or Create an IAM Instance Profile with Session Manager Permissions](session-manager-getting-started-instance-profile.md)\.
+ **Solution B**: The instance is not running a version of SSM Agent that supports Session Manager\. Update SSM Agent on the instance to version 2\.3\.68\.0 or later\. 

  Update SSM Agent manually on an instance by following the steps in [Install and Configure SSM Agent on Amazon EC2 Windows Instances](sysman-install-win.md) or [Manually Install SSM Agent on Amazon EC2 Linux Instances](sysman-manual-agent-install.md), depending on the operating system\. 

  Alternatively, use the Run Command document `AWS-UpdateSSMAgent` to update the agent version on one or more instances at a time\. For information, see [Update SSM Agent by using Run Command](rc-console.md#rc-console-agentexample)\.
**Tip**  
To always keep your agent up\-to\-date, we recommend updating SSM Agent to the latest version on an automated schedule that you define using either of the following methods:  
Run `AWS-UpdateSSMAgent` as part of a State Manager association\. For information, see [Automatically Update SSM Agent \(CLI\)](sysman-state-cli.md)\.
Run `AWS-UpdateSSMAgent` as part of a maintenance window\. For information about working with maintenance windows, see [Working with Maintenance Windows \(Console\)](sysman-maintenance-working.md) and [Tutorial: Create and Configure a Maintenance Window \(AWS CLI\)](maintenance-windows-cli-tutorials-create.md)\.

## Session Manager Plugin Not Found<a name="plugin-not-found"></a>

To use the AWS CLI to run session commands, the Session Manager plugin must also be installed on your local machine\. For information, see [\(Optional\) Install the Session Manager Plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.

## Session Manager Plugin Not Automatically Added to Command Line Path \(Windows\)<a name="windows-plugin-env-var-not-set"></a>

When you install the Session Manager plugin on Windows, the `session-manager-plugin` executable should be automatically added to your operating system's `PATH` environment variable\. If the command failed after you ran it to check whether the Session Manager plugin installed correctly \(`aws ssm start-session --target instance-id`\), you might need to set it manually using the following procedure\.

**To modify your PATH variable \(Windows\)**

1. Press the Windows key and enter **environment variables**\.

1. Choose **Edit environment variables for your account**\.

1. Choose **PATH** and then choose **Edit**\.

1. Add paths to the **Variable value** field, separated by semicolons\. For example: *`C:\existing\path`*;*`C:\new\path`*

   *`C:\existing\path`* represents the value already in the field\. *`C:\new\path`* represents the path you want to add\. For example:
   + **64\-bit machines**: `C:\Program Files\Amazon\SessionManagerPlugin\bin\`
   + **32\-bit machines**: `C:\Program Files (x86)\Amazon\SessionManagerPlugin\bin\` 

1. Choose **OK** twice to apply the new settings\.

1. Close any running command prompts and re\-open\.