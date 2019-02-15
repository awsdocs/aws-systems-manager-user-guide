# Troubleshooting Systems Manager Run Command<a name="troubleshooting-remote-commands"></a>

Run Command provides status details with each command execution\. For more information about the details of command statuses, see [Understanding Command Statuses](monitor-commands.md)\. You can also use the information in this topic to help troubleshoot problems with Run Command\.

**Topics**
+ [Where Are My Instances?](#where-are-instances)
+ [Getting Status Information on Windows Instances](#rc-healthapi-win)
+ [Getting Status Information on Linux Instances](#rc-healthapi-linux)
+ [Troubleshooting SSM Agent](#ts-ssmagent-linux)

## Where Are My Instances?<a name="where-are-instances"></a>

In the **Run a command** page, after you choose an SSM document to run and select **Manually selecting instances** in the **Targets** section, a list is displayed of instances you can choose to run the command on\. If an instance you expect to see is not listed, check the following requirements:
+ **SSM Agent**: Make sure the latest version of SSM Agent is installed on the instance\. Only Amazon EC2 Windows Amazon Machine Images \(AMIs\) and some Linux AMIs are pre\-configured with SSM Agent\. For information about installing or reinstalling SSM Agent on an instance, see [Installing and Configuring SSM Agent on Amazon EC2 Linux Instances](sysman-install-ssm-agent.md) or [Installing and Configuring SSM Agent on Windows Instances](sysman-install-ssm-win.md)\.
+ ** IAM instance role**: Verify that the instance is configured with an AWS Identity and Access Management \(IAM\) role that enables the instance to communicate with the Systems Manager API\. Also verify that your user account has an IAM user trust policy that enables your account to communicate with the Systems Manager API\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\. 
+ **Target operating system type**: Double\-check that you have selected an SSM document that supports the type of instance you want to update\. Most SSM documents support both Windows and Linux instances, but some do not\. For example, if you select the SSM document `AWS-InstallPowerShellModule`, which applies only to Windows instances, you will not see Linux instances in the target instances list\.

**Check Instance Status Using the Health API**  
You can use the Amazon EC2 Health API to quickly determine the following information about Amazon EC2 instances:
+ The status of one or more instances
+ The last time the instance sent a heartbeat value
+ The version of SSM Agent
+ The operating system 
+ The version of the EC2Config service \(Windows\)
+ The status of the EC2Config service \(Windows\)

## Getting Status Information on Windows Instances<a name="rc-healthapi-win"></a>

Use the following command to get status details about one or more instances:

```
Get-SSMInstanceInformation -InstanceInformationFilterList @{Key="InstanceIds";ValueSet="instance-ID","instance-ID"}
```

Use the following command with no filters to see all instances registered to your account that are currently reporting an online status\. Substitute the ValueSet="Online" with "ConnectionLost" or "Inactive" to view those statuses:

```
Get-SSMInstanceInformation -InstanceInformationFilterList @{Key="PingStatus";ValueSet="Online"}
```

Use the following command to see which instances are running the latest version of the EC2Config service\. Substitute ValueSet="LATEST" with a specific version \(for example, 3\.0\.54 or 3\.10\) to view those details:

```
Get-SSMInstanceInformation -InstanceInformationFilterList @{Key="AgentVersion";ValueSet="LATEST"}
```

## Getting Status Information on Linux Instances<a name="rc-healthapi-linux"></a>

Use the following command to get status details about one or more instances:

```
aws ssm describe-instance-information --instance-information-filter-list key=InstanceIds,valueSet=instance-ID
```

Use the following command with no filters to see all instances registered to your account that are currently reporting an online status\. Substitute the ValueSet="Online" with "ConnectionLost" or "Inactive" to view those statuses:

```
aws ssm describe-instance-information --instance-information-filter-list key=PingStatus,valueSet=Online
```

Use the following command to see which instances are running the latest version of SSM Agent\. Substitute ValueSet="LATEST" with a specific version \(for example, 1\.0\.145 or 1\.0\) to view those details:

```
aws ssm describe-instance-information --instance-information-filter-list key=AgentVersion,valueSet=LATEST
```

If the describe\-instance\-information API operation returns an AgentStatus of Online, then your instance is ready to be managed using Run Command\. If the status is Inactive, the instance has one or more of the following problems\. 
+ SSM Agent is not installed\.
+ The instance does not have outbound internet connectivity\.
+ The instance was not launched with an IAM role that enables it to communicate with the SSM API, or the permissions for the IAM role are not correct for Run Command\. For more information, see [Configuring Access to Systems Manager](systems-manager-access.md)\.

## Troubleshooting SSM Agent<a name="ts-ssmagent-linux"></a>

If you experience problems executing commands using Run Command, there might be a problem with SSM Agent\. Use the following information to help you view SSM Agent log files and troubleshoot the agent\. 

**Topics**
+ [View SSM Agent Log Files](#systems-manager-ssm-agent-log-files)
+ [Enable SSM Agent Debug Logging](#systems-manager-ssm-agent-debug-log-files)

### View SSM Agent Log Files<a name="systems-manager-ssm-agent-log-files"></a>

SSM Agent logs information in the following files\. The information in these files can help you troubleshoot problems\.

**Note**  
If you choose to view these logs by using Windows File Explorer, be sure to enable the viewing of hidden files and system files in Folder Options\.

**On Windows**
+ %PROGRAMDATA%\\Amazon\\SSM\\Logs\\amazon\-ssm\-agent\.log
+ %PROGRAMDATA%\\Amazon\\SSM\\Logs\\errors\.log

**On Linux**
+ /var/log/amazon/ssm/amazon\-ssm\-agent\.log
+ /var/log/amazon/ssm/errors\.log

### Enable SSM Agent Debug Logging<a name="systems-manager-ssm-agent-debug-log-files"></a>

Use the follow procedure to enable SSM Agent debug logging on Windows Server and Linux managed instances\.

1. Either use Systems Manager Session Manager to connect to the instance where you want to enable debug logging, or log on to the managed instance\. For more information, see [Working with Session Manager](session-manager-working-with.md)\.

1. Make a copy of the **seelog\.xml\.template** file\. Change the name of the copy to **seelog\.xml**\. The file is located in the following directory:

   1. **Windows Server**: %PROGRAMFILES%\\Amazon\\SSM\\seelog\.xml\.template

   1. **Linux**: /etc/amazon/ssm/seelog\.xml\.template

1. Edit the seelog\.xml file to change the default logging behavior\. Change the value of **minlevel** from **info** to **debug**, as shown in the following example\.

   ```
   <seelog type="adaptive" mininterval="2000000" maxinterval="100000000" critmsgcount="500" minlevel="debug">
   ```

1. **Windows only**: Locate the following entry:

   ```
   filename="{{LOCALAPPDATA}}\Amazon\SSM\Logs\amazon-ssm-agent.log"
   ```

   Change this entry to use the following path:

   ```
   filename="C:\ProgramData\Amazon\SSM\Logs\amazon-ssm-agent.log"
   ```

1. **Windows only**: Locate the following entry:

   ```
   filename="{{LOCALAPPDATA}}\Amazon\SSM\Logs\errors.log"
   ```

   Change this entry to use the following path:

   ```
   filename="C:\ProgramData\Amazon\SSM\Logs\errors.log"
   ```

1. Restart SSM Agent\.
   + **Windows Server**: Use Windows Task Manager to restart AmazonSSMAgent\.exe\.
   + **Linux**: Execute the following command:

     ```
     sudo restart amazon-ssm-agent
     ```