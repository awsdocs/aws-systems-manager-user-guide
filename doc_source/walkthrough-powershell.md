# Walkthrough: Use the AWS Tools for Windows PowerShell with Run Command<a name="walkthrough-powershell"></a>

The following examples show how to use the Tools for Windows PowerShell to view information about commands and command parameters, how to run commands, and how to view the status of those commands\. This walkthrough includes an example for each of the pre\-defined Systems Manager documents\.

**Important**  
Only trusted administrators should be allowed to use Systems Manager pre\-configured documents shown in this topic\. The commands or scripts specified in Systems Manager documents run with administrative privilege on your instances\. If a user has permission to run any of the pre\-defined Systems Manager documents \(any document that begins with AWS\), then that user also has administrator access to the instance\. For all other users, you should create restrictive documents and share them with specific users\. For more information about restricting access to Run Command, see [ Create non\-Admin IAM users and groups for Systems Manager](setup-create-iam-user.md)\.

**Topics**
+ [Configure AWS Tools for Windows PowerShell session settings](#walkthrough-powershell-settings)
+ [List all available documents](#walkthrough-powershell-all-documents)
+ [Run PowerShell commands or scripts](#walkthrough-powershell-run-script)
+ [Install an application using the AWS\-InstallApplication document](#walkthrough-powershell-install-application)
+ [Install a PowerShell module using the AWS\-InstallPowerShellModule JSON document](#walkthrough-powershell-install-module)
+ [Join an instance to a Domain using the AWS\-JoinDirectoryServiceDomain JSON document](#walkthrough-powershell-domain-join)
+ [Send Windows metrics to Amazon CloudWatch Logs using the AWS\-ConfigureCloudWatch document](#walkthrough-powershell-windows-metrics)
+ [Update EC2Config using the AWS\-UpdateEC2Config document](#walkthrough-powershell-update-ec2config)
+ [Enable/Disable Windows automatic update using the AWS\-ConfigureWindowsUpdate document](#walkthrough-powershell-enable-windows-update)
+ [Manage Windows updates using Run Command](#walkthough-powershell-windows-updates)

## Configure AWS Tools for Windows PowerShell session settings<a name="walkthrough-powershell-settings"></a>

**Specify your credentials**  
Open **AWS Tools for Windows PowerShell** on your local computer and run the following command to specify your credentials\. You must either have administrator privileges on the instances you want to configure or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager prerequisites](systems-manager-prereqs.md)\.

```
Set-AWSCredentials –AccessKey key-name –SecretKey key-name
```

**Set a default AWS Region**  
Run the following command to set the region for your PowerShell session\. The example uses the US East \(Ohio\) Region \(us\-east\-2\)\. Run Command is currently available in the AWS Regions listed in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

```
Set-DefaultAWSRegion `
	-Region us-east-2
```

## List all available documents<a name="walkthrough-powershell-all-documents"></a>

This command lists all of the documents available for your account:

```
Get-SSMDocumentList
```

## Run PowerShell commands or scripts<a name="walkthrough-powershell-run-script"></a>

Using Run Command and the `AWS-RunPowerShell` document, you can run any command or script on an EC2 instance as if you were logged onto the instance using Remote Desktop\. You can issue commands or type in a path to a local script to run the command\. 

**Note**  
For information about rebooting servers and instances when using Run Command to call scripts, see [Rebooting managed instance from scripts](send-commands-reboot.md)\.

**View the description and available parameters**

```
Get-SSMDocumentDescription `
	-Name "AWS-RunPowerShellScript"
```

**View more information about parameters**

```
Get-SSMDocumentDescription `
	-Name "AWS-RunPowerShellScript" | Select -ExpandProperty Parameters
```

### Send a command using the AWS\-RunPowerShellScript document<a name="walkthrough-powershell-run-script-send-command-aws-runpowershellscript"></a>

The following command shows the contents of the `"C:\Users"` directory and the contents of the `"C:\"` directory on two instances\. 

```
$runPSCommand = Send-SSMCommand `
	-InstanceIds @("instance-ID-1", "instance-ID-2") `
	-DocumentName "AWS-RunPowerShellScript" `
	-Comment "Demo AWS-RunPowerShellScript with two instances" `
	-Parameter @{'commands'=@('dir C:\Users', 'dir C:\')}
```

**Get command request details**  
The following command uses the `CommandId` to get the status of the command execution on both instances\. This example uses the `CommandId` that was returned in the previous command\. 

```
Get-SSMCommand `
	-CommandId $runPSCommand.CommandId
```

The status of the command in this example can be Success, Pending, or InProgress\.

**Get command information per instance**  
The following command uses the `CommandId` from the previous command to get the status of the command execution on a per instance basis\.

```
Get-SSMCommandInvocation `
	-CommandId $runPSCommand.CommandId
```

**Get command information with response data for a specific instance**  
The following command returns the output of the original `Send-SSMCommand` for a specific instance\. 

```
Get-SSMCommandInvocation `
	-CommandId $runPSCommand.CommandId `
	-Details $true `
	-InstanceId instance-ID | Select -ExpandProperty CommandPlugins
```

### Cancel a command<a name="walkthrough-powershell-run-script-cancel-command"></a>

The following command cancels the `Send-SSMCommand` for the `AWS-RunPowerShellScript` document\.

```
$cancelCommand = Send-SSMCommand `
	-InstanceIds @("instance-ID-1","instance-ID-2") `
	-DocumentName "AWS-RunPowerShellScript" `
	-Comment "Demo AWS-RunPowerShellScript with two instances" `
	-Parameter @{'commands'='Start-Sleep –Seconds 120; dir C:\'}

Stop-SSMCommand -CommandId $cancelCommand.CommandId
```

**Check the command status**  
The following command checks the status of the `Cancel` command\.

```
Get-SSMCommand `
	-CommandId $cancelCommand.CommandId
```

## Install an application using the AWS\-InstallApplication document<a name="walkthrough-powershell-install-application"></a>

Using Run Command and the `AWS-InstallApplication` document, you can install, repair, or uninstall applications on instances\. The command requires the path or address to an MSI\.

**Note**  
For information about rebooting servers and instances when using Run Command to call scripts, see [Rebooting managed instance from scripts](send-commands-reboot.md)\.

**View the description and available parameters**

```
Get-SSMDocumentDescription `
	-Name "AWS-InstallApplication"
```

**View more information about parameters**

```
Get-SSMDocumentDescription `
	-Name "AWS-InstallApplication" | Select -ExpandProperty Parameters
```

### Send a command using the AWS\-InstallApplication document<a name="walkthrough-powershell-install-application-send-command-aws-installapplication"></a>

The following command installs a version of Python on your instance in unattended mode, and logs the output to a local text file on your `C:` drive\.

```
$installAppCommand = Send-SSMCommand `
	-InstanceId instance-ID `
	-DocumentName "AWS-InstallApplication" `
	-Parameter @{'source'='https://www.python.org/ftp/python/2.7.9/python-2.7.9.msi'; 'parameters'='/norestart /quiet /log c:\pythoninstall.txt'}
```

**Get command information per instance**  
The following command uses the `CommandId` to get the status of the command execution\.

```
Get-SSMCommandInvocation `
	-CommandId $installAppCommand.CommandId `
	-Details $true
```

**Get command information with response data for a specific instance**  
The following command returns the results of the Python installation\.

```
Get-SSMCommandInvocation `
	-CommandId $installAppCommand.CommandId `
	-Details $true `
	-InstanceId instance-ID | Select -ExpandProperty CommandPlugins
```

## Install a PowerShell module using the AWS\-InstallPowerShellModule JSON document<a name="walkthrough-powershell-install-module"></a>

You can use Run Command to install PowerShell modules on an EC2 instance\. For more information about PowerShell modules, see [Windows PowerShell Modules](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_modules?view=powershell-6)\.

**View the description and available parameters**

```
Get-SSMDocumentDescription `
	-Name "AWS-InstallPowerShellModule"
```

**View more information about parameters**

```
Get-SSMDocumentDescription `
	-Name "AWS-InstallPowerShellModule" | Select -ExpandProperty Parameters
```

### Install a PowerShell module<a name="walkthrough-powershell-install-module-install"></a>

The following command downloads the EZOut\.zip file, installs it, and then runs an additional command to install XPS viewer\. Lastly, the output of this command is uploaded to an S3 bucket named "demo\-ssm\-output\-bucket"\. 

```
$installPSCommand = Send-SSMCommand `
	-InstanceId instance-ID `
	-DocumentName "AWS-InstallPowerShellModule" `
	-Parameter @{'source'='https://gallery.technet.microsoft.com/EZOut-33ae0fb7/file/110351/1/EZOut.zip';'commands'=@('Add-WindowsFeature -name XPS-Viewer -restart')} `
	-OutputS3BucketName demo-ssm-output-bucket
```

**Get command information per instance**  
The following command uses the `CommandId` to get the status of the command execution\. 

```
Get-SSMCommandInvocation `
	-CommandId $installPSCommand.CommandId `
	-Details $true
```

**Get command information with response data for the instance**  
The following command returns the output of the original `Send-SSMCommand` for the specific `CommandId`\. 

```
Get-SSMCommandInvocation `
	-CommandId $installPSCommand.CommandId `
	-Details $true | Select -ExpandProperty CommandPlugins
```

## Join an instance to a Domain using the AWS\-JoinDirectoryServiceDomain JSON document<a name="walkthrough-powershell-domain-join"></a>

Using Run Command, you can quickly join an instance to an AWS Directory Service domain\. Before executing this command you must [create a directory](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/create_directory.html)\. We also recommend that you learn more about the AWS Directory Service\. For more information, see [What Is AWS Directory Service?](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/)\.

Currently you can only join an instance to a domain\. You cannot remove an instance from a domain\.

**Note**  
For information about rebooting servers and instances when using Run Command to call scripts, see [Rebooting managed instance from scripts](send-commands-reboot.md)\.

**View the description and available parameters**

```
Get-SSMDocumentDescription `
	-Name "AWS-JoinDirectoryServiceDomain"
```

**View more information about parameters**

```
Get-SSMDocumentDescription `
	-Name "AWS-JoinDirectoryServiceDomain" | Select -ExpandProperty Parameters
```

### Join an instance to a domain<a name="walkthrough-powershell-domain-join-instance"></a>

The following command joins the instance to the given AWS Directory Service domain and uploads any generated output to the example S3 bucketS3 bucket\. 

```
$domainJoinCommand = Send-SSMCommand `
	-InstanceId instance-ID `
	-DocumentName "AWS-JoinDirectoryServiceDomain" `
	-Parameter @{'directoryId'='d-example01'; 'directoryName'='ssm.example.com'; 'dnsIpAddresses'=@('192.168.10.195', '192.168.20.97')} `
	-OutputS3BucketName demo-ssm-output-bucket
```

**Get command information per instance**  
The following command uses the `CommandId` to get the status of the command execution\. 

```
Get-SSMCommandInvocation `
	-CommandId $domainJoinCommand.CommandId `
	-Details $true
```

**Get command information with response data for the instance**  
This command returns the output of the original `Send-SSMCommand` for the specific `CommandId`\.

```
Get-SSMCommandInvocation `
	-CommandId $domainJoinCommand.CommandId `
	-Details $true | Select -ExpandProperty CommandPlugins
```

## Send Windows metrics to Amazon CloudWatch Logs using the AWS\-ConfigureCloudWatch document<a name="walkthrough-powershell-windows-metrics"></a>

You can send Windows Server messages in the application, system, security, and Event Tracing for Windows \(ETW\) logs to Amazon CloudWatch Logs\. When you enable logging for the first time, Systems Manager sends all logs generated within one \(1\) minute from the time that you start uploading logs for the application, system, security, and ETW logs\. Logs that occurred before this time are not included\. If you disable logging and then later re\-enable logging, Systems Manager sends logs from the time it left off\. For any custom log files and Internet Information Services \(IIS\) logs, Systems Manager reads the log files from the beginning\. In addition, Systems Manager can also send performance counter data to CloudWatch Logs\.

If you previously enabled CloudWatch integration in EC2Config, the Systems Manager settings override any settings stored locally on the instance in the C:\\Program Files\\Amazon\\EC2ConfigService\\Settings\\AWS\.EC2\.Windows\.CloudWatch\.json file\. For more information about using EC2Config to manage performance counters and logs on single instance, see [Sending Performance Counters to CloudWatch and Logs to CloudWatch Logs](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/send_logs_to_cwl.html)\.

**View the description and available parameters**

```
Get-SSMDocumentDescription `
	-Name "AWS-ConfigureCloudWatch"
```

**View more information about parameters**

```
Get-SSMDocumentDescription `
	-Name "AWS-ConfigureCloudWatch" | Select -ExpandProperty Parameters
```

### Send application logs to CloudWatch<a name="walkthrough-powershell-windows-metrics-send-logs-cloudwatch"></a>

The following command configures the instance and moves Windows Applications logs to CloudWatch\.

```
$cloudWatchCommand = Send-SSMCommand `
	-InstanceID instance-ID `
	-DocumentName "AWS-ConfigureCloudWatch" `
	-Parameter @{'properties'='{"engineConfiguration": {"PollInterval":"00:00:15", "Components":[{"Id":"ApplicationEventLog", "FullName":"AWS.EC2.Windows.CloudWatch.EventLog.EventLogInputComponent,AWS.EC2.Windows.CloudWatch", "Parameters":{"LogName":"Application", "Levels":"7"}},{"Id":"CloudWatch", "FullName":"AWS.EC2.Windows.CloudWatch.CloudWatchLogsOutput,AWS.EC2.Windows.CloudWatch", "Parameters":{"Region":"region", "LogGroup":"my-log-group", "LogStream":"instance-id"}}], "Flows":{"Flows":["ApplicationEventLog,CloudWatch"]}}}'}
```

**Get command information per instance**  
The following command uses the `CommandId` to get the status of the command execution\. 

```
Get-SSMCommandInvocation `
	-CommandId $cloudWatchCommand.CommandId `
	-Details $true
```

**Get command information with response data for a specific instance**  
The following command returns the results of the Amazon CloudWatch configuration\.

```
Get-SSMCommandInvocation `
	-CommandId $cloudWatchCommand.CommandId `
	-Details $true `
	-InstanceId instance-ID | Select -ExpandProperty CommandPlugins
```

### Send performance counters to CloudWatch using the AWS\-ConfigureCloudWatch document<a name="walkthrough-powershell-windows-metrics-send-performance-counters-cloudwatch"></a>

The following demonstration command uploads performance counters to CloudWatch\. For more information, see the *[Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)*\.

```
$cloudWatchMetricsCommand = Send-SSMCommand `
	-InstanceID instance-ID `
	-DocumentName "AWS-ConfigureCloudWatch" `
	-Parameter @{'properties'='{"engineConfiguration": {"PollInterval":"00:00:15", "Components":[{"Id":"PerformanceCounter", "FullName":"AWS.EC2.Windows.CloudWatch.PerformanceCounterComponent.PerformanceCounterInputComponent,AWS.EC2.Windows.CloudWatch", "Parameters":{"CategoryName":"Memory", "CounterName":"Available MBytes", "InstanceName":"", "MetricName":"AvailableMemory", "Unit":"Megabytes","DimensionName":"", "DimensionValue":""}},{"Id":"CloudWatch", "FullName":"AWS.EC2.Windows.CloudWatch.CloudWatch.CloudWatchOutputComponent,AWS.EC2.Windows.CloudWatch", "Parameters":{"AccessKey":"", "SecretKey":"","Region":"region", "NameSpace":"Windows-Default"}}], "Flows":{"Flows":["PerformanceCounter,CloudWatch"]}}}'}
```

## Update EC2Config using the AWS\-UpdateEC2Config document<a name="walkthrough-powershell-update-ec2config"></a>

Using Run Command and the `AWS-EC2ConfigUpdate` document, you can update the EC2Config service running on your Windows Server instances\. This command can update the EC2Config service to the latest version or a version you specify\.

**View the description and available parameters**

```
Get-SSMDocumentDescription `
	-Name "AWS-UpdateEC2Config"
```

**View more information about parameters**

```
Get-SSMDocumentDescription `
	-Name "AWS-UpdateEC2Config" | Select -ExpandProperty Parameters
```

### Update EC2Config to the latest version<a name="walkthrough-powershell-update-ec2config-latest-version"></a>

```
$ec2ConfigCommand = Send-SSMCommand `
	-InstanceId instance-ID `
	-DocumentName "AWS-UpdateEC2Config"
```

**Get command information with response data for the instance**  
This command returns the output of the specified command from the previous `Send-SSMCommand`:

```
Get-SSMCommandInvocation `
	-CommandId $ec2ConfigCommand.CommandId `
	-Details $true `
	-InstanceId instance-ID | Select -ExpandProperty CommandPlugins
```

### Update EC2Config to a specific version<a name="walkthrough-powershell-update-ec2config-specific-version"></a>

The following command will downgrade EC2Config to an older version:

```
Send-SSMCommand `
	-InstanceId instance-ID `
	-DocumentName "AWS-UpdateEC2Config" `
	-Parameter @{'version'='4.9.3519'; 'allowDowngrade'='true'}
```

## Enable/Disable Windows automatic update using the AWS\-ConfigureWindowsUpdate document<a name="walkthrough-powershell-enable-windows-update"></a>

Using Run Command and the `AWS-ConfigureWindowsUpdate` document, you can enable or disable automatic Windows updates on your Windows Server instances\. This command configures the Windows update agent to download and install Windows updates on the day and hour that you specify\. If an update requires a reboot, the computer reboots automatically 15 minutes after updates have been installed\. With this command you can also configure Windows update to check for updates but not install them\. The `AWS-ConfigureWindowsUpdate` document is compatible with Windows Server 2008, 2008 R2, 2012, 2012 R2, and 2016\.

**View the description and available parameters**

```
Get-SSMDocumentDescription `
	–Name "AWS-ConfigureWindowsUpdate"
```

**View more information about parameters**

```
Get-SSMDocumentDescription `
	-Name "AWS-ConfigureWindowsUpdate" | Select -ExpandProperty Parameters
```

### Enable Windows automatic update<a name="walkthrough-powershell-enable-windows-update-automatic"></a>

The following command configures Windows Update to automatically download and install updates daily at 10:00 pm\. 

```
$configureWindowsUpdateCommand = Send-SSMCommand `
	-InstanceId instance-ID `
	-DocumentName "AWS-ConfigureWindowsUpdate" `
	-Parameters @{'updateLevel'='InstallUpdatesAutomatically'; 'scheduledInstallDay'='Daily'; 'scheduledInstallTime'='22:00'}
```

**View command status for enabling Windows automatic update**  
The following command uses the `CommandId` to get the status of the command execution for enabling Windows Automatic Update\.

```
Get-SSMCommandInvocation `
	-Details $true `
	-CommandId $configureWindowsUpdateCommand.CommandId | Select -ExpandProperty CommandPlugins
```

### Disable Windows automatic update<a name="walkthrough-powershell-enable-windows-update-disable"></a>

The following command lowers the Windows Update notification level so the system checks for updates but does not automatically update the instance\.

```
$configureWindowsUpdateCommand = Send-SSMCommand `
	-InstanceId instance-ID `
	-DocumentName "AWS-ConfigureWindowsUpdate" `
	-Parameters @{'updateLevel'='NeverCheckForUpdates'}
```

**View command status for disabling Windows automatic update**  
The following command uses the `CommandId` to get the status of the command execution for disabling Windows automatic update\.

```
Get-SSMCommandInvocation `
	-Details $true `
	-CommandId $configureWindowsUpdateCommand.CommandId | Select -ExpandProperty CommandPlugins
```

## Manage Windows updates using Run Command<a name="walkthough-powershell-windows-updates"></a>

Using Run Command and the `AWS-InstallWindowsUpdates` document, you can manage updates for EC2 instances for Windows Server\. This command scans for or installs missing updates on your EC2 instances for Windows Server and optionally reboots following installation\. You can also specify the appropriate classifications and severity levels for updates to install in your environment\.

**Note**  
For information about rebooting servers and instances when using Run Command to call scripts, see [Rebooting managed instance from scripts](send-commands-reboot.md)\.

The following examples demonstrate how to perform the specified Windows Update management tasks\.

### Search for all missing Windows updates<a name="walkthough-powershell-windows-updates-search"></a>

```
Send-SSMCommand `
	-InstanceId instance-ID `
	-DocumentName "AWS-InstallWindowsUpdates" `
	-Parameters @{'Action'='Scan'}
```

### Install specific Windows updates<a name="walkthough-powershell-windows-updates-install-specific"></a>

```
Send-SSMCommand `
	-InstanceId instance-ID `
	-DocumentName "AWS-InstallWindowsUpdates" `
	-Parameters @{'Action'='Install';'IncludeKbs'='kb-ID-1,kb-ID-2,kb-ID-3';'AllowReboot'='True'}
```

### Install important missing Windows updates<a name="walkthough-powershell-windows-updates-install-missing"></a>

```
Send-SSMCommand `
	-InstanceId instance-ID `
	-DocumentName "AWS-InstallWindowsUpdates" `
	-Parameters @{'Action'='Install';'SeverityLevels'='Important';'AllowReboot'='True'}
```

### Install missing Windows updates with specific exclusions<a name="walkthough-powershell-windows-updates-install-exclusions"></a>

```
Send-SSMCommand `
	-InstanceId instance-ID `
	-DocumentName "AWS-InstallWindowsUpdates" `
	-Parameters @{'Action'='Install';'ExcludeKbs'='kb-ID-1,kb-ID-2';'AllowReboot'='True'}
```