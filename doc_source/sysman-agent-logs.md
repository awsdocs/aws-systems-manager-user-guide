# Viewing SSM Agent logs<a name="sysman-agent-logs"></a>

AWS Systems Manager Agent \(SSM Agent\) writes information about executions, commands, scheduled actions, errors, and health statuses to log files on each managed node\. You can view log files by manually connecting to a managed node, or you can automatically send logs to Amazon CloudWatch Logs\. For more information about sending logs to CloudWatch Logs, see [Monitoring AWS Systems Manager](monitoring.md)\.

You can view SSM Agent logs on managed nodes in the following locations\.

------
#### [ Linux and macOS ]

`/var/log/amazon/ssm/amazon-ssm-agent.log`

`/var/log/amazon/ssm/errors.log`

`/var/log/amazon/ssm/audits/amazon-ssm-agent-audit-YYYY-MM-DD`

------
#### [ Windows ]

`%PROGRAMDATA%\Amazon\SSM\Logs\amazon-ssm-agent.log`

`%PROGRAMDATA%\Amazon\SSM\Logs\errors.log`

`%PROGRAMDATA%\Amazon\SSM\Logs\audits\amazon-ssm-agent-audit-YYYY-MM-DD`

------

For Linux managed nodes, the SSM Agent `stderr` and `stdout` files are written to the following directory: `/var/lib/amazon/ssm`\.

For information about allowing SSM Agent debug logging, see [Allowing SSM Agent debug logging](#ssm-agent-debug-log-files)\.

For more information about `cihub/seelog` configuration, see the [Seelog Wiki](https://github.com/cihub/seelog/wiki) on GitHub\. For examples of `cihub/seelog` configurations, see the [cihub/seelog examples](https://github.com/cihub/seelog-examples) repository on GitHub\. 

## Allowing SSM Agent debug logging<a name="ssm-agent-debug-log-files"></a>

Use the following procedure to allow SSM Agent debug logging on your managed nodes\.

------
#### [ Linux and macOS ]

**To allow SSM Agent debug logging on Linux and macOS managed nodes**

1. Either use Session Manager, a capability of AWS Systems Manager, to connect to the managed node where you want to allow debug logging, or log on to the managed node\. For more information, see [Working with Session Manager](session-manager-working-with.md)\.

1. Locate the **seelog\.xml\.template** file\.

   **Linux**:

   On most Linux managed node types, the file is located in the directory `/etc/amazon/ssm/seelog.xml.template`\.

   On Ubuntu Server 20\.10 STR & 20\.04, 18\.04, and 16\.04 LTS, the file is located in the directory `/snap/amazon-ssm-agent/current/seelog.xml.template`\. Copy this file from the `/snap/amazon-ssm-agent/current/` directory to the `/etc/amazon/ssm/` directory before making any changes\.

   **macOS**: 

   On macOS instance types, the file is located in the directory `/opt/aws/ssm/seelog.xml.template`\.

1. Change the file name from `seelog.xml.template` to `seelog.xml`\.
**Note**  
On Ubuntu Server 20\.10 STR & 20\.04, 18\.04, and 16\.04 LTS, the file `seelog.xml` must be created in the directory `/etc/amazon/ssm/`\. You can create this directory and file by running the following commands\.   

   ```
   sudo mkdir -p /etc/amazon/ssm
   ```

   ```
   sudo cp -p /snap/amazon-ssm-agent/current/seelog.xml.template /etc/amazon/ssm/seelog.xml
   ```

1. Edit the `seelog.xml` file to change the default logging behavior\. Change the value of **minlevel** from **info** to **debug**, as shown in the following example\.

   `<seelog type="adaptive" mininterval="2000000" maxinterval="100000000" critmsgcount="500" minlevel="debug">`

1. \(Optional\) Restart SSM Agent using the following command\.

   Linux:

   ```
   sudo service amazon-ssm-agent restart
   ```

   macOS:

   ```
   sudo /opt/aws/ssm/bin/amazon-ssm-agent restart
   ```

------
#### [ Windows ]

**To allow SSM Agent debug logging on Windows Server managed nodes**

1. Either use Session Manager to connect to the managed node where you want to allow debug logging, or log on to the managed nodes\. For more information, see [Working with Session Manager](session-manager-working-with.md)\.

1. Make a copy of the **seelog\.xml\.template** file\. Change the name of the copy to **seelog\.xml**\. The file is located in the following directory\.

   `%PROGRAMFILES%\Amazon\SSM\seelog.xml.template`

1. Edit the `seelog.xml` file to change the default logging behavior\. Change the value of **minlevel** from **info** to **debug**, as shown in the following example\.

   `<seelog type="adaptive" mininterval="2000000" maxinterval="100000000" critmsgcount="500" minlevel="debug">`

1. Locate the following entry\.

   `filename="{{LOCALAPPDATA}}\Amazon\SSM\Logs\amazon-ssm-agent.log"`

   Change this entry to use the following path\.

   `filename="C:\ProgramData\Amazon\SSM\Logs\amazon-ssm-agent.log"`

1. Locate the following entry\.

   `filename="{{LOCALAPPDATA}}\Amazon\SSM\Logs\errors.log"`

   Change this entry to use the following path\.

   `filename="C:\ProgramData\Amazon\SSM\Logs\errors.log"`

1. Restart SSM Agent using the following PowerShell command in Administrator mode\.

   ```
   Restart-Service AmazonSSMAgent
   ```

------