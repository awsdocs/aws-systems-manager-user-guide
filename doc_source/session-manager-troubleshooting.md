# Troubleshooting Session Manager<a name="session-manager-troubleshooting"></a>

Use the following information to help you troubleshoot problems with Session Manager\.

**Topics**
+ [No permission to start a session](#session-manager-troubleshooting-start-permissions)
+ [No permission to change session preferences](#session-maner-troubleshooting-preferences-permissions)
+ [Instance not available or not configured for Session Manager](#session-manager-troubleshooting-instances)
+ [Session Manager Plugin not found](#plugin-not-found)
+ [Session Manager Plugin not automatically added to command line path \(Windows\)](#windows-plugin-env-var-not-set)
+ [TargetNotConnected](#ssh-target-not-connected)
+ [Blank screen displays after starting a session](#session-manager-troubleshooting-start-blank-screen)

## No permission to start a session<a name="session-manager-troubleshooting-start-permissions"></a>

**Problem**: You try to start a session, but the system tells you that you do not have the necessary permissions\.
+ **Solution**: A system administrator has not granted you IAM policy permissions for starting Session Manager sessions\. For information, see [Control user session access to instances](session-manager-getting-started-restrict-access.md)\.

## No permission to change session preferences<a name="session-maner-troubleshooting-preferences-permissions"></a>

**Problem**: You try to update global session preferences for your organization, but the system tells you that you do not have the necessary permissions\.
+ **Solution**: A system administrator has not granted you IAM policy permissions for setting Session Manager preferences\. For information, see [Grant or deny a user permissions to update Session Manager preferences](preference-setting-permissions.md)\.

## Instance not available or not configured for Session Manager<a name="session-manager-troubleshooting-instances"></a>

**Problem 1**: You want to start a session on the **Start a session** console page, but an instance is not in the list\.
+ **Solution**: The instance you want to connect to might not have been configured to use with the AWS Systems Manager service\. To use an instance with Systems Manager, an IAM instance profile that gives Systems Manager permission to perform actions on your instances must be attached to the instance\. For information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. 
**Note**  
If SSM Agent is already running on an instance when you attach the IAM instance profile, you might need to restart the agent before the instance is listed on the **Start a session** console page\.

**Problem 2**: An instance you want to connect is in the list on the **Start a session** console page, but the page reports that "The instance you selected is not configured to use Session Manager\." 
+ **Solution A**: The instance has been configured for use with the AWS Systems Manager service, but the IAM instance profile attached to the instance might not include permissions for the Session Manager capability\. For information, see [Verify or Create an IAM Instance Profile with Session Manager Permissions](session-manager-getting-started-instance-profile.md)\.
+ **Solution B**: The instance is not running a version of SSM Agent that supports Session Manager\. Update SSM Agent on the instance to version 2\.3\.68\.0 or later\. 

  Update SSM Agent manually on an instance by following the steps in [Manually install SSM Agent on EC2 instances for Windows Server](sysman-install-win.md) or [Manually install SSM Agent on EC2 instances for Linux](sysman-manual-agent-install.md), depending on the operating system\. 

  Alternatively, use the Run Command document `AWS-UpdateSSMAgent` to update the agent version on one or more instances at a time\. For information, see [Update SSM Agent by using Run Command](rc-console.md#rc-console-agentexample)\.
**Tip**  
To always keep your agent up\-to\-date, we recommend updating SSM Agent to the latest version on an automated schedule that you define using either of the following methods:  
Run `AWS-UpdateSSMAgent` as part of a State Manager association\. For information, see [Automatically update SSM Agent \(CLI\)](sysman-state-cli.md)\.
Run `AWS-UpdateSSMAgent` as part of a maintenance window\. For information about working with maintenance windows, see [Working with maintenance windows \(console\)](sysman-maintenance-working.md) and [Tutorial: Create and configure a maintenance window \(AWS CLI\)](maintenance-windows-cli-tutorials-create.md)\.
+ **Solution C**: The instance is in a Virtual Private Cloud \(VPC\), but an `ssmmessages` endpoint has not been created in the VPC\. An `ssmmessages` endpoint in the format **com\.amazonaws\.*region*\.ssmmessages** is required if you are connecting to your instances through a secure data channel using Session Manager\. For more information, see [Creating VPC endpoints for Systems Manager](setup-create-vpc.md#sysman-setting-up-vpc-create)\.
+ **Solution D**: The instance has limited available CPU or memory resources\. Though your instance may otherwise be functional, if the instance does not have enough available resources, you can't establish a session\. For more information, see [Troubleshooting an Unreachable Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-console.html)\.

## Session Manager Plugin not found<a name="plugin-not-found"></a>

To use the AWS CLI to run session commands, the Session Manager plugin must also be installed on your local machine\. For information, see [\(Optional\) Install the Session Manager Plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.

## Session Manager Plugin not automatically added to command line path \(Windows\)<a name="windows-plugin-env-var-not-set"></a>

When you install the Session Manager plugin on Windows, the `session-manager-plugin` executable should be automatically added to your operating system's `PATH` environment variable\. If the command failed after you ran it to check whether the Session Manager plugin installed correctly \(`aws ssm start-session --target instance-id`\), you might need to set it manually using the following procedure\.

**To modify your PATH variable \(Windows\)**

1. Press the Windows key and enter **environment variables**\.

1. Choose **Edit environment variables for your account**\.

1. Choose **PATH** and then choose **Edit**\.

1. Add paths to the **Variable value** field, separated by semicolons, as shown in this example: *`C:\existing\path`*;*`C:\new\path`*

   *`C:\existing\path`* represents the value already in the field\. *`C:\new\path`* represents the path you want to add, as shown in these examples\.
   + **64\-bit machines**: `C:\Program Files\Amazon\SessionManagerPlugin\bin\`
   + **32\-bit machines**: `C:\Program Files (x86)\Amazon\SessionManagerPlugin\bin\` 

1. Choose **OK** twice to apply the new settings\.

1. Close any running command prompts and re\-open\.

## TargetNotConnected<a name="ssh-target-not-connected"></a>

**Problem**: You try to start a session, but the system returns the error message, "An error occurred \(TargetNotConnected\) when calling the StartSession operation: *InstanceID* is not connected\."
+ **Solution**: This error is returned when the specified target instance for the session is not fully configured for use with Session Manager\. For information, see [Getting started with Session Manager](session-manager-getting-started.md)\.

## Blank screen displays after starting a session<a name="session-manager-troubleshooting-start-blank-screen"></a>

**Problem**: You start a session and Session Manager displays a blank screen\.
+ **Solution A**: This issue can occur when the root volume on the instance is full\. Due to lack of disk space, SSM Agent on the instance stops working\. To resolve this issue, use Amazon CloudWatch to collect metrics and logs from the operating systems\. For information, see [Monitoring memory and disk metrics for Amazon EC2 Linux instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/mon-scripts.html) or [Monitoring memory and disk metrics for Amazon EC2 Windows instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/mon-scripts.html)\.
+ **Solution B**: A blank screen might display if you've accessed the console using a link that includes a mismatched endpoint and Region pair\. For example, in the following console URL, `us-west-2` is the specified endpoint, but `us-west-1` is the specified AWS Region:

  ```
  https://us-west-2.console.aws.amazon.com/systems-manager/session-manager/sessions?region=us-west-1
  ```
+ **Solution C**: The instance is connecting to Systems Manager using VPC endpoints, and your Session Manager preferences write session output to an Amazon S3 bucket, but an `s3` gateway endpoint does not exist in the VPC\. An `s3` endpoint in the format **com\.amazonaws\.*region*\.s3** is required if your instances are connecting to Systems Manager using VPC endpoints, and your Session Manager preferences write session output to an Amazon S3 bucket\. For more information, see [Creating VPC endpoints for Systems Manager](setup-create-vpc.md#sysman-setting-up-vpc-create)\.