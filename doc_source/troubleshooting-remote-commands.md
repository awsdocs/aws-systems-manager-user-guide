# Troubleshooting Systems Manager Run Command<a name="troubleshooting-remote-commands"></a>

Run Command provides status details with each command execution\. For more information about the details of command statuses, see [Understanding command statuses](monitor-commands.md)\. You can also use the information in this topic to help troubleshoot problems with Run Command\.

**Topics**
+ [Where are my instances?](#where-are-instances)
+ [A step in my script failed, but the overall status is 'succeeded'](#ts-exit-codes)
+ [What's the status of my Windows instances?](#rc-healthapi-win)
+ [What's the status of my Linux instances?](#rc-healthapi-linux)
+ [Troubleshooting SSM Agent](#ts-ssmagent-linux)

## Where are my instances?<a name="where-are-instances"></a>

In the **Run a command** page, after you choose an SSM document to run and select **Manually selecting instances** in the **Targets** section, a list is displayed of instances you can choose to run the command on\.

After you create, activate, reboot, or restart a managed instance, install SSM Agent on an instance, or attach an IAM instance profile to an instance, it can take a few minutes for the instance to appear in the list\.

If an instance you expect to see is still not listed, check the following requirements\.
+ **SSM Agent**: Make sure the latest version of SSM Agent is installed on the instance\. Only Amazon Machine Images \(AMIs\) for Windows Server and some Linux AMIs are pre\-configured with SSM Agent\. For information about installing or reinstalling SSM Agent on an instance, see [Installing and configuring SSM Agent on EC2 instances for Linux](sysman-install-ssm-agent.md) or [Installing and configuring SSM Agent on Windows Server instances](sysman-install-ssm-win.md)\.
+ ** IAM instance role**: Verify that the instance is configured with an AWS Identity and Access Management \(IAM\) role that enables the instance to communicate with the Systems Manager API\. Also verify that your user account has an IAM user trust policy that enables your account to communicate with the Systems Manager API\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. 
+ **Service Endpoint connectivity**: Verify that the instance has connectivity to the Systems Manager service endpoints\. This connectivity is provided by creating and configuring VPC endpoints for Systems Manager, or by allowing HTTPS \(port 443\) outbound traffic to the service endpoints\. For more information, see [Step 6: \(Optional\) Create a Virtual Private Cloud endpoint](setup-create-vpc.md)\.
+ **Target operating system type**: Double\-check that you have selected an SSM document that supports the type of instance you want to update\. Most SSM documents support both Windows and Linux instances, but some do not\. For example, if you select the SSM document `AWS-InstallPowerShellModule`, which applies only to Windows Server instances, you will not see Linux instances in the target instances list\.

## A step in my script failed, but the overall status is 'succeeded'<a name="ts-exit-codes"></a>

Run Command lets you define how exit codes are handled in your scripts\. By default, the exit code of the last command run in a script is reported as the exit code for the entire script\. You can, however, include a conditional statement to exit the script if any command before the final one fails\. For information and examples, see [Managing exit codes in Run Command commands](command-exit-codes.md)\. 

## What's the status of my Windows instances?<a name="rc-healthapi-win"></a>

Use the following PowerShell command to get status details about one or more instances:

```
Get-SSMInstanceInformation -InstanceInformationFilterList @{Key="InstanceIds";ValueSet="instance-ID","instance-ID"}
```

Use the following PowerShell command with no filters to see all instances registered to your account that are currently reporting an online status\. Substitute the ValueSet="Online" with "ConnectionLost" or "Inactive" to view those statuses:

```
Get-SSMInstanceInformation -InstanceInformationFilterList @{Key="PingStatus";ValueSet="Online"}
```

Use the following PowerShell command to see which instances are running the latest version of the EC2Config service\. Substitute `ValueSet="LATEST"` with a specific version \(for example, `3.0.54` or `3.10`\) to view those details:

```
Get-SSMInstanceInformation -InstanceInformationFilterList @{Key="AgentVersion";ValueSet="LATEST"}
```

## What's the status of my Linux instances?<a name="rc-healthapi-linux"></a>

Use the following AWS CLI command to get status details about one or more instances:

```
aws ssm describe-instance-information --instance-information-filter-list key=InstanceIds,valueSet=instance-ID
```

Use the following command with no filters to see all instances registered to your account that are currently reporting an online status\. Substitute the `ValueSet="Online"` with `"ConnectionLost"` or `"Inactive"` to view those statuses:

```
aws ssm describe-instance-information --instance-information-filter-list key=PingStatus,valueSet=Online
```

Use the following command to see which instances are running the latest version of SSM Agent\. Substitute `ValueSet="LATEST"` with a specific version \(for example, `1.0.145` or `1.0`\) to view those details:

```
aws ssm describe-instance-information --instance-information-filter-list key=AgentVersion,valueSet=LATEST
```

If the describe\-instance\-information API operation returns an AgentStatus of `Online`, then your instance is ready to be managed using Run Command\. If the status is `Inactive`, the instance has one or more of the following problems\. 
+ SSM Agent is not installed\.
+ The instance does not have outbound internet connectivity\.
+ The instance was not launched with an IAM role that enables it to communicate with the SSM API, or the permissions for the IAM role are not correct for Run Command\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\.

## Troubleshooting SSM Agent<a name="ts-ssmagent-linux"></a>

If you experience problems executing commands using Run Command, there might be a problem with SSM Agent\. Use the following information to help you view SSM Agent log files and troubleshoot the agent\. 

**Topics**
+ [View SSM Agent log files](#systems-manager-ssm-agent-log-files)

### View SSM Agent log files<a name="systems-manager-ssm-agent-log-files"></a>

SSM Agent logs information in the following files\. The information in these files can help you troubleshoot problems\.

**Note**  
If you choose to view these logs by using Windows File Explorer, be sure to enable the viewing of hidden files and system files in Folder Options\.

**On Windows**
+ %PROGRAMDATA%\\Amazon\\SSM\\Logs\\amazon\-ssm\-agent\.log
+ %PROGRAMDATA%\\Amazon\\SSM\\Logs\\errors\.log

**On Linux**
+ /var/log/amazon/ssm/amazon\-ssm\-agent\.log
+ /var/log/amazon/ssm/errors\.log