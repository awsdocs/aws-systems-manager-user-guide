# Send Logs to CloudWatch Logs \(CloudWatch Agent\)<a name="monitoring-cloudwatch-agent"></a>

You can configure and use the Amazon CloudWatch Agent to collect metrics and logs from your instances instead of using SSM Agent for these tasks\. The CloudWatch Agent enables you to gather more metrics on Amazon EC2 instances than are available using SSM Agent\. In addition, you can gather metrics from on\-premises servers using the CloudWatch Agent\. 

You can also store agent configuration settings in the Systems Manager Parameter Store for use with the CloudWatch Agent\.

**Note**  
Currently, AWS Systems Manager supports migrating from SSM Agent to the CloudWatch Agent for collecting logs and metrics on 64\-bit versions of Windows only\. For information about setting up the CloudWatch Agent on other operating systems, and for complete information about using the CloudWatch Agent, see [Collect Metrics from Amazon Elastic Compute Cloud Instances and On\-Premises Servers with the CloudWatch Agent](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) in the *[Amazon CloudWatch User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)*\.  
You can use the CloudWatch Agent on other supported operating systems, but you will not be able to use Systems Manager to perform a tool migration\. 

**Topics**
+ [Migrate Windows Server Instance Log Collection to the CloudWatch Agent](#monitoring-cloudwatch-agent-migrate)
+ [Store CloudWatch Agent Configuration Settings in Parameter Store](#monitoring-cloudwatch-agent-store-config)
+ [Roll Back to Log Collection with SSM Agent](#monitoring-cloudwatch-agent-roll-back)

## Migrate Windows Server Instance Log Collection to the CloudWatch Agent<a name="monitoring-cloudwatch-agent-migrate"></a>

If you are currently using SSM Agent on supported Windows Server instances to send SSM Agent log files to Amazon CloudWatch Logs, you can use Systems Manager to migrate from SSM Agent to the CloudWatch Agent as your log collection tool, as well as migrate your configuration settings\.

The CloudWatch Agent is not supported on 32\-bit versions of Windows Server\.

For 64\-bit Amazon EC2 Windows instances, you can perform the migration to the CloudWatch Agent automatically or manually\. For on\-premises instances, the process must be performed manually\. 

**Note**  
During the migration process, the data sent to CloudWatch may be interrupted or duplicated\. Your metrics and log data will be recorded accurately again in CloudWatch after the migration is completed\.

We recommend testing the migration on a limited number of instances before migrating an entire fleet to the CloudWatch Agent\. After migration, if you prefer log collection with SSM Agent, you can return to using it instead\. 

**Important**  
In the following cases, you won’t be able to migrate to the CloudWatch Agent using the steps described in this topic:  
The existing configuration for SSM Agent specifies multiple Regions\.
The existing configuration for SSM Agent specifies multiple sets of access/secret key credentials\.
In these cases, it will be necessary to disable log collection in SSM Agent and install the CloudWatch Agent without a migration process\. For more information, see the following topics:  
[Install the CloudWatch Agent on an Amazon EC2 Instance](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-on-EC2-Instance.html)
[Install the CloudWatch Agent on an On\-Premises Server](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-on-premise.html)

**Before You Begin**  
Before you begin a migration to the CloudWatch Agent for log collection, ensure that the instances on which you will perform the migration meet these requirements:
+ The OS is a 64\-bit version of Windows Server\.
+ SSM Agent 2\.2\.93\.0 or later is installed on the instance\.
+ SSM Agent is configured for monitoring on the instance\. 

**Topics**
+ [Automatically Migrate to the CloudWatch Agent](#monitoring-cloudwatch-agent-migrate-auto)
+ [Manually Migrate to the CloudWatch Agent](#monitoring-cloudwatch-agent-migrate-manual)

### Automatically Migrate to the CloudWatch Agent<a name="monitoring-cloudwatch-agent-migrate-auto"></a>

For Amazon EC2 Windows instances only, you can use the AWS Systems Manager console, the Amazon EC2 console, or the AWS CLI to automatically migrate to the CloudWatch Agent as your log collection tool\.

**Note**  
Currently, AWS Systems Manager supports migrating from SSM Agent to the CloudWatch Agent for collecting logs and metrics on 64\-bit versions of Windows only\. For information about setting up the CloudWatch Agent on other operating systems, and for complete information about using the CloudWatch Agent, see [Collect Metrics from Amazon Elastic Compute Cloud Instances and On\-Premises Servers with the CloudWatch Agent](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) in the *[Amazon CloudWatch User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)*\.  
You can use the CloudWatch Agent on other supported operating systems, but you will not be able to use Systems Manager to perform a tool migration\. 

After the migration succeeds, check your results in CloudWatch to ensure you are receiving the metrics, logs, or Windows event logs you expect\. If you are satisfied with the results, you can optionally [Store CloudWatch Agent Configuration Settings in Parameter Store](#monitoring-cloudwatch-agent-store-config)\. If the migration is not successful or the results are not as expected, you can [Roll Back to Log Collection with SSM Agent](#monitoring-cloudwatch-agent-roll-back)\.

**To automatically migrate to the CloudWatch Agent \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

   \-or\-

   Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.
**Note**  
If you are using the Amazon EC2 console, some field names and locations may differ slightly\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 
**Note**  
AWS Systems Manager only: If the AWS Systems Manager home page opens, scroll down and choose **Explore Run Command**\.

1. In the **Command document** list, choose AmazonCloudWatch\-MigrateCloudWatchAgent\.

1. In the **Targets** section, choose an option and select the instances to update\.

1. Choose **Run**\.

**To automatically migrate to the CloudWatch Agent \(AWS CLI\)**
+ Run the following command:

  ```
  aws ssm send-command --document-name AmazonCloudWatch-MigrateCloudWatchAgent --targets Key=instanceids,Values=ID1,ID2,ID3
  ```

  *ID1*, *ID2*, and *ID3* represent the IDs of instances you want to update, such as *i\-1234567890abcdef0*\.

### Manually Migrate to the CloudWatch Agent<a name="monitoring-cloudwatch-agent-migrate-manual"></a>

For on\-premises Windows instances or Amazon EC2 Windows instances, follow these steps to manually migrate log collection to the Amazon CloudWatch Agent\. 

**One: Install the CloudWatch Agent**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

   \-or\-

   Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.
**Note**  
If you are using the Amazon EC2 console, some field names and locations may differ slightly\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 
**Note**  
AWS Systems Manager only: If the AWS Systems Manager home page opens, scroll down and choose **Explore Run Command**\.

1. In the **Command document** list, choose AWS\-ConfigureAWSPackage\.

1. In the **Targets** section, choose an option and select the instances to update\.

1. In the **Action** list, choose Install\.

1. In **Name**, type **AmazonCloudWatchAgent**\.

1. In **Version**, type latest if it is not already provided by default\.

1. Choose **Run**\.

**Two: Update Config Data JSON Format**
+ To update the JSON formatting of the existing config settings for the CloudWatch Agent, use AWS Systems Manager **Run Command** or log into the instance directly with an RDP connection to run the following Windows PowerShell commands on the instance, one at a time:

  ```
  cd ${Env:ProgramFiles}\\Amazon\\AmazonCloudWatchAgent
  ```

  ```
  .\\amazon-cloudwatch-agent-config-wizard.exe --isNonInteractiveWindowsMigration
  ```

  *\{Env:ProgramFiles\}* represents the location where the Amazon folder containing the CloudWatch Agent can be found, typically *C:\\Program Files*\. 

**Three: Configure and Start the CloudWatch Agent**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

   \-or\-

   Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.
**Note**  
If you are using the Amazon EC2 console, some field names and locations may differ slightly\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 
**Note**  
AWS Systems Manager only: If the AWS Systems Manager home page opens, scroll down and choose **Explore Run Command**\.

1. In the **Command document** list, choose AWS\-RunPowerShellScript\.

1. In the **Targets** section, choose an option and select the instances to update\.

1. In the **Commands** box, enter the following two commands: 

   ```
   cd ${Env:ProgramFiles}\Amazon\AmazonCloudWatchAgent
   ```

   ```
   .\amazon-cloudwatch-agent-ctl.ps1 -a fetch-config -m ec2 -c file:config.json -s
   ```

   *\{Env:ProgramFiles\}* represents the location where the Amazon folder containing the CloudWatch Agent can be found, typically *C:\\Program Files*\. 

1. Choose **Run**\.

**Four: Disable Log Collection in SSM Agent**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

   \-or\-

   Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.
**Note**  
If you are using the Amazon EC2 console, some field names and locations may differ slightly\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 
**Note**  
AWS Systems Manager only: If the AWS Systems Manager home page opens, scroll down and choose **Explore Run Command**\.

1. In the **Command document** list, choose AWS\-ConfigurecloudWatch\.

1. In the **Targets** section, choose an option and select the instances to update\.

1. In the **Status** list, choose Disabled\.

1. Choose **Run**\.

   After completing these steps, check your logs in CloudWatch to ensure you are receiving the metrics, logs, or Windows event logs you expect\. If you are satisfied with the results, you can optionally [Store CloudWatch Agent Configuration Settings in Parameter Store](#monitoring-cloudwatch-agent-store-config)\. If the migration is not successful or the results are not as expected, you can [Roll Back to Log Collection with SSM Agent](#monitoring-cloudwatch-agent-roll-back)\.

## Store CloudWatch Agent Configuration Settings in Parameter Store<a name="monitoring-cloudwatch-agent-store-config"></a>

You can store the contents of an Amazon CloudWatch Agent configuration file in Parameter Store\. By maintaining this configuration data in a parameter, multiple instances can derive their configuration settings from it, and you avoid having to create or manually update configuration files on your instances\. For example, you can use Run Command to write the contents of the parameter to configuration files on multiple instances, or use State Manager to help avoid configuration drift in the CloudWatch Agent configuration settings across a fleet of instances\.

When you run the CloudWatch Agent configuration wizard, you can choose to let the wizard save your configuration settings as a new parameter in Parameter Store\. For information about running the CloudWatch Agent configuration wizard, see [Create the CloudWatch Agent Configuration File with the Wizard](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create-cloudwatch-agent-configuration-file-wizard.html)\.

If you ran the wizard but did not choose the option to save the settings as a parameter, or you created the CloudWatch Agent configuration file manually, you can retrieve the data to save as a parameter on your instance in the following file:

```
${Env:ProgramFiles}\Amazon\AmazonCloudWatchAgent\config.json
```

*\{Env:ProgramFiles\}* represents the location where the Amazon folder containing the CloudWatch Agent can be found, typically *C:\\Program Files*\. 

We recommend keeping a backup of the JSON in this file on a location other than the instance itself\.

For information about creating a parameter, see [Creating Systems Manager Parameters](sysman-paramstore-su-create.md)\.

For more information about the CloudWatch Agent, see [Collect Metrics from Amazon Elastic Compute Cloud Instances and On\-Premises Servers with the CloudWatch Agent ](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) in the [Amazon CloudWatch User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.

## Roll Back to Log Collection with SSM Agent<a name="monitoring-cloudwatch-agent-roll-back"></a>

If you want to return to using SSM Agent for log collection, follow these steps\.

**One: Retrieve Config Data from SSM Agent**

1. On the instance where you want to return to collecting logs with the SSM Agent, locate the contents of the SSM Agent config file\. This JSON file is typically found in the following location:

   ```
   ${Env:ProgramFiles}\\Amazon\\SSM\\Plugins\\awsCloudWatch\\AWS.EC2.Windows.CloudWatch.json
   ```

   *\{Env:ProgramFiles\}* represents the location where the `Amazon` folder can be found, typically *C:\\Program Files*\. 

1. Copy this data into a text file for use in a later step\. 

   We recommend storing a backup of the JSON on a location other than the instance itself\.

**Two: Uninstall the CloudWatch Agent**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

   \-or\-

   Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.
**Note**  
If you are using the Amazon EC2 console, some field names and locations may differ slightly\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 
**Note**  
AWS Systems Manager only: If the AWS Systems Manager home page opens, scroll down and choose **Explore Run Command**\.

1. In the **Command document** list, choose AWS\-ConfigureAWSPackage\.

1. In the **Targets** section, choose an option and select the instances to update\.

1. In the **Action** list, choose Uninstall\.

1. In **Name**, type **AmazonCloudWatchAgent**\.

1. Choose **Run**\.

**Three: Reenable Log Collection in SSM Agent**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

   \-or\-

   Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.
**Note**  
If you are using the Amazon EC2 console, some field names and locations may differ slightly\.

1. In the navigation pane, choose **Run Command**, and then choose **Run command**\. 
**Note**  
AWS Systems Manager only: If the AWS Systems Manager home page opens, scroll down and choose **Explore Run Command**\.

1. In the **Command document** list, choose AWS\-ConfigureCloudWatch\.

1. In the **Targets** section, choose an option and select the instances to update\.

1. In the **Status** list, choose Enabled\.

1. In the **Properties** box \(AWS Systems Manager console\) or **Parameters** box \(Amazon EC2 console\), paste the contents of the old config data you saved to the text file\.

1. Choose **Run**\.