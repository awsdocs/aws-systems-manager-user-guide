# Troubleshooting Session Manager<a name="session-manager-troubleshooting"></a>

Use the following information to help you troubleshoot problems with AWS Systems Manager Session Manager\.

**Topics**
+ [No permission to start a session](#session-manager-troubleshooting-start-permissions)
+ [No permission to change session preferences](#session-manager-troubleshooting-preferences-permissions)
+ [Managed node not available or not configured for Session Manager](#session-manager-troubleshooting-instances)
+ [Session Manager plugin not found](#plugin-not-found)
+ [Session Manager plugin not automatically added to command line path \(Windows\)](#windows-plugin-env-var-not-set)
+ [Session Manager plugin becomes unresponsive](#plugin-unresponsive)
+ [TargetNotConnected](#ssh-target-not-connected)
+ [Blank screen displays after starting a session](#session-manager-troubleshooting-start-blank-screen)
+ [Managed node becomes unresponsive during long running sessions](#session-manager-troubleshooting-log-retention)

## No permission to start a session<a name="session-manager-troubleshooting-start-permissions"></a>

**Problem**: You try to start a session, but the system tells you that you don't have the necessary permissions\.
+ **Solution**: A system administrator hasn't granted you AWS Identity and Access Management \(IAM\) policy permissions for starting Session Manager sessions\. For information, see [Control user session access to instances](session-manager-getting-started-restrict-access.md)\.

## No permission to change session preferences<a name="session-manager-troubleshooting-preferences-permissions"></a>

**Problem**: You try to update global session preferences for your organization, but the system tells you that you don't have the necessary permissions\.
+ **Solution**: A system administrator hasn't granted you IAM policy permissions for setting Session Manager preferences\. For information, see [Grant or deny a user permissions to update Session Manager preferences](preference-setting-permissions.md)\.

## Managed node not available or not configured for Session Manager<a name="session-manager-troubleshooting-instances"></a>

**Problem 1**: You want to start a session on the **Start a session** console page, but a managed node isn't in the list\.
+ **Solution A**: The managed node you want to connect to might not have been configured for AWS Systems Manager\. For more information, see [Setting up AWS Systems Manager](systems-manager-setting-up.md)\. 
**Note**  
If AWS Systems Manager SSM Agent is already running on a managed node when you attach the IAM instance profile, you might need to restart the agent before the instance is listed on the **Start a session** console page\.
+ **Solution B**: The proxy configuration you applied to the SSM Agent on your managed node might be incorrect\. If the proxy configuration is incorrect, the managed node won't be able to reach the needed service endpoints, or the node might report as a different operating system to Systems Manager\. For more information, see [Configuring SSM Agent to use a proxy \(Linux\)](sysman-proxy-with-ssm-agent.md) and [Configure SSM Agent to use a proxy for Windows Server instances](sysman-install-ssm-proxy.md)\.

**Problem 2**: A managed node you want to connect is in the list on the **Start a session** console page, but the page reports that "The instance you selected isn't configured to use Session Manager\." 
+ **Solution A**: The managed node has been configured for use with the Systems Manager service, but the IAM instance profile attached to the node might not include permissions for the Session Manager capability\. For information, see [Verify or Create an IAM Instance Profile with Session Manager Permissions](session-manager-getting-started-instance-profile.md)\.
+ **Solution B**: The managed node isn't running a version of SSM Agent that supports Session Manager\. Update SSM Agent on the node to version 2\.3\.68\.0 or later\. 

  Update SSM Agent manually on a managed node by following the steps in [Manually installing SSM Agent on EC2 instances for Windows Server](sysman-install-win.md), [Manually installing SSM Agent on EC2 instances for Linux](sysman-manual-agent-install.md), or [Working with SSM Agent on EC2 instances for macOS](install-ssm-agent-macos.md), depending on the operating system\. 

  Alternatively, use the Run Command document `AWS-UpdateSSMAgent` to update the agent version on one or more managed nodes at a time\. For information, see [Updating the SSM Agent using Run Command](run-command-tutorial-update-software.md#rc-console-agentexample)\.
**Tip**  
To always keep your agent up to date, we recommend updating SSM Agent to the latest version on an automated schedule that you define using either of the following methods:  
Run `AWS-UpdateSSMAgent` as part of a State Manager association\. For information, see [Walkthrough: Automatically update SSM Agent \(CLI\)](sysman-state-cli.md)\.
Run `AWS-UpdateSSMAgent` as part of a maintenance window\. For information about working with maintenance windows, see [Working with maintenance windows \(console\)](sysman-maintenance-working.md) and [Tutorial: Create and configure a maintenance window \(AWS CLI\)](maintenance-windows-cli-tutorials-create.md)\.
+ **Solution C**: The managed node can't reach the requisite service endpoints\. You can improve the security posture of your managed nodes by using interface endpoints powered by AWS PrivateLink to connect to Systems Manager endpoints\. The alternative to using interface endpoints is to allow outbound internet access on your managed nodes\. For more information, see [Use PrivateLink to set up a VPC endpoint for Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-getting-started-privatelink.html)\.
+ **Solution D**: The managed node has limited available CPU or memory resources\. Although your managed node might otherwise be functional, if the node doesn't have enough available resources, you can't establish a session\. For more information, see [Troubleshooting an Unreachable Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-console.html)\.

## Session Manager plugin not found<a name="plugin-not-found"></a>

To use the AWS CLI to run session commands, the Session Manager plugin must also be installed on your local machine\. For information, see [Install the Session Manager plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.

## Session Manager plugin not automatically added to command line path \(Windows\)<a name="windows-plugin-env-var-not-set"></a>

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

## Session Manager plugin becomes unresponsive<a name="plugin-unresponsive"></a>

During a port forwarding session, traffic might stop forwarding if you have antivirus software installed on your local machine\. In some cases, antivirus software interferes with the Session Manager plugin causing process deadlocks\. To resolve this issue, allow or exclude the Session Manager plugin from the antivirus software\. For information about the default installation path for the Session Manager plugin, see [Install the Session Manager plugin for the AWS CLI](session-manager-working-with-install-plugin.md)\.

## TargetNotConnected<a name="ssh-target-not-connected"></a>

**Problem**: You try to start a session, but the system returns the error message, "An error occurred \(TargetNotConnected\) when calling the StartSession operation: *InstanceID* isn't connected\."
+ **Solution A**: This error is returned when the specified target managed node for the session isn't fully configured for use with Session Manager\. For information, see [Setting up Session Manager](session-manager-getting-started.md)\.
+ **Solution B**: This error is also returned if you attempt to start a session on a managed node that is located in a different AWS account or AWS Region\.

## Blank screen displays after starting a session<a name="session-manager-troubleshooting-start-blank-screen"></a>

**Problem**: You start a session and Session Manager displays a blank screen\.
+ **Solution A**: This issue can occur when the root volume on the managed node is full\. Due to lack of disk space, SSM Agent on the node stops working\. To resolve this issue, use Amazon CloudWatch to collect metrics and logs from the operating systems\. For information, see [Monitoring memory and disk metrics for Amazon EC2 Linux instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/mon-scripts.html) or [Monitoring memory and disk metrics for Amazon EC2 Windows instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/mon-scripts.html)\.
+ **Solution B**: A blank screen might display if you accessed the console using a link that includes a mismatched endpoint and Region pair\. For example, in the following console URL, `us-west-2` is the specified endpoint, but `us-west-1` is the specified AWS Region\.

  ```
  https://us-west-2.console.aws.amazon.com/systems-manager/session-manager/sessions?region=us-west-1
  ```
+ **Solution C**: The managed node is connecting to Systems Manager using VPC endpoints, and your Session Manager preferences write session output to an Amazon S3 bucket or Amazon CloudWatch Logs log group, but an `s3` gateway endpoint or `logs` interface endpoint doesn't exist in the VPC\. An `s3` endpoint in the format **`com.amazonaws.region.s3`** is required if your managed nodes are connecting to Systems Manager using VPC endpoints, and your Session Manager preferences write session output to an Amazon S3 bucket\. Alternatively, a `logs` endpoint in the format **`com.amazonaws.region.logs`** is required if your managed nodes are connecting to Systems Manager using VPC endpoints, and your Session Manager preferences write session output to a CloudWatch Logs log group\. For more information, see [Creating VPC endpoints for Systems Manager](setup-create-vpc.md#sysman-setting-up-vpc-create)\.
+ **Solution D**: The log group or Amazon S3 bucket you specified in your session preferences has been deleted\. To resolve this issue, update your session preferences with a valid log group or S3 bucket\.
+ **Solution E**: The log group or Amazon S3 bucket you specified in your session preferences isn't encrypted, but you have set the `cloudWatchEncryptionEnabled` or `s3EncryptionEnabled` input to `true`\. To resolve this issue, update your session preferences with a log group or Amazon S3 bucket that is encrypted, or set the `cloudWatchEncryptionEnabled` or `s3EncryptionEnabled` input to `false`\. This scenario is only applicable to customers who create session preferences using command line tools\.

## Managed node becomes unresponsive during long running sessions<a name="session-manager-troubleshooting-log-retention"></a>

**Problem**: Your managed node becomes unresponsive or crashes during a long running session\.

**Solution**: Decrease the SSM Agent log retention duration for Session Manager\.

**To decrease the SSM Agent log retention duration for sessions**

1. Locate the `amazon-ssm-agent.json.template` in the `/etc/amazon/ssm/` directory for Linux, or `C:\Program Files\Amazon\SSM` for Windows\.

1. Copy the contents of the `amazon-ssm-agent.json.template` to a new file in the same directory named `amazon-ssm-agent.json`\.

1. Decrease the default value of the `SessionLogsRetentionDurationHours` value in the `SSM` property, and save the file\.

1. Restart the SSM Agent\.