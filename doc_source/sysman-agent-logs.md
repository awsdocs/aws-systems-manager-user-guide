# View SSM Agent Logs<a name="sysman-agent-logs"></a>

SSM Agent writes information about executions, scheduled actions, errors, and health statuses to log files on each instance\. You can view log files by manually connecting to an instance, or you can automatically send logs to Amazon CloudWatch Logs\. For more information about sending logs to CloudWatch, see [Monitoring AWS Systems Manager](monitoring.md)\.

You can view SSM Agent logs on instances in the following locations\.

------
#### [ Linux ]

`/var/log/amazon/ssm/amazon-ssm-agent.log`

`/var/log/amazon/ssm/errors.log`

------
#### [ Windows ]

`%PROGRAMDATA%\Amazon\SSM\Logs\amazon-ssm-agent.log`

`%PROGRAMDATA%\Amazon\SSM\Logs\errors.log`

------

For Linux instances, the SSM Agent `sterr` and `stdout` files are written to the following directory: /var/lib/amazon/ssm\.

For information about enabling SSM Agent debug logging, see [Enable SSM Agent Debug Logging](#ssm-agent-debug-log-files)\.

For more information about `cihub/seelog` configuration, see the [Seelog Wiki](https://github.com/cihub/seelog/wiki) on GitHub\. For examples of `cihub/seelog` configurations, see the [cihub/seelog examples](https://github.com/cihub/seelog-examples) repository on GitHub\. 

## Enable SSM Agent Debug Logging<a name="ssm-agent-debug-log-files"></a>

Use the following procedure to enable SSM Agent debug logging on Windows Server and Linux managed instances\.

------
#### [ Linux ]

**To enable SSM Agent debug logging on Linux instances**

1. Either use Systems Manager Session Manager to connect to the instance where you want to enable debug logging, or log on to the managed instance\. For more information, see [Working with Session Manager](session-manager-working-with.md)\.

1. Make a copy of the **seelog\.xml\.template** file\. Change the name of the copy to **seelog\.xml**\. The file is located in the following directory\.

   ```
   /etc/amazon/ssm/seelog.xml.template
   ```

1. Edit the `seelog.xml` file to change the default logging behavior\. Change the value of **minlevel** from **info** to **debug**, as shown in the following example\.

   ```
   <seelog type="adaptive" mininterval="2000000" maxinterval="100000000" critmsgcount="500" minlevel="debug">
   ```

1. Restart the SSM Agent using the following command\.

   ```
   sudo amazon-ssm-agent restart
   ```

------
#### [ Windows ]

**To enable SSM Agent debug logging on Windows instances**

1. Either use Systems Manager Session Manager to connect to the instance where you want to enable debug logging, or log on to the managed instance\. For more information, see [Working with Session Manager](session-manager-working-with.md)\.

1. Make a copy of the **seelog\.xml\.template** file\. Change the name of the copy to **seelog\.xml**\. The file is located in the following directory\.

   ```
   %PROGRAMFILES%\Amazon\SSM\seelog.xml.template
   ```

1. Edit the `seelog.xml` file to change the default logging behavior\. Change the value of **minlevel** from **info** to **debug**, as shown in the following example\.

   ```
   <seelog type="adaptive" mininterval="2000000" maxinterval="100000000" critmsgcount="500" minlevel="debug">
   ```

1. Locate the following entry:

   ```
   filename="{{LOCALAPPDATA}}\Amazon\SSM\Logs\amazon-ssm-agent.log"
   ```

   Change this entry to use the following path:

   ```
   filename="C:\ProgramData\Amazon\SSM\Logs\amazon-ssm-agent.log"
   ```

1. Locate the following entry:

   ```
   filename="{{LOCALAPPDATA}}\Amazon\SSM\Logs\errors.log"
   ```

   Change this entry to use the following path:

   ```
   filename="C:\ProgramData\Amazon\SSM\Logs\errors.log"
   ```

1. Restart the SSM Agent using the following command\.

   ```
    Restart-Service AmazonSSMAgent
   ```

------