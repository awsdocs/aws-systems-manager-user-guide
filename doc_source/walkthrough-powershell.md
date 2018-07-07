# Walkthrough: Use the AWS Tools for Windows PowerShell with Run Command<a name="walkthrough-powershell"></a>

The following examples show how to use the Tools for Windows PowerShell to view information about commands and command parameters, how to run commands, and how to view the status of those commands\. This walkthrough includes an example for each of the pre\-defined Systems Manager documents\.

**Important**  
Only trusted administrators should be allowed to use Systems Manager pre\-configured documents shown in this topic\. The commands or scripts specified in Systems Manager documents run with administrative privilege on your instances\. If a user has permission to run any of the pre\-defined Systems Manager documents \(any document that begins with AWS\), then that user also has administrator access to the instance\. For all other users, you should create restrictive documents and share them with specific users\. For more information about restricting access to Run Command, see [Configuring Access to Systems Manager](systems-manager-access.md)\.

**Topics**
+ [Configure AWS Tools for Windows PowerShell Session Settings](#walkthrough-powershell-settings)
+ [List all Available Documents](#walkthrough-powershell-all-documents)
+ [Run PowerShell Commands or Scripts](#walkthrough-powershell-run-script)
+ [Install an Application Using the AWS\-InstallApplication Document](#walkthrough-powershell-install-application)
+ [Install a PowerShell Module Using the AWS\-InstallPowerShellModule JSON Document](#walkthrough-powershell-install-module)
+ [Join an Instance to a Domain Using the AWS\-JoinDirectoryServiceDomain JSON Document](#walkthrough-powershell-domain-join)
+ [Send Windows Metrics to Amazon CloudWatch using the AWS\-ConfigureCloudWatch document](#walkthrough-powershell-windows-metrics)
+ [Enable/Disable Windows Automatic Update Using the AWS\-ConfigureWindowsUpdate document](#walkthrough-powershell-enable-windows-update)
+ [Update EC2Config Using the AWS\-UpdateEC2Config Document](#walkthrough-powershell-update-ec2config)
+ [Manage Windows Updates Using Run Command](#walkthough-powershell-windows-updates)

## Configure AWS Tools for Windows PowerShell Session Settings<a name="walkthrough-powershell-settings"></a>

Open **AWS Tools for Windows PowerShell** on your local computer and run the following command to specify your credentials\. You must either have administrator privileges on the instances you want to configure or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.

```
Set-AWSCredentials –AccessKey key_name –SecretKey key_name
```

Execute the following command to set the region for your PowerShell session\. The example uses the US East \(Ohio\) Region \(us\-east\-2\)\. Run Command is currently available in the AWS Regions listed in [AWS Systems Manager](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *Amazon Web Services General Reference*\.

```
Set-DefaultAWSRegion -Region us-east-2
```

## List all Available Documents<a name="walkthrough-powershell-all-documents"></a>

This command lists all of the documents available for your account:

```
Get-SSMDocumentList
```

## Run PowerShell Commands or Scripts<a name="walkthrough-powershell-run-script"></a>

Using Run Command and the AWS\-RunPowerShell document, you can run any command or script on an EC2 instance as if you were logged onto the instance using Remote Desktop\. You can issue commands or type in a path to a local script to run the command\. 

**Note**  
For information about rebooting servers and instances when using Run Command to call scripts, see [Rebooting Managed Instance from Scripts](send-commands-reboot.md)\.

**View the description and available parameters**

```
Get-SSMDocumentDescription -Name "AWS-RunPowerShellScript"
```

**View more information about parameters**

```
Get-SSMDocumentDescription -Name "AWS-RunPowerShellScript" | select -ExpandProperty Parameters
```

### Send a command using the AWS\-RunPowerShellScript document<a name="walkthrough-powershell-run-script-send-command-aws-runpowershellscript"></a>

The following command shows the contents of the C:\\Users directory and the contents of the C:\\ directory on two instances\. 

```
$runPSCommand=Send-SSMCommand -InstanceId @('Instance-ID', 'Instance-ID') -DocumentName AWS-RunPowerShellScript -Comment 'Demo AWS-RunPowerShellScript with two instances' -Parameter @{'commands'=@('dir C:\Users', 'dir C:\')}
```

**Get command request details**  
The following command uses the Command ID to get the status of the command execution on both instances\. This example uses the Command ID that was returned in the previous command\. 

```
Get-SSMCommand -CommandId $runPSCommand.CommandId
```

The status of the command in this example can be Success, Pending, or InProgress\.

**Get command information per instance**  
The following command uses the command ID from the previous command to get the status of the command execution on a per instance basis\.

```
Get-SSMCommandInvocation -CommandId $runPSCommand.CommandId
```

**Get command information with response data for a specific instance**  
The following command returns the output of the original Send\-SSMCommand for a specific instance\. 

```
Get-SSMCommandInvocation -CommandId $runPSCommand.CommandId -Details $true -InstanceId Instance-ID | select -ExpandProperty CommandPlugins
```

### Cancel a command<a name="walkthrough-powershell-run-script-cancel-command"></a>

The following command cancels the Send\-SSMComand for the AWS\-RunPowerShellScript document\.

```
$cancelCommandResponse=Send-SSMCommand -InstanceId @('Instance-ID','Instance-ID') -DocumentName AWS-RunPowerShellScript -Comment 'Demo AWS-RunPowerShellScript with two instances' -Parameter @{'commands'='Start-Sleep –Seconds 120; dir C:\'} 
Stop-SSMCommand -CommandId $cancelCommandResponse.CommandId Get-SSMCommand -CommandId $cancelCommandResponse.CommandId
```

**Check the command status**  
The following command checks the status of the Cancel command

```
Get-SSMCommand -CommandId $cancelCommandResponse.CommandId
```

## Install an Application Using the AWS\-InstallApplication Document<a name="walkthrough-powershell-install-application"></a>

Using Run Command and the AWS\-InstallApplication document, you can install, repair, or uninstall applications on instances\. The command requires the path or address to an MSI\.

**Note**  
For information about rebooting servers and instances when using Run Command to call scripts, see [Rebooting Managed Instance from Scripts](send-commands-reboot.md)\.

**View the description and available parameters**

```
Get-SSMDocumentDescription -Name "AWS-InstallApplication"
```

**View more information about parameters**

```
Get-SSMDocumentDescription -Name "AWS-InstallApplication" | select -ExpandProperty Parameters
```

### Send a command using the AWS\-InstallApplication document<a name="walkthrough-powershell-install-application-send-command-aws-installapplication"></a>

The following command installs a version of Python on your instance in unattended mode, and logs the output to a local text file on your C: drive\.

```
$installAppCommand=Send-SSMCommand -InstanceId Instance-ID -DocumentName AWS-InstallApplication -Parameter @{'source'='https://www.python.org/ftp/python/2.7.9/python-2.7.9.msi'; 'parameters'='/norestart /quiet /log c:\pythoninstall.txt'}
```

**Get command information per instance**  
The following command uses the Command ID to get the status of the command execution 

```
Get-SSMCommandInvocation -CommandId $installAppCommand.CommandId -Details $true
```

**Get command information with response data for a specific instance**  
The following command returns the results of the Python installation\.

```
Get-SSMCommandInvocation -CommandId $installAppCommand.CommandId -Details $true -InstanceId Instance-ID | select -ExpandProperty CommandPlugins
```

## Install a PowerShell Module Using the AWS\-InstallPowerShellModule JSON Document<a name="walkthrough-powershell-install-module"></a>

You can use Run Command to install PowerShell modules on an EC2 instance\. For more information about PowerShell modules, see [Windows PowerShell Modules](https://msdn.microsoft.com/en-us/library/dd878324%28v=vs.85%29.aspx)\.

**View the description and available parameters**

```
Get-SSMDocumentDescription -Name "AWS-InstallPowerShellModule"
```

**View more information about parameters**

```
Get-SSMDocumentDescription -Name "AWS-InstallPowerShellModule" | select -ExpandProperty Parameters
```

### Install a PowerShell module<a name="walkthrough-powershell-install-module-install"></a>

The following command downloads the EZOut\.zip file, installs it, and then runs an additional command to install XPS viewer\. Lastly, the output of this command is uploaded to an Amazon S3 bucket named demo\-ssm\-output\-bucket\. 

```
$installPSCommand=Send-SSMCommand -InstanceId Instance-ID -DocumentName AWS-InstallPowerShellModule -Parameter @{'source'='https://gallery.technet.microsoft.com/EZOut-33ae0fb7/file/110351/1/EZOut.zip';'commands'=@('Add-WindowsFeature -name XPS-Viewer -restart')} -OutputS3BucketName demo-ssm-output-bucket
```

**Get command information per instance**  
The following command uses the Command ID to get the status of the command execution\. 

```
Get-SSMCommandInvocation -CommandId $installPSCommand.CommandId -Details $true
```

**Get command information with response data for the instance**  
The following command returns the output of the original Send\-SSMCommand for the specific command ID\. 

```
Get-SSMCommandInvocation -CommandId $installPSCommand.CommandId -Details $true | select -ExpandProperty CommandPlugins
```

## Join an Instance to a Domain Using the AWS\-JoinDirectoryServiceDomain JSON Document<a name="walkthrough-powershell-domain-join"></a>

Using Run Command, you can quickly join an instance to an AWS Directory Service domain\. Before executing this command you must [create a directory](http://docs.aws.amazon.com/directoryservice/latest/admin-guide/create_directory.html)\. We also recommend that you learn more about the AWS Directory Service\. For more information, see [What Is AWS Directory Service?](http://docs.aws.amazon.com/directoryservice/latest/admin-guide/)\.

Currently you can only join an instance to a domain\. You cannot remove an instance from a domain\.

**Note**  
For information about rebooting servers and instances when using Run Command to call scripts, see [Rebooting Managed Instance from Scripts](send-commands-reboot.md)\.

**View the description and available parameters**

```
Get-SSMDocumentDescription -Name "AWS-JoinDirectoryServiceDomain"
```

**View more information about parameters**

```
Get-SSMDocumentDescription -Name "AWS-JoinDirectoryServiceDomain" | select -ExpandProperty Parameters
```

### Join an instance to a domain<a name="walkthrough-powershell-domain-join-instance"></a>

The following command joins the instance to the given AWS Directory Service domain and uploads any generated output to the Amazon S3 bucket\. 

```
$domainJoinCommand=Send-SSMCommand -InstanceId Instance-ID -DocumentName AWS-JoinDirectoryServiceDomain -Parameter @{'directoryId'='d-9067386b64'; 'directoryName'='ssm.test.amazon.com'; 'dnsIpAddresses'=@('172.31.38.48', '172.31.55.243')} -OutputS3BucketName demo-ssm-output-bucket
```

**Get command information per instance**  
The following command uses the Command ID to get the status of the command execution\. 

```
Get-SSMCommandInvocation -CommandId $domainJoinCommand.CommandId -Details $true
```

**Get command information with response data for the instance**  
This command returns the output of the original Send\-SSMCommand for the specific command ID\.

```
Get-SSMCommandInvocation -CommandId $domainJoinCommand.CommandId -Details $true | select -ExpandProperty CommandPlugins
```

## Send Windows Metrics to Amazon CloudWatch using the AWS\-ConfigureCloudWatch document<a name="walkthrough-powershell-windows-metrics"></a>

You can send Windows Server messages in the application, system, security, and Event Tracing for Windows \(ETW\) logs to Amazon CloudWatch Logs\. When you enable logging for the first time, Systems Manager sends all logs generated within 1 minute from the time that you start uploading logs for the application, system, security, and ETW logs\. Logs that occurred before this time are not included\. If you disable logging and then later re\-enable logging, Systems Manager sends logs from the time it left off\. For any custom log files and Internet Information Services \(IIS\) logs, Systems Manager reads the log files from the beginning\. In addition, Systems Manager can also send performance counter data to Amazon CloudWatch\.

If you previously enabled CloudWatch integration in EC2Config, the Systems Manager settings override any settings stored locally on the instance in the C:\\Program Files\\Amazon\\EC2ConfigService\\Settings\\AWS\.EC2\.Windows\.CloudWatch\.json file\. For more information about using EC2Config to manage performance counters and logs on single instance, see [Sending Performance Counters to CloudWatch and Logs to CloudWatch Logs](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/send_logs_to_cwl.html)\.

**View the description and available parameters**

```
Get-SSMDocumentDescription -Name "AWS-ConfigureCloudWatch"
```

**View more information about parameters**

```
Get-SSMDocumentDescription -Name "AWS-ConfigureCloudWatch" | select -ExpandProperty Parameters
```

### Send Application Logs to CloudWatch<a name="walkthrough-powershell-windows-metrics-send-logs-cloudwatch"></a>

The following command configures the instance and moves Windows Applications logs to CloudWatch\.

```
$cloudWatchCommand=Send-SSMCommand -InstanceID Instance-ID -DocumentName 'AWS-ConfigureCloudWatch' -Parameter @{'properties'='{"engineConfiguration": {"PollInterval":"00:00:15", "Components":[{"Id":"ApplicationEventLog", "FullName":"AWS.EC2.Windows.CloudWatch.EventLog.EventLogInputComponent,AWS.EC2.Windows.CloudWatch", "Parameters":{"LogName":"Application", "Levels":"7"}},{"Id":"CloudWatch", "FullName":"AWS.EC2.Windows.CloudWatch.CloudWatchLogsOutput,AWS.EC2.Windows.CloudWatch", "Parameters":{"Region":"us-east-2", "LogGroup":"My-Log-Group", "LogStream":"i-1234567890abcdef0"}}], "Flows":{"Flows":["ApplicationEventLog,CloudWatch"]}}}'}
```

**Get command information per instance**  
The following command uses the Command ID to get the status of the command execution\. 

```
Get-SSMCommandInvocation -CommandId $cloudWatchCommand.CommandId -Details $true
```

**Get command information with response data for a specific instance**  
The following command returns the results of the Amazon CloudWatch configuration\.

```
Get-SSMCommandInvocation -CommandId $cloudWatchCommand.CommandId -Details $true -InstanceId Instance-ID | select -ExpandProperty CommandPlugins
```

### Send Performance Counters to CloudWatch Using the AWS\-ConfigureCloudWatch document<a name="walkthrough-powershell-windows-metrics-send-performance-counters-cloudwatch"></a>

The following demonstration command uploads performance counters to CloudWatch\. For more information, see the [Amazon CloudWatch Documentation](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/)\.

```
$cloudWatchMetricsCommand=Send-SSMCommand -InstanceID Instance-ID -DocumentName 'AWS-ConfigureCloudWatch' -Parameter @{'properties'='{"engineConfiguration": {"PollInterval":"00:00:15", "Components":[{"Id":"PerformanceCounter", "FullName":"AWS.EC2.Windows.CloudWatch.PerformanceCounterComponent.PerformanceCounterInputComponent,AWS.EC2.Windows.CloudWatch", "Parameters":{"CategoryName":"Memory", "CounterName":"Available MBytes", "InstanceName":"", "MetricName":"AvailableMemory", "Unit":"Megabytes","DimensionName":"", "DimensionValue":""}},{"Id":"CloudWatch", "FullName":"AWS.EC2.Windows.CloudWatch.CloudWatch.CloudWatchOutputComponent,AWS.EC2.Windows.CloudWatch", "Parameters":{"AccessKey":"", "SecretKey":"","Region":"us-east-2", "NameSpace":"Windows-Default"}}], "Flows":{"Flows":["PerformanceCounter,CloudWatch"]}}}'}
```

## Enable/Disable Windows Automatic Update Using the AWS\-ConfigureWindowsUpdate document<a name="walkthrough-powershell-enable-windows-update"></a>

Using Run Command and the AWS\-ConfigureWindowsUpdate document, you can enable or disable automatic Windows updates on your Windows instances\. This command configures the Windows update agent to download and install Windows updates on the day and hour that you specify\. If an update requires a reboot, the computer reboots automatically 15 minutes after updates have been installed\. With this command you can also configure Windows update to check for updates but not install them\. The AWS\-ConfigureWindowsUpdate document is compatible with Windows Server 2008, 2008 R2, 2012, 2012 R2, and 2016\.

**View the description and available parameters**

```
Get-SSMDocumentDescription –Name "AWS-ConfigureWindowsUpdate"
```

**View more information about parameters**

```
Get-SSMDocumentDescription -Name "AWS-ConfigureWindowsUpdate" | select -ExpandProperty Parameters
```

### Enable Windows automatic update<a name="walkthrough-powershell-enable-windows-update-automatic"></a>

The following command configures Windows Update to automatically download and install updates daily at 10:00 pm\. 

```
$configureWindowsUpdateCommand = Send-SSMCommand -InstanceId Instance-ID -DocumentName 'AWS-ConfigureWindowsUpdate' -Parameters @{'updateLevel'='InstallUpdatesAutomatically'; 'scheduledInstallDay'='Daily'; 'scheduledInstallTime'='22:00'}
```

**View command status for enabling Windows automatic update**  
The following command uses the Command ID to get the status of the command execution for enabling Windows Automatic Update\.

```
Get-SSMCommandInvocation -Details $true -CommandId $configureWindowsUpdateCommand.CommandId | select -ExpandProperty CommandPlugins
```

### Disable Windows automatic update<a name="walkthrough-powershell-enable-windows-update-disable"></a>

The following command lowers the Windows Update notification level so the system checks for updates but does not automatically update the instance\.

```
$configureWindowsUpdateCommand = Send-SSMCommand -InstanceId Instance-ID -DocumentName 'AWS-ConfigureWindowsUpdate' -Parameters @{'updateLevel'='NeverCheckForUpdates'}
```

**View command status for disabling Windows automatic update**  
The following command uses the Command ID to get the status of the command execution for disabling Windows automatic update\.

```
Get-SSMCommandInvocation -Details $true -CommandId $configureWindowsUpdateCommand.CommandId | select -ExpandProperty CommandPlugins
```

## Update EC2Config Using the AWS\-UpdateEC2Config Document<a name="walkthrough-powershell-update-ec2config"></a>

Using Run Command and the AWS\-EC2ConfigUpdate document, you can update the EC2Config service running on your Windows instances\. This command can update the EC2Config service to the latest version or a version you specify\.

**View the description and available parameters**

```
Get-SSMDocumentDescription -Name "AWS-UpdateEC2Config"
```

**View more information about parameters**

```
Get-SSMDocumentDescription -Name "AWS-UpdateEC2Config" | select -ExpandProperty Parameters
```

### Update EC2Config to the latest version<a name="walkthrough-powershell-update-ec2config-latest-version"></a>

```
Send-SSMCommand -InstanceId Instance-ID -DocumentName "AWS-UpdateEC2Config"
```

**Get command information with response data for the instance**  
This command returns the output of the specified command from the previous Send\-SSMCommand:

```
Get-SSMCommandInvocation -CommandId ID -Details $true -InstanceId Instance-ID | select -ExpandProperty CommandPlugins
```

### Update EC2Config to a specific version<a name="walkthrough-powershell-update-ec2config-specific-version"></a>

The following command will downgrade EC2Config to an older version:

```
Send-SSMCommand -InstanceId Instance-ID -DocumentName "AWS-UpdateEC2Config" -Parameter @{'version'='3.8.354'; 'allowDowngrade'='true'}
```

## Manage Windows Updates Using Run Command<a name="walkthough-powershell-windows-updates"></a>

Run Command includes three documents to help you manage updates for Amazon EC2 Windows instances\.
+ **AWS\-FindWindowsUpdates** — Scans an instance and determines which updates are missing\.
+ **AWS\-InstallMissingWindowsUpdates** — Installs missing updates on your EC2 instance\.
+ **AWS\-InstallSpecificUpdates** — Installs a specific update\.

**Note**  
For information about rebooting servers and instances when using Run Command to call scripts, see [Rebooting Managed Instance from Scripts](send-commands-reboot.md)\.

The following examples demonstrate how to perform the specified Windows Update management tasks\.

### Search for all missing Windows updates<a name="walkthough-powershell-windows-updates-search"></a>

```
Send-SSMCommand -InstanceId Instance-ID -DocumentName 'AWS-FindWindowsUpdates' -Parameters @{'UpdateLevel'='All'}
```

### Install specific Windows updates<a name="walkthough-powershell-windows-updates-install-specific"></a>

```
Send-SSMCommand -InstanceId Instance-ID -DocumentName 'AWS-InstallSpecificWindowsUpdates' -Parameters @{'KbArticleIds'='123456,KB567890,987654'}
```

### Install important missing Windows updates<a name="walkthough-powershell-windows-updates-install-missing"></a>

```
Send-SSMCommand -InstanceId Instance-ID -DocumentName 'AWS-InstallMissingWindowsUpdates' -Parameters @{'UpdateLevel'='Important'}
```

### Install missing Windows updates with specific exclusions<a name="walkthough-powershell-windows-updates-install-exclusions"></a>

```
Send-SSMCommand -InstanceId Instance-ID -DocumentName 'AWS-InstallMissingWindowsUpdates' -Parameters @{'UpdateLevel'='All';'ExcludeKbArticleIds'='KB567890,987654'}
```