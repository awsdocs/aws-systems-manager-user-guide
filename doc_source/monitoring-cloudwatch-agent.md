# Sending node logs to CloudWatch Logs \(CloudWatch agent\)<a name="monitoring-cloudwatch-agent"></a>

You can configure and use the Amazon CloudWatch agent to collect metrics and logs from your nodes instead of using AWS Systems Manager Agent \(SSM Agent\) for these tasks\. The CloudWatch agent allows you to gather more metrics on EC2 instances than are available using SSM Agent\. In addition, you can gather metrics from on\-premises servers using the CloudWatch agent\. 

You can also store agent configuration settings in the Systems Manager Parameter Store for use with the CloudWatch agent\. Parameter Store is a capability of AWS Systems Manager\.

**Note**  
AWS Systems Manager supports migrating from SSM Agent to the CloudWatch agent for collecting logs and metrics on 64\-bit versions of Windows only\. For information about setting up the CloudWatch agent on other operating systems, and for complete information about using the CloudWatch agent, see [Collecting metrics and logs from Amazon EC2 instances and on\-premises servers with the CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) in the *[Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)*\.  
You can use the CloudWatch agent on other supported operating systems, but you won't be able to use Systems Manager to perform a tool migration\. 

SSM Agent writes information about executions, scheduled actions, errors, and health statuses to log files on each node\. Manually connecting to a node to view log files and troubleshoot an issue with SSM Agent is time\-consuming\. For more efficient node monitoring, you can configure either SSM Agent itself or the CloudWatch agent to send this log data to Amazon CloudWatch Logs\. 

**Important**  
The unified CloudWatch agent has replaced SSM Agent as the tool for sending log data to Amazon CloudWatch Logs\. Support for using SSM Agent to send log data will be deprecated in the near future\. We recommend using only the unified CloudWatch agent for your log collection processes\. For more information, see the following topics:  
[Sending node logs to CloudWatch Logs \(CloudWatch agent\)](#monitoring-cloudwatch-agent)
[Migrate Windows Server node log collection to the CloudWatch agent](#monitoring-cloudwatch-agent-migrate)
[Collecting metrics and logs from Amazon EC2 instances and on\-premises servers with the CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) in the *Amazon CloudWatch User Guide*

Using CloudWatch Logs, you can monitor log data in real time, search and filter log data by creating one or more metric filters, and archive and retrieve historical data when you need it\. For more information about CloudWatch Logs, see the *[Amazon CloudWatch Logs User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/)*\.

Configuring an agent to send log data to Amazon CloudWatch Logs provides the following benefits:
+ Centralized log file storage for all SSM Agent log files\.
+ Quicker access to files to investigate errors\.
+ Indefinite log file retention \(configurable\)\.
+ Logs can be maintained and accessed regardless of the status of the node\.
+ Access to other CloudWatch features such as metrics and alarms\.

For information about monitoring Session Manager activity, see [Auditing session activity](session-manager-auditing.md) and [Logging session activity](session-manager-logging.md)\.

## Migrate Windows Server node log collection to the CloudWatch agent<a name="monitoring-cloudwatch-agent-migrate"></a>

If you're using SSM Agent on supported Windows Server nodes to send SSM Agent log files to Amazon CloudWatch Logs, you can use Systems Manager to migrate from SSM Agent to the CloudWatch agent as your log collection tool, and migrate your configuration settings\.

The CloudWatch agent isn't supported on 32\-bit versions of Windows Server\.

For 64\-bit EC2 instances for Windows Server, you can perform the migration to the CloudWatch agent automatically or manually\. For on\-premises servers and virtual machines, the process must be performed manually\. 

**Note**  
During the migration process, the data sent to CloudWatch might be interrupted or duplicated\. Your metrics and log data will be recorded accurately again in CloudWatch after the migration is completed\.

We recommend testing the migration on a limited number of nodes before migrating an entire fleet to the CloudWatch agent\. After migration, if you prefer log collection with SSM Agent, you can return to using it instead\. 

**Important**  
In the following cases, you won’t be able to migrate to the CloudWatch agent using the steps described in this topic:  
The existing configuration for SSM Agent specifies multiple Regions\.
The existing configuration for SSM Agent specifies multiple sets of access/secret key credentials\.
In these cases, it will be necessary to turn off log collection in SSM Agent and install the CloudWatch agent without a migration process\. For more information, see the following topics in the *Amazon CloudWatch User Guide*:  
[Installing the CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-on-EC2-Instance.html)
[Installing the CloudWatch agent on on\-premises servers](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-on-premise.html)

**Before you begin**  
Before you begin a migration to the CloudWatch agent for log collection, ensure that the nodes on which you will perform the migration meet these requirements:
+ The OS is a 64\-bit version of Windows Server\.
+ SSM Agent 2\.2\.93\.0 or later is installed on the node\.
+ SSM Agent is configured for monitoring on the node\. 

**Topics**
+ [Automatically migrating to the CloudWatch agent](#monitoring-cloudwatch-agent-migrate-auto)
+ [Manually migrating to the CloudWatch agent](#monitoring-cloudwatch-agent-migrate-manual)

### Automatically migrating to the CloudWatch agent<a name="monitoring-cloudwatch-agent-migrate-auto"></a>

For EC2 instances for Windows Server only, you can use the AWS Systems Manager console or the AWS Command Line Interface \(AWS CLI\) to automatically migrate to the CloudWatch agent as your log collection tool\.

**Note**  
AWS Systems Manager supports migrating from SSM Agent to the CloudWatch agent for collecting logs and metrics on 64\-bit versions of Windows only\. For information about setting up the CloudWatch agent on other operating systems, and for complete information about using the CloudWatch agent, see [Collecting metrics and logs from Amazon EC2 instances and on\-premises servers with the CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) in the *[Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)*\.  
You can use the CloudWatch agent on other supported operating systems, but you won't be able to use Systems Manager to perform a tool migration\. 

After the migration succeeds, check your results in CloudWatch to ensure you're receiving the metrics, logs, or Windows event logs you expect\. If you're satisfied with the results, you can optionally [Store CloudWatch agent configuration settings in Parameter Store](#monitoring-cloudwatch-agent-store-config)\. If the migration isn't successful or the results aren't as expected, you can try [Rolling back to log collection with SSM Agent](#monitoring-cloudwatch-agent-roll-back)\.

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
After the migration, this entry maps to a domain, such as ip\-11\-1\-1\-11\.production\. ExampleCompany\.com\. To retain the local hostname value, specify `{local_hostname}` instead of `{hostname}`\.

**To automatically migrate to the CloudWatch agent \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 

1. In the **Command document** list, choose `AmazonCloudWatch-MigrateCloudWatchAgent`\.

1. For **Status**, choose **Enabled**\.

1. In the **Targets** section, choose the managed nodes on which you want to run this operation by specifying tags, selecting instances or edge devices manually, or specifying a resource group\.
**Note**  
If a managed node you expect to see isn't listed, see [Troubleshooting managed node availability](troubleshooting-managed-instances.md) for troubleshooting tips\.

1. For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of managed nodes on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed nodes or by specifying AWS resource groups, and you aren't certain how many managed nodes are targeted, then restrict the number of targets that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other managed nodes after it fails on either a number or a percentage of nodes\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Managed nodes still processing the command might also send errors\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Write command output to an S3 bucket** box\. Enter the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile \(for EC2 instances\) or IAM service role \(on\-premises machines\) assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md) or [Create an IAM service role for a hybrid environment](sysman-service-role.md)\. In addition, if the specified S3 bucket is in a different AWS account, make sure that the instance profile or IAM service role associated with the managed node has the necessary permissions to write to that bucket\.

1. In the **SNS notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.

1. Choose **Run**\.

**To automatically migrate to the CloudWatch agent \(AWS CLI\)**
+ Run the following command\.

  ```
  aws ssm send-command --document-name AmazonCloudWatch-MigrateCloudWatchAgent --targets Key=instanceids,Values=ID1,ID2,ID3
  ```

  *ID1*, *ID2*, and *ID3* represent the IDs of nodes you want to update, such as *i\-02573cafcfEXAMPLE*\.

### Manually migrating to the CloudWatch agent<a name="monitoring-cloudwatch-agent-migrate-manual"></a>

For on\-premises Windows Server nodes or EC2 instances for Windows Server, follow these steps to manually migrate log collection to the Amazon CloudWatch agent\. 

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
After the migration, this entry maps to a domain, such as ip\-11\-1\-1\-11\.production\.ExampleCompany\.com\. To retain the local hostname value, specify `{local_hostname}` instead of `{hostname}`\.

**One: To install the CloudWatch agent \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 

1. In the **Command document** list, choose `AWS-ConfigureAWSPackage`\.

1. For **Action**, choose `Install`\.

1. For **Name**, enter **AmazonCloudWatchAgent**\.

1. For **Version**, enter **latest** if it isn't already provided by default\.

1. In the **Targets** section, choose the managed nodes on which you want to run this operation by specifying tags, selecting instances or edge devices manually, or specifying a resource group\.
**Note**  
If a managed node you expect to see isn't listed, see [Troubleshooting managed node availability](troubleshooting-managed-instances.md) for troubleshooting tips\.

1. For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of managed nodes on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed nodes or by specifying AWS resource groups, and you aren't certain how many managed nodes are targeted, then restrict the number of targets that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other managed nodes after it fails on either a number or a percentage of nodes\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Managed nodes still processing the command might also send errors\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Write command output to an S3 bucket** box\. Enter the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile \(for EC2 instances\) or IAM service role \(on\-premises machines\) assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md) or [Create an IAM service role for a hybrid environment](sysman-service-role.md)\. In addition, if the specified S3 bucket is in a different AWS account, make sure that the instance profile or IAM service role associated with the managed node has the necessary permissions to write to that bucket\.

1. In the **SNS notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.

1. Choose **Run**\.

**Two: To update config data JSON format**
+ To update the JSON formatting of the existing config settings for the CloudWatch agent, use **Run Command**, a capability of AWS Systems Manager, or log in to the node directly with an RDP connection to run the following Windows PowerShell commands on the node, one at a time\.

  ```
  cd ${Env:ProgramFiles}\\Amazon\\AmazonCloudWatchAgent
  ```

  ```
  .\\amazon-cloudwatch-agent-config-wizard.exe --isNonInteractiveWindowsMigration
  ```

  *\{Env:ProgramFiles\}* represents the location where the Amazon directory containing the CloudWatch agent can be found, typically `C:\Program Files`\. 

**Three: To configure and start the CloudWatch agent \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 

1. In the **Command document** list, choose `AWS-RunPowerShellScript`\.

1. For **Commands**, enter the following two commands\.

   ```
   cd ${Env:ProgramFiles}\Amazon\AmazonCloudWatchAgent
   ```

   ```
   .\amazon-cloudwatch-agent-ctl.ps1 -a fetch-config -m ec2 -c file:config.json -s
   ```

   *\{Env:ProgramFiles\}* represents the location where the Amazon directory containing the CloudWatch agent can be found, typically `C:\Program Files`\. 

1. In the **Targets** section, choose the managed nodes on which you want to run this operation by specifying tags, selecting instances or edge devices manually, or specifying a resource group\.
**Note**  
If a managed node you expect to see isn't listed, see [Troubleshooting managed node availability](troubleshooting-managed-instances.md) for troubleshooting tips\.

1. For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of managed nodes on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed nodes or by specifying AWS resource groups, and you aren't certain how many managed nodes are targeted, then restrict the number of targets that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other managed nodes after it fails on either a number or a percentage of nodes\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Managed nodes still processing the command might also send errors\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Write command output to an S3 bucket** box\. Enter the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile \(for EC2 instances\) or IAM service role \(on\-premises machines\) assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md) or [Create an IAM service role for a hybrid environment](sysman-service-role.md)\. In addition, if the specified S3 bucket is in a different AWS account, make sure that the instance profile or IAM service role associated with the managed node has the necessary permissions to write to that bucket\.

1. In the **SNS notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.

1. Choose **Run**\.

**Four: To turn off log collection in SSM Agent \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 

1. In the **Command document** list, choose `AWS-ConfigureCloudWatch`\.

1. For **Status**, choose **Disabled**\.

1. In the **Targets** section, choose the managed nodes on which you want to run this operation by specifying tags, selecting instances or edge devices manually, or specifying a resource group\.
**Note**  
If a managed node you expect to see isn't listed, see [Troubleshooting managed node availability](troubleshooting-managed-instances.md) for troubleshooting tips\.

1. For **Status**, choose `Disabled`\.

1. For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of managed nodes on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed nodes or by specifying AWS resource groups, and you aren't certain how many managed nodes are targeted, then restrict the number of targets that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other managed nodes after it fails on either a number or a percentage of nodes\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Managed nodes still processing the command might also send errors\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Write command output to an S3 bucket** box\. Enter the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile \(for EC2 instances\) or IAM service role \(on\-premises machines\) assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md) or [Create an IAM service role for a hybrid environment](sysman-service-role.md)\. In addition, if the specified S3 bucket is in a different AWS account, make sure that the instance profile or IAM service role associated with the managed node has the necessary permissions to write to that bucket\.

1. In the **SNS notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.

1. Choose **Run**\.

   After completing these steps, check your logs in CloudWatch to verify you are receiving the metrics, logs, or Windows event logs you expect\. If the results are satisfactory, you can optionally [Store CloudWatch agent configuration settings in Parameter Store](#monitoring-cloudwatch-agent-store-config)\. If the migration isn't successful or the results aren't as expected, you can [Rolling back to log collection with SSM Agent](#monitoring-cloudwatch-agent-roll-back)\.

## Store CloudWatch agent configuration settings in Parameter Store<a name="monitoring-cloudwatch-agent-store-config"></a>

You can store the contents of an CloudWatch agent configuration file in Parameter Store\. By maintaining this configuration data in a parameter, multiple nodes can derive their configuration settings from it, and you avoid having to create or manually update configuration files on your nodes\. For example, you can use Run Command to write the contents of the parameter to configuration files on multiple nodes, or use State Manager, a capability of AWS Systems Manager, to help avoid configuration drift in the CloudWatch agent configuration settings across a fleet of nodes\.

When you run the CloudWatch agent configuration wizard, you can choose to let the wizard save your configuration settings as a new parameter in Parameter Store\. For information about running the CloudWatch agent configuration wizard, see [Create the CloudWatch agent configuration file with the wizard](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create-cloudwatch-agent-configuration-file-wizard.html) in the *Amazon CloudWatch User Guide*\.

If you ran the wizard but didn't choose the option to save the settings as a parameter, or you created the CloudWatch agent configuration file manually, you can retrieve the data to save as a parameter on your node in the following file\.

```
${Env:ProgramFiles}\Amazon\AmazonCloudWatchAgent\config.json
```

*\{Env:ProgramFiles\}* represents the location where the Amazon directory containing the CloudWatch agent can be found, typically `C:\Program Files`\. 

We recommend keeping a backup of the JSON in this file on a location other than the node itself\.

For information about creating a parameter, see [Creating Systems Manager parameters](sysman-paramstore-su-create.md)\.

For more information about the CloudWatch agent, see [Collecting metrics and logs from Amazon EC2 instances and on\-premises servers with the CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) in the *Amazon CloudWatch User Guide*\.

## Rolling back to log collection with SSM Agent<a name="monitoring-cloudwatch-agent-roll-back"></a>

If you want to return to using SSM Agent for log collection, follow these steps\.

**One: To retrieve config data from SSM Agent**

1. On the node where you want to return to collecting logs with the SSM Agent, locate the contents of the SSM Agent config file\. This JSON file is typically found in the following location:

   `${Env:ProgramFiles}\\Amazon\\SSM\\Plugins\\awsCloudWatch\\AWS.EC2.Windows.CloudWatch.json`

   *\{Env:ProgramFiles\}* represents the location where the `Amazon` directory can be found, typically `C:\Program Files`\. 

1. Copy this data into a text file for use in a later step\. 

   We recommend storing a backup of the JSON on a location other than the node itself\.

**Two: To uninstall the CloudWatch agent \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 

1. In the **Command document** list, choose `AWS-ConfigureAWSPackage`\.

1. For **Action**, choose **Uninstall**\.

1. For **Name**, enter **AmazonCloudWatchAgent**\.

1. In the **Targets** section, choose the managed nodes on which you want to run this operation by specifying tags, selecting instances or edge devices manually, or specifying a resource group\.
**Note**  
If a managed node you expect to see isn't listed, see [Troubleshooting managed node availability](troubleshooting-managed-instances.md) for troubleshooting tips\.

1. For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of managed nodes on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed nodes or by specifying AWS resource groups, and you aren't certain how many managed nodes are targeted, then restrict the number of targets that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other managed nodes after it fails on either a number or a percentage of nodes\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Managed nodes still processing the command might also send errors\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Write command output to an S3 bucket** box\. Enter the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile \(for EC2 instances\) or IAM service role \(on\-premises machines\) assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md) or [Create an IAM service role for a hybrid environment](sysman-service-role.md)\. In addition, if the specified S3 bucket is in a different AWS account, make sure that the instance profile or IAM service role associated with the managed node has the necessary permissions to write to that bucket\.

1. In the **SNS notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.

1. Choose **Run**\.

**Three: To turn log collection back on in SSM Agent \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 

1. In the **Command document** list, choose `AWS-ConfigureCloudWatch`\.

1. For **Status**, choose `Enabled`\.

1. For **Properties**, paste the contents of the old config data you saved to the text file\.

1. In the **Targets** section, choose the managed nodes on which you want to run this operation by specifying tags, selecting instances or edge devices manually, or specifying a resource group\.
**Note**  
If a managed node you expect to see isn't listed, see [Troubleshooting managed node availability](troubleshooting-managed-instances.md) for troubleshooting tips\.

1. For **Rate control**:
   + For **Concurrency**, specify either a number or a percentage of managed nodes on which to run the command at the same time\.
**Note**  
If you selected targets by specifying tags applied to managed nodes or by specifying AWS resource groups, and you aren't certain how many managed nodes are targeted, then restrict the number of targets that can run the document at the same time by specifying a percentage\.
   + For **Error threshold**, specify when to stop running the command on other managed nodes after it fails on either a number or a percentage of nodes\. For example, if you specify three errors, then Systems Manager stops sending the command when the fourth error is received\. Managed nodes still processing the command might also send errors\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Write command output to an S3 bucket** box\. Enter the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile \(for EC2 instances\) or IAM service role \(on\-premises machines\) assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md) or [Create an IAM service role for a hybrid environment](sysman-service-role.md)\. In addition, if the specified S3 bucket is in a different AWS account, make sure that the instance profile or IAM service role associated with the managed node has the necessary permissions to write to that bucket\.

1. In the **SNS notifications** section, if you want notifications sent about the status of the command execution, select the **Enable SNS notifications** check box\.

   For more information about configuring Amazon SNS notifications for Run Command, see [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.

1. Choose **Run**\.