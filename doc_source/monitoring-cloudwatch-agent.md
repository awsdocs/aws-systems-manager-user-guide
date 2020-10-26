# Sending instance logs to CloudWatch Logs \(CloudWatch agent\)<a name="monitoring-cloudwatch-agent"></a>

You can configure and use the Amazon CloudWatch agent to collect metrics and logs from your instances instead of using SSM Agent for these tasks\. The CloudWatch agent enables you to gather more metrics on EC2 instances than are available using SSM Agent\. In addition, you can gather metrics from on\-premises servers using the CloudWatch agent\. 

You can also store agent configuration settings in the Systems Manager Parameter Store for use with the CloudWatch agent\.

**Note**  
Currently, AWS Systems Manager supports migrating from SSM Agent to the CloudWatch agent for collecting logs and metrics on 64\-bit versions of Windows only\. For information about setting up the CloudWatch agent on other operating systems, and for complete information about using the CloudWatch agent, see [Collect metrics from Amazon Elastic Compute Cloud instances and on\-premises servers with the CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) in the *[Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)*\.  
You can use the CloudWatch agent on other supported operating systems, but you will not be able to use Systems Manager to perform a tool migration\. 

SSM Agent writes information about executions, scheduled actions, errors, and health statuses to log files on each instance\. Manually connecting to an instance to view log files and troubleshoot an issue with SSM Agent is time\-consuming\. For more efficient instance monitoring, you can configure either SSM Agent itself or the CloudWatch agent to send this log data to Amazon CloudWatch Logs\. 

**Important**  
The unified CloudWatch agent has replaced SSM Agent as the tool for sending log data to Amazon CloudWatch Logs\. Support for using SSM Agent to send log data will be deprecated in the near future\. We recommend using only the unified CloudWatch agent for your log collection processes\. For more information, see the following topics:  
[Sending instance logs to CloudWatch Logs \(CloudWatch agent\)](#monitoring-cloudwatch-agent)
[Migrate Windows Server instance log collection to the CloudWatch agent](#monitoring-cloudwatch-agent-migrate)
[Collect metrics from Amazon Elastic Compute Cloud instances and on\-premises servers with the CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) in the *Amazon CloudWatch User Guide*

Using CloudWatch Logs, you can monitor log data in real\-time, search and filter log data by creating one or more metric filters, and archive and retrieve historical data when you need it\. For more information about CloudWatch Logs, see the *[Amazon CloudWatch Logs User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)*\.

Configuring an agent to send log data to Amazon CloudWatch Logs provides the following benefits:
+ Centralized log file storage for all of your SSM Agent log files\.
+ Quicker access to files to investigate errors\.
+ Indefinite log file retention \(configurable\)\.
+ Logs can be maintained and accessed regardless of the status of the instance\.
+ Access to other CloudWatch features such as metrics and alarms\.

For information about monitoring Session Manager activity, see [Auditing and logging session activity](session-manager-logging-auditing.md)\.

## Migrate Windows Server instance log collection to the CloudWatch agent<a name="monitoring-cloudwatch-agent-migrate"></a>

If you are currently using SSM Agent on supported Windows Server instances to send SSM Agent log files to Amazon CloudWatch Logs, you can use Systems Manager to migrate from SSM Agent to the CloudWatch agent as your log collection tool, as well as migrate your configuration settings\.

The CloudWatch agent is not supported on 32\-bit versions of Windows Server\.

For 64\-bit EC2 instances for Windows Server, you can perform the migration to the CloudWatch agent automatically or manually\. For on\-premises servers and virtual machines, the process must be performed manually\. 

**Note**  
During the migration process, the data sent to CloudWatch may be interrupted or duplicated\. Your metrics and log data will be recorded accurately again in CloudWatch after the migration is completed\.

We recommend testing the migration on a limited number of instances before migrating an entire fleet to the CloudWatch agent\. After migration, if you prefer log collection with SSM Agent, you can return to using it instead\. 

**Important**  
In the following cases, you won’t be able to migrate to the CloudWatch agent using the steps described in this topic:  
The existing configuration for SSM Agent specifies multiple Regions\.
The existing configuration for SSM Agent specifies multiple sets of access/secret key credentials\.
In these cases, it will be necessary to disable log collection in SSM Agent and install the CloudWatch agent without a migration process\. For more information, see the following topics:  
[Install the CloudWatch agent on an EC2 instance](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-on-EC2-Instance.html)
[Install the CloudWatch agent on an on\-premises server](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-on-premise.html)

**Before You Begin**  
Before you begin a migration to the CloudWatch agent for log collection, ensure that the instances on which you will perform the migration meet these requirements:
+ The OS is a 64\-bit version of Windows Server\.
+ SSM Agent 2\.2\.93\.0 or later is installed on the instance\.
+ SSM Agent is configured for monitoring on the instance\. 

**Topics**
+ [Automatically migrating to the CloudWatch agent](#monitoring-cloudwatch-agent-migrate-auto)
+ [Manually migrating to the CloudWatch agent](#monitoring-cloudwatch-agent-migrate-manual)

### Automatically migrating to the CloudWatch agent<a name="monitoring-cloudwatch-agent-migrate-auto"></a>

For EC2 instances for Windows Server only, you can use the AWS Systems Manager console or the AWS CLI to automatically migrate to the CloudWatch agent as your log collection tool\.

**Note**  
Currently, AWS Systems Manager supports migrating from SSM Agent to the CloudWatch agent for collecting logs and metrics on 64\-bit versions of Windows only\. For information about setting up the CloudWatch agent on other operating systems, and for complete information about using the CloudWatch agent, see [Collect metrics from Amazon Elastic Compute Cloud instances and on\-premises servers with the CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) in the *[Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)*\.  
You can use the CloudWatch agent on other supported operating systems, but you will not be able to use Systems Manager to perform a tool migration\. 

After the migration succeeds, check your results in CloudWatch to ensure you are receiving the metrics, logs, or Windows event logs you expect\. If you are satisfied with the results, you can optionally [Store CloudWatch agent configuration settings in Parameter Store](#monitoring-cloudwatch-agent-store-config)\. If the migration is not successful or the results are not as expected, you can [Rolling back to log collection with SSM Agent](#monitoring-cloudwatch-agent-roll-back)\.

**Note**  
If you want to migrate a source configuration file that includes a `{hostname}` entry, then be aware that the `{hostname}` entry can change the value of the field after the migration is complete\. For example, say that the following `"LogStream": "{hostname}"` entry maps to a server named *MyLogServer001*\.  

```
{
"Id": "CloudWatchIISLogs",
"FullName": "AWS.EC2.Windows.CloudWatch.CloudWatchLogsOutput,AWS.EC2.Windows.CloudWatch",
"Parameters": {
"AccessKey": "",
"SecretKey": "",
"Region": "us-east-1",
"LogGroup": "Production-Windows-IIS",
"LogStream": "{hostname}"
     }
}
```
After the migration, this entry will map to a domain, such as ip\-11\-1\-1\-11\.production\.ExampleCompany\.com\. To retain the local hostname value, specify `{local_hostname}` instead of `{hostname}`\.

**To automatically migrate to the CloudWatch agent \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 

1. In the **Command document** list, choose AmazonCloudWatch\-MigrateCloudWatchAgent\.

1. In the **Targets** section, choose an option and select the instances to update\.

1. Choose **Run**\.

**To automatically migrate to the CloudWatch agent \(AWS CLI\)**
+ Run the following command:

  ```
  aws ssm send-command --document-name AmazonCloudWatch-MigrateCloudWatchAgent --targets Key=instanceids,Values=ID1,ID2,ID3
  ```

  *ID1*, *ID2*, and *ID3* represent the IDs of instances you want to update, such as *i\-02573cafcfEXAMPLE*\.

### Manually migrating to the CloudWatch agent<a name="monitoring-cloudwatch-agent-migrate-manual"></a>

For on\-premises Windows Server instances or EC2 instances for Windows Server, follow these steps to manually migrate log collection to the Amazon CloudWatch agent\. 

**Note**  
If you want to migrate a source configuration file that includes a `{hostname}` entry, then be aware that the `{hostname}` entry can change the value of the field after the migration is complete\. For example, say that the following `"LogStream": "{hostname}"` entry maps to a server named *MyLogServer001*\.  

```
{
"Id": "CloudWatchIISLogs",
"FullName": "AWS.EC2.Windows.CloudWatch.CloudWatchLogsOutput,AWS.EC2.Windows.CloudWatch",
"Parameters": {
"AccessKey": "",
"SecretKey": "",
"Region": "us-east-1",
"LogGroup": "Production-Windows-IIS",
"LogStream": "{hostname}"
     }
}
```
After the migration, this entry will map to a domain, such as ip\-11\-1\-1\-11\.production\.ExampleCompany\.com\. To retain the local hostname value, specify `{local_hostname}` instead of `{hostname}`\.

**One: To install the CloudWatch agent \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 

1. In the **Command document** list, choose AWS\-ConfigureAWSPackage\.

1. In the **Targets** section, choose an option and select the instances to update\.

1. In the **Action** list, choose Install\.

1. In **Name**, type **AmazonCloudWatchAgent**\.

1. In **Version**, type latest if it is not already provided by default\.

1. Choose **Run**\.

**Two: To update config data JSON format**
+ To update the JSON formatting of the existing config settings for the CloudWatch agent, use AWS Systems Manager **Run Command** or log into the instance directly with an RDP connection to run the following Windows PowerShell commands on the instance, one at a time:

  ```
  cd ${Env:ProgramFiles}\\Amazon\\AmazonCloudWatchAgent
  ```

  ```
  .\\amazon-cloudwatch-agent-config-wizard.exe --isNonInteractiveWindowsMigration
  ```

  *\{Env:ProgramFiles\}* represents the location where the Amazon folder containing the CloudWatch agent can be found, typically *C:\\Program Files*\. 

**Three: To configure and start the CloudWatch agent \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 

1. In the **Command document** list, choose AWS\-RunPowerShellScript\.

1. In the **Targets** section, choose an option and select the instances to update\.

1. In the **Commands** box, enter the following two commands: 

   ```
   cd ${Env:ProgramFiles}\Amazon\AmazonCloudWatchAgent
   ```

   ```
   .\amazon-cloudwatch-agent-ctl.ps1 -a fetch-config -m ec2 -c file:config.json -s
   ```

   *\{Env:ProgramFiles\}* represents the location where the Amazon folder containing the CloudWatch agent can be found, typically *C:\\Program Files*\. 

1. Choose **Run**\.

**Four: To disable log collection in SSM Agent \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 

1. In the **Command document** list, choose AWS\-ConfigurecloudWatch\.

1. In the **Targets** section, choose an option and select the instances to update\.

1. In the **Status** list, choose Disabled\.

1. Choose **Run**\.

   After completing these steps, check your logs in CloudWatch to ensure you are receiving the metrics, logs, or Windows event logs you expect\. If you are satisfied with the results, you can optionally [Store CloudWatch agent configuration settings in Parameter Store](#monitoring-cloudwatch-agent-store-config)\. If the migration is not successful or the results are not as expected, you can [Rolling back to log collection with SSM Agent](#monitoring-cloudwatch-agent-roll-back)\.

## Store CloudWatch agent configuration settings in Parameter Store<a name="monitoring-cloudwatch-agent-store-config"></a>

You can store the contents of an Amazon CloudWatch agent configuration file in Parameter Store\. By maintaining this configuration data in a parameter, multiple instances can derive their configuration settings from it, and you avoid having to create or manually update configuration files on your instances\. For example, you can use Run Command to write the contents of the parameter to configuration files on multiple instances, or use State Manager to help avoid configuration drift in the CloudWatch agent configuration settings across a fleet of instances\.

When you run the CloudWatch agent configuration wizard, you can choose to let the wizard save your configuration settings as a new parameter in Parameter Store\. For information about running the CloudWatch agent configuration wizard, see [Create the CloudWatch agent configuration file with the wizard](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create-cloudwatch-agent-configuration-file-wizard.html)\.

If you ran the wizard but did not choose the option to save the settings as a parameter, or you created the CloudWatch agent configuration file manually, you can retrieve the data to save as a parameter on your instance in the following file:

```
${Env:ProgramFiles}\Amazon\AmazonCloudWatchAgent\config.json
```

*\{Env:ProgramFiles\}* represents the location where the Amazon folder containing the CloudWatch agent can be found, typically *C:\\Program Files*\. 

We recommend keeping a backup of the JSON in this file on a location other than the instance itself\.

For information about creating a parameter, see [Creating Systems Manager parameters](sysman-paramstore-su-create.md)\.

For more information about the CloudWatch agent, see [Collect metrics from Amazon Elastic Compute Cloud instances and on\-premises servers with the CloudWatch agent ](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) in the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

## Rolling back to log collection with SSM Agent<a name="monitoring-cloudwatch-agent-roll-back"></a>

If you want to return to using SSM Agent for log collection, follow these steps\.

**One: To retrieve config data from SSM Agent**

1. On the instance where you want to return to collecting logs with the SSM Agent, locate the contents of the SSM Agent config file\. This JSON file is typically found in the following location:

   ```
   ${Env:ProgramFiles}\\Amazon\\SSM\\Plugins\\awsCloudWatch\\AWS.EC2.Windows.CloudWatch.json
   ```

   *\{Env:ProgramFiles\}* represents the location where the `Amazon` folder can be found, typically *C:\\Program Files*\. 

1. Copy this data into a text file for use in a later step\. 

   We recommend storing a backup of the JSON on a location other than the instance itself\.

**Two: To uninstall the CloudWatch agent \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 

1. In the **Command document** list, choose AWS\-ConfigureAWSPackage\.

1. In the **Targets** section, choose an option and select the instances to update\.

1. In the **Action** list, choose Uninstall\.

1. In **Name**, type **AmazonCloudWatchAgent**\.

1. Choose **Run**\.

**Three: To reenable log collection in SSM Agent \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 

1. In the **Command document** list, choose AWS\-ConfigureCloudWatch\.

1. In the **Targets** section, choose an option and select the instances to update\.

1. In the **Status** list, choose Enabled\.

1. In the **Properties** box, paste the contents of the old config data you saved to the text file\.

1. Choose **Run**\.