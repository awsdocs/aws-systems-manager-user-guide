# Systems Manager Command document plugin reference<a name="ssm-plugins"></a>

This reference describes the plugins that you can specify in an AWS Systems Manager \(SSM\) Command document\. These plugins cannot be used in SSM Automation documents, which use Automation actions\. For information about AWS Systems Manager Automation actions, see [Systems Manager Automation actions reference](automation-actions.md)\.

Systems Manager determines the actions to perform on a managed instance by reading the contents of an SSM document\. Each document includes a code\-execution section\. Depending on the schema version of your document, this code\-execution section can include one or more plugins or steps\. For the purpose of this Help topic, plugins and steps are called *plugins*\. This section includes information about each of the Systems Manager plugins\. For more information about documents, including information about creating documents and the differences between schema versions, see [AWS Systems Manager documents](sysman-ssm-docs.md)\.

**Note**  
Some of the plugins described here run only on either Windows Server instances or Linux instances\. Platform dependencies are noted for each plugin\.

**Topics**
+ [aws:applications](#aws-applications)
+ [aws:cloudWatch](#aws-cloudWatch)
+ [aws:configureDocker](#aws-configuredocker)
+ [aws:configurePackage](#aws-configurepackage)
+ [aws:domainJoin](#aws-domainJoin)
+ [aws:downloadContent](#aws-downloadContent)
+ [aws:psModule](#aws-psModule)
+ [aws:refreshAssociation](#aws-refreshassociation)
+ [aws:runDockerAction](#aws-rundockeraction)
+ [aws:runDocument](#aws-rundocument)
+ [aws:runPowerShellScript](#aws-runPowerShellScript)
+ [aws:runShellScript](#aws-runShellScript)
+ [aws:softwareInventory](#aws-softwareinventory)
+ [aws:updateAgent](#aws-updateagent)
+ [aws:updateSsmAgent](#aws-updatessmagent)

## aws:applications<a name="aws-applications"></a>

Install, repair, or uninstall applications on an EC2 instance\. This plugin only runs on Windows Server operating systems\. For more information, see [AWS Systems Manager documents](sysman-ssm-docs.md)\.

### Syntax<a name="applications-syntax"></a>

#### Schema 2\.2<a name="applications-syntax-2.2"></a>

------
#### [ YAML ]

```
---
schemaVersion: '2.2'
description: aws:applications plugin
parameters:
  source:
    description: "(Required) Source of msi."
    type: String
mainSteps:
- action: aws:applications
  name: example
  inputs:
    action: Install
    source: "{{ source }}"
```

------
#### [ JSON ]

```
{
  "schemaVersion":"2.2",
  "description":"aws:applications",
  "parameters":{
    "source":{
    "description":"(Required) Source of msi.",
    "type":"String"
    }
  },
  "mainSteps":[
    {
      "action":"aws:applications",
      "name":"example",
      "inputs":{
        "action":"Install",
        "source":"{{ source }}"
      }
    }
  ]
}
```

------

#### Schema 1\.2<a name="applications-syntax-1.2"></a>

------
#### [ YAML ]

```
---
runtimeConfig:
  aws:applications:
    properties:
    - id: 0.aws:applications
      action: "{{ action }}"
      parameters: "{{ parameters }}"
      source: "{{ source }}"
      sourceHash: "{{ sourceHash }}"
```

------
#### [ JSON ]

```
{
   "runtimeConfig":{
      "aws:applications":{
         "properties":[
            {
               "id":"0.aws:applications",
               "action":"{{ action }}",
               "parameters":"{{ parameters }}",
               "source":"{{ source }}",
               "sourceHash":"{{ sourceHash }}"
            }
         ]
      }
   }
}
```

------

### Properties<a name="applications-properties"></a>

**action**  
The action to take\.  
Type: Enum  
Valid values: `Install` \| `Repair` \| `Uninstall`  
Required: Yes

**parameters**  
The parameters for the installer\.  
Type: String  
Required: No

**source**  
The URL of the `.msi` file for the application\.  
Type: String  
Required: Yes

**sourceHash**  
The SHA256 hash of the `.msi` file\.  
Type: String  
Required: No

## aws:cloudWatch<a name="aws-cloudWatch"></a>

Export data from Windows Server to Amazon CloudWatch or Amazon CloudWatch Logs and monitor the data using CloudWatch metrics\. This plugin only runs on Windows Server operating systems\. For more information about configuring CloudWatch integration with Amazon EC2, see [Sending Logs, Events, and Performance Counters to Amazon CloudWatch](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/send_logs_to_cwl.html)\. For more information about documents, see [AWS Systems Manager documents](sysman-ssm-docs.md)\.

**Important**  
This plugin has been deprecated\. The unified CloudWatch agent has replaced SSM Agent as the tool for sending log data to Amazon CloudWatch Logs\. We recommend using only the unified CloudWatch agent for your log collection processes\. For more information, see the following topics:  
[Sending instance logs to CloudWatch Logs \(CloudWatch agent\)](monitoring-cloudwatch-agent.md)
[Migrate Windows Server instance log collection to the CloudWatch agent](monitoring-cloudwatch-agent.md#monitoring-cloudwatch-agent-migrate)
[Collect metrics from Amazon Elastic Compute Cloud instances and on\-premises servers with the CloudWatch agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) in the *Amazon CloudWatch User Guide*

You can export and monitor the following data types:

**ApplicationEventLog**  
Sends application event log data to CloudWatch Logs\.

**CustomLogs**  
Sends any text\-based log file to Amazon CloudWatch Logs\. The CloudWatch plugin creates a fingerprint for log files\. The system then associates a data offset with each fingerprint\. The plugin uploads files when there are changes, records the offset, and associates the offset with a fingerprint\. This method is used to avoid a situation where a user enables the plugin, associates the service with a directory that contains a large number of files, and the system uploads all of the files\.  
Be aware that if your application truncates or attempts to clean logs during polling, any logs specified for `LogDirectoryPath` can lose entries\. If, for example, you want to limit log file size, create a new log file when that limit is reached, and then continue writing data to the new file\.

**ETW**  
Sends Event Tracing for Windows \(ETW\) data to CloudWatch Logs\.

**IIS**  
Sends IIS log data to CloudWatch Logs\.

**PerformanceCounter**  
Sends Windows performance counters to CloudWatch\. You can select different categories to upload to CloudWatch as metrics\. For each performance counter that you want to upload, create a **PerformanceCounter** section with a unique ID \(for example, "PerformanceCounter2", "PerformanceCounter3", and so on\) and configure its properties\.  
If the SSM Agent or the CloudWatch plugin is stopped, performance counter data is not logged in CloudWatch This behavior is different than custom logs or Windows Event logs\. Custom logs and Windows Event logs preserve performance counter data and upload it to CloudWatch after SSM Agent or the CloudWatch plugin is available\.

**SecurityEventLog**  
Sends security event log data to CloudWatch Logs\.

**SystemEventLog**  
Sends system event log data to CloudWatch Logs\.

You can define the following destinations for the data:

**CloudWatch**  
The destination where your performance counter metric data is sent\. You can add more sections with unique IDs \(for example, "CloudWatch2", CloudWatch3", and so on\), and specify a different Region for each new ID to send the same data to different locations\.

**CloudWatchLogs**  
The destination where your log data is sent\. You can add more sections with unique IDs \(for example, "CloudWatchLogs2", CloudWatchLogs3", and so on\), and specify a different Region for each new ID to send the same data to different locations\.

### Syntax<a name="cloudWatch-syntax"></a>

```
"runtimeConfig":{
        "aws:cloudWatch":{
            "settings":{
                "startType":"{{ status }}"
            },
            "properties":"{{ properties }}"
        }
    }
```

### Settings and properties<a name="cloudWatch-properties"></a>

**AccessKey**  
Your access key ID\. This property is required unless you launched your instance using an IAM role\. This property cannot be used with SSM\.  
Type: String  
Required: No

**CategoryName**  
The performance counter category from Performance Monitor\.  
Type: String  
Required: Yes

**CounterName**  
The name of the performance counter from Performance Monitor\.  
Type: String  
Required: Yes

**CultureName**  
The locale where the timestamp is logged\. If **CultureName** is blank, it defaults to the same locale currently used by your Windows Server instance\.  
Type: String  
Valid values: For a list of supported values, see [National Language Support \(NLS\)](https://msdn.microsoft.com/en-us/library/cc233982.aspx) on the Microsoft website\. Note that the **div**, **div\-MV**, **hu**, and **hu\-HU** values are not supported\.  
Required: No

**DimensionName**  
A dimension for your Amazon CloudWatch metric\. If you specify `DimensionName`, you must specify `DimensionValue`\. These parameters provide another view when listing metrics\. You can use the same dimension for multiple metrics so that you can view all metrics belonging to a specific dimension\.  
Type: String  
Required: No

**DimensionValue**  
A dimension value for your Amazon CloudWatch metric\.  
Type: String  
Required: No

**Encoding**  
The file encoding to use \(for example, UTF\-8\)\. Use the encoding name, not the display name\.  
Type: String  
Valid values: For a list of supported values, see [Encoding Class](http://msdn.microsoft.com/en-us/library/system.text.encoding.aspx) in the MSDN Library\.  
Required: Yes

**Filter**  
The prefix of log names\. Leave this parameter blank to monitor all files\.  
Type: String  
Valid values: For a list of supported values, see the [FileSystemWatcherFilter Property](http://msdn.microsoft.com/en-us/library/system.io.filesystemwatcher.filter.aspx) in the MSDN Library\.  
Required: No

**Flows**  
Each data type to upload, along with the destination for the data \(CloudWatch or CloudWatch Logs\)\. For example, to send a performance counter defined under `"Id": "PerformanceCounter"` to the CloudWatch destination defined under `"Id": "CloudWatch"`, enter **"PerformanceCounter,CloudWatch"**\. Similarly, to send the custom log, ETW log, and system log to the CloudWatch Logs destination defined under `"Id": "ETW"`, enter **"\(ETW\),CloudWatchLogs"**\. In addition, you can send the same performance counter or log file to more than one destination\. For example, to send the application log to two different destinations that you defined under `"Id": "CloudWatchLogs"` and `"Id": "CloudWatchLogs2"`, enter **"ApplicationEventLog,\(CloudWatchLogs, CloudWatchLogs2\)"**\.  
Type: String  
Valid values \(source\): `ApplicationEventLog` \| `CustomLogs` \| `ETW` \| `PerformanceCounter` \| `SystemEventLog` \| `SecurityEventLog`   
Valid values \(destination\): `CloudWatch` \| `CloudWatchLogs` \| `CloudWatch`*n* \| `CloudWatchLogs`*n*   
Required: Yes

**FullName**  
The full name of the component\.  
Type: String  
Required: Yes

**Id**  
Identifies the data source or destination\. This identifier must be unique within the configuration file\.  
Type: String  
Required: Yes

**InstanceName**  
The name of the performance counter instance\. Do not use an asterisk \(\*\) to indicate all instances because each performance counter component only supports one metric\. You can, however use **\_Total**\.  
Type: String  
Required: Yes

**Levels**  
The types of messages to send to Amazon CloudWatch\.  
Type: String  
Valid values:   
+ **1** \- Only error messages uploaded\.
+ **2** \- Only warning messages uploaded\.
+ **4** \- Only information messages uploaded\.
Note that you can add values together to include more than one type of message\. For example, **3** means that error messages \(**1**\) and warning messages \(**2**\) are included\. A value of **7** means that error messages \(**1**\), warning messages \(**2**\), and informational messages \(**4**\) are included\.  
Required: Yes  
Windows Security Logs should set Levels to 7\.

**LineCount**  
The number of lines in the header to identify the log file\. For example, IIS log files have virtually identical headers\. You could enter **3**, which would read the first three lines of the log file's header to identify it\. In IIS log files, the third line is the date and time stamp, which is different between log files\.  
Type: Integer  
Required: No

**LogDirectoryPath**  
For CustomLogs, the path where logs are stored on your EC2 instance\. For IIS logs, the folder where IIS logs are stored for an individual site \(for example, **C:\\\\inetpub\\\\logs\\\\LogFiles\\\\W3SVC*n***\)\. For IIS logs, only W3C log format is supported\. IIS, NCSA, and Custom formats are not supported\.   
Type: String  
Required: Yes

**LogGroup**  
The name for your log group\. This name is displayed on the **Log Groups** screen in the CloudWatch console\.  
Type: String  
Required: Yes

**LogName**  
The name of the log file\.  

1. To find the name of the log, in Event Viewer, in the navigation pane, click **Applications and Services Logs**\.

1. In the list of logs, right\-click the log you want to upload \(for example, Microsoft>Windows>Backup>Operational\), and then click **Create Custom View**\.

1. In the **Create Custom View** dialog box, click the **XML** tab\. The **LogName** is in the <Select Path=> tag \(for example, `Microsoft-Windows-Backup`\)\. Copy this text into the **LogName** parameter\.
Type: String  
Valid values: `Application` \| `Security` \| `System` \| `Microsoft-Windows-WinINet/Analytic`  
Required: Yes

**LogStream**  
The destination log stream\. If you use **\{instance\_id\}**, the default, the instance ID of this instance is used as the log stream name\.  
Type: String  
Valid values: `{instance_id}` \| `{hostname}` \| `{ip_address}` *<log\_stream\_name>*  
If you enter a log stream name that doesn't already exist, CloudWatch Logs automatically creates it for you\. You can use a literal string or predefined variables \(**\{instance\_id\}**, **\{hostname\}**, **\{ip\_address\}**, or a combination of all three to define a log stream name\.  
The log stream name specified in this parameter appears on the **Log Groups > Streams for *<YourLogStream>*** screen in the CloudWatch console\.  
Required: Yes

**MetricName**  
The CloudWatch metric that you want performance data to appear under\.  
Don't use special characters in the name\. If you do, the metric and associated alarms might not work\.
Type: String  
Required: Yes

**NameSpace**  
The metric namespace where you want performance counter data to be written\.  
Type: String  
Required: Yes

**PollInterval**  
How many seconds must elapse before new performance counter and log data is uploaded\.  
Type: Integer  
Valid values: Set this to 5 or more seconds\. Fifteen seconds \(00:00:15\) is recommended\.  
Required: Yes

**Region**  
The AWS Region where you want to send log data\. Although you can send performance counters to a different Region from where you send your log data, we recommend that you set this parameter to the same Region where your instance is running\.  
Type: String  
Valid values: Regions IDs of the AWS Regions supported by both Systems Manager and CloudWatch Logs, such as `us-east-2`, `eu-west-1`, and `ap-southeast-1`\. For lists of AWS Regions supported by each service, see [Amazon CloudWatch Logs Service Endpoints](url-aws-gen;cwl_region.html#cwl_region) and [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.   
Required: Yes

**SecretKey**  
Your secret access key\. This property is required unless you launched your instance using an IAM role\.  
Type: String  
Required: No

**startType**  
Enable or disable CloudWatch on the instance\.  
Type: String  
Valid values: `Enabled` \| `Disabled`  
Required: Yes

**TimestampFormat**  
The timestamp format you want to use\. For a list of supported values, see [Custom Date and Time Format Strings](http://msdn.microsoft.com/en-us/library/8kb3ddd4.aspx) in the MSDN Library\.  
Type: String  
Required: Yes

**TimeZoneKind**  
Provides time zone information when no time zone information is included in your log’s timestamp\. If this parameter is left blank and if your timestamp doesn’t include time zone information, CloudWatch Logs defaults to the local time zone\. This parameter is ignored if your timestamp already contains time zone information\.  
Type: String  
Valid values: `Local` \| `UTC`  
Required: No

**Unit**  
The appropriate unit of measure for the metric\.  
Type: String  
Valid values: Seconds \| Microseconds \| Milliseconds \| Bytes \| Kilobytes \| Megabytes \| Gigabytes \| Terabytes \| Bits \| Kilobits \| Megabits \| Gigabits \| Terabits \| Percent \| Count \| Bytes/Second \| Kilobytes/Second \| Megabytes/Second \| Gigabytes/Second \| Terabytes/Second \| Bits/Second \| Kilobits/Second \| Megabits/Second \| Gigabits/Second \| Terabits/Second \| Count/Second \| None  
Required: Yes

## aws:configureDocker<a name="aws-configuredocker"></a>

\(Schema version 2\.0 or later\) Configure an instance to work with containers and Docker\. This plugin is supported on Linux and Windows Server operating systems\. For more information, see [AWS Systems Manager documents](sysman-ssm-docs.md)\. 

### Syntax<a name="configuredocker-syntax"></a>

#### Schema 2\.2<a name="configuredocker-syntax-2.2"></a>

------
#### [ YAML ]

```
---
schemaVersion: '2.2'
description: aws:configureDocker
parameters:
  action:
    description: "(Required) The type of action to perform."
    type: String
    default: Install
    allowedValues:
    - Install
    - Uninstall
mainSteps:
- action: aws:configureDocker
  name: configureDocker
  inputs:
    action: "{{ action }}"
```

------
#### [ JSON ]

```
{
  "schemaVersion": "2.2",
  "description": "aws:configureDocker plugin",
  "parameters": {
    "action": {
      "description": "(Required) The type of action to perform.",
      "type": "String",
      "default": "Install",
      "allowedValues": [
        "Install",
        "Uninstall"
      ]
    }
  },
  "mainSteps": [
    {
      "action": "aws:configureDocker",
      "name": "configureDocker",
      "inputs": {
        "action": "{{ action }}"
      }
    }
  ]
}
```

------

### Inputs<a name="configuredocker-properties"></a>

**action**  
The type of action to perform\.  
Type: Enum  
Valid values: `Install` \| `Uninstall`  
Required: Yes

## aws:configurePackage<a name="aws-configurepackage"></a>

\(Schema version 2\.0 or later\) Install or uninstall a Distributor package\. You can install the latest version, default version, or a version of the package you specify\. Packages provided by AWS are also supported\. This plugin runs on Windows Server and Linux operating systems, but not all the available packages are supported on Linux operating systems\.

Available AWS packages for Windows Server include the following: AWSPVDriver, AWSNVMe, AwsEnaNetworkDriver, AwsVssComponents, AmazonCloudWatchAgent, and AWSSupport\-EC2Rescue\.

Available AWS packages for Linux operating systems include the following: AmazonCloudWatchAgent and AWSSupport\-EC2Rescue\.

For more information, see [AWS Systems Manager documents](sysman-ssm-docs.md)\.

### Syntax<a name="configurepackage-syntax"></a>

#### Schema 2\.2<a name="configurepackage-syntax-2.2"></a>

------
#### [ YAML ]

```
---
schemaVersion: '2.2'
description: aws:configurePackage
parameters:
  name:
    description: "(Required) The name of the AWS package to install or uninstall."
    type: String
  action:
    description: "(Required) The type of action to perform."
    type: String
    default: Install
    allowedValues:
    - Install
    - Uninstall
mainSteps:
- action: aws:configurePackage
  name: configurePackage
  inputs:
    name: "{{ name }}"
    action: "{{ action }}"
    additionalArguments: 
      SSM_parameter_store_arg: "{{ ssm:parameter_store_arg }}"
      SSM_custom_arg: "myValue"
```

------
#### [ JSON ]

```
{
  "schemaVersion": "2.2",
  "description": "aws:configurePackage",
  "parameters": {
    "name": {
      "description": "(Required) The name of the AWS package to install or uninstall.",
      "type": "String"
    },
    "action": {
      "description": "(Required) The type of action to perform.",
      "type": "String",
      "default": "Install",
      "allowedValues": [
        "Install",
        "Uninstall"
      ]
    }
  },
  "mainSteps": [
    {
      "action": "aws:configurePackage",
      "name": "configurePackage",
      "inputs": {
        "name": "{{ name }}",
        "action": "{{ action }}",
        "additionalArguments": {
          "SSM_parameter_store_arg": "{{ ssm:parameter_store_arg }}",
          "SSM_custom_arg": "myValue"
        }
      }
    }
  ]
}
```

------

### Inputs<a name="configurepackage-properties"></a>

**name**  
The name of the AWS package to install or uninstall\. Available packages include the following: AWSPVDriver, AwsEnaNetworkDriver, AwsVssComponents, and AmazonCloudWatchAgent\.  
Type: String  
Required: Yes

**action**  
Install or uninstall a package\.  
Type: Enum  
Valid values: `Install` \| `Uninstall`  
Required: Yes

**installationType**  
The type of installation to perform\. If you specify `Uninstall and reinstall`, the package is completely uninstalled, and then reinstalled\. The application is unavailable until the reinstallation completes\. If you specify `In-place update`, only new or changed files are added to the existing installation according you instructions you provide in an update script\. The application remains available throughout the update process\. The `In-place update` option is not supported for AWS\-published packages\. `Uninstall and reinstall` is the default value\.  
Type: Enum  
Valid values: `Uninstall and reinstall` \| `In-place update`  
Required: No

**additionalArguments**  
The additional parameters to provide to your install, uninstall, or update scripts\. Each parameter must be prefixed with `SSM_`\. You can reference a Parameter Store parameter in your additional arguments by using the convention `{{ssm:parameter-name}}`\. To use the additional parameter in your install, uninstall, or update script, you must reference the parameter as an environment variable using the syntax appropriate for the operating system\. For example, in PowerShell, you reference the `SSM_arg` argument as `$Env:SSM_arg`\. There is no limit to the number of arguments you define, but the additional argument input has a 4096 character limit\. This limit includes all of the keys and values you define\.  
Type: StringMap  
Required: No

**version**  
A specific version of the package to install or uninstall\. If installing, the system installs the latest published version, by default\. If uninstalling, the system uninstalls the currently installed version, by default\. If no installed version is found, the latest published version is downloaded, and the uninstall action is run\.  
Type: String  
Required: No

## aws:domainJoin<a name="aws-domainJoin"></a>

Join an EC2 instance to a domain\. This plugin only runs on Windows Server operating systems\. For more information, see [AWS Systems Manager documents](sysman-ssm-docs.md)\.

### Syntax<a name="domainJoin-syntax"></a>

#### Schema 2\.2<a name="domainJoin-syntax-2.2"></a>

------
#### [ YAML ]

```
---
schemaVersion: '2.2'
description: aws:domainJoin
parameters:
  directoryId:
    description: "(Required) The ID of the directory."
    type: String
  directoryName:
    description: "(Required) The name of the domain."
    type: String
mainSteps:
- action: aws:domainJoin
  name: domainJoin
  inputs:
    directoryId: "{{ directoryId }}"
    directoryName: "{{ directoryName }}"
```

------
#### [ JSON ]

```
{
  "schemaVersion": "2.2",
  "description": "aws:domainJoin",
  "parameters": {
    "directoryId": {
      "description": "(Required) The ID of the directory.",
      "type": "String"
    },
    "directoryName": {
      "description": "(Required) The name of the domain.",
      "type": "String"
    }
  },
  "mainSteps": [
    {
      "action": "aws:domainJoin",
      "name": "domainJoin",
      "inputs": {
        "directoryId": "{{ directoryId }}",
        "directoryName": "{{ directoryName }}"
      }
    }
  ]
}
```

------

#### Schema 1\.2<a name="domainJoin-syntax-1.2"></a>

------
#### [ YAML ]

```
---
runtimeConfig:
  aws:domainJoin:
    properties:
      directoryId: "{{ directoryId }}"
      directoryName: "{{ directoryName }}"
      directoryOU: "{{ directoryOU }}"
      dnsIpAddresses: "{{ dnsIpAddresses }}"
```

------
#### [ JSON ]

```
{
   "runtimeConfig":{
      "aws:domainJoin":{
         "properties":{
            "directoryId":"{{ directoryId }}",
            "directoryName":"{{ directoryName }}",
            "directoryOU":"{{ directoryOU }}",
            "dnsIpAddresses":"{{ dnsIpAddresses }}"
         }
      }
   }
}
```

------

### Properties<a name="domainJoin-properties"></a>

**directoryId**  
The ID of the directory\.  
Type: String  
Required: Yes  
Example: "directoryId": "d\-1234567890"

**directoryName**  
The name of the domain\.  
Type: String  
Required: Yes  
Example: "directoryName": "example\.com"

**directoryOU**  
The organizational unit \(OU\)\.  
Type: String  
Required: No  
Example: "directoryOU": "OU=test,DC=example,DC=com"

**dnsIpAddresses**  
The IP addresses of the DNS servers\.  
Type: Array  
Required: No  
Example: "dnsIpAddresses": \["198\.51\.100\.1","198\.51\.100\.2"\]

### Examples<a name="domainJoin-examples"></a>

For examples, see [Joining a Windows Server Instance to an AWS Directory Service Domain](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-join-aws-domain.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## aws:downloadContent<a name="aws-downloadContent"></a>

\(Schema version 2\.0 or later\) Download SSM documents and scripts from remote locations\. This plugin is supported on Linux and Windows Server operating systems\.

### Syntax<a name="downloadContent-syntax"></a>

#### Schema 2\.2<a name="downloadContent-syntax-2.2"></a>

------
#### [ YAML ]

```
---
schemaVersion: '2.2'
description: aws:downloadContent
parameters:
  sourceType:
    description: "(Required) The download source."
    type: String
  sourceInfo:
    description: "(Required) The information required to retrieve the content from
      the required source."
    type: StringMap
mainSteps:
- action: aws:downloadContent
  name: downloadContent
  inputs:
    sourceType: "{{ sourceType }}"
    sourceInfo: "{{ sourceInfo }}"
```

------
#### [ JSON ]

```
{
  "schemaVersion": "2.2",
  "description": "aws:downloadContent",
  "parameters": {
    "sourceType": {
    "description": "(Required) The download source.",
    "type": "String"
  },
  "sourceInfo": {
    "description": "(Required) The information required to retrieve the content from the required source.",
    "type": "StringMap"
    }
  },
  "mainSteps": [
    {
      "action": "aws:downloadContent",
      "name": "downloadContent",
      "inputs": {
        "sourceType":"{{ sourceType }}",
        "sourceInfo":"{{ sourceInfo }}"
      }
    }
  ]
}
```

------

### Inputs<a name="downloadContent-inputs"></a>

**sourceType**  
The download source\. Systems Manager currently supports the following source types for downloading scripts and SSM documents: `GitHub`, `S3`, and `SSMDocument`\.  
Type: String  
Required: Yes

**sourceInfo**  
The information required to retrieve the content from the required source\.  
Type: StringMap  
Required: Yes  
 **For sourceType GitHub, specify the following:**   
+ owner: The repository owner\.
+ repository: The name of the repository\.
+ path: The path to the file or directory you want to download\.
+ getOptions: Extra options to retrieve content from a branch other than master or from a specific commit in the repository\. getOptions can be omitted if you are using the latest commit in the master branch\.

  This parameter uses the following format:
  + branch:*branch\_name*

    The default is `master`\.

    `branch` parameter is required only if your SSM document is stored in a branch other than `master`\.
  + commitID:*commitID*

    The default is `head`\.

    To use the version of your SSM document in a commit other than the latest, specify the full commit ID\. For example:

    ```
    "getOptions": "commitID:bbc1ddb94...b76d3bEXAMPLE",
    ```
+ tokenInfo: The Systems Manager parameter \(a SecureString parameter\) where you store your GitHub access token information, in the format `{{ssm-secure:secure-string-token-name}}`\.
**Note**  
This `tokenInfo` field is the only SSM document plugin field that supports a SecureString parameter\. SecureString parameters are not supported for any other fields, nor for any other SSM document plugins\.

```
{
    "owner":"TestUser",
    "repository":"GitHubTest",
    "path":"scripts/python/test-script",
    "getOptions":"branch:master",
    "tokenInfo":"{{ssm-secure:secure-string-token}}"
}
```
 **For sourceType S3, specify the following:**   
+ path: The URL to the file or directory you want to download from Amazon S3\.

```
{
    "path": "https://s3.amazonaws.com/aws-executecommand-test/powershell/helloPowershell.ps1" 
}
```
 **For sourceType SSMDocument, specify *one* of the following:**   
+ name: The name and version of the document in the following format: `name:version`\. Version is optional\. 

  ```
  {
      "name": "Example-RunPowerShellScript:3" 
  }
  ```
+ name: The ARN for the document in the following format: arn:aws:ssm:*region*:*account\_id*:document/*document\_name*

  ```
  {
     "name":"arn:aws:ssm:us-east-2:3344556677:document/MySharedDoc"
  }
  ```

**destinationPath**  
An optional local path on the instance where you want to download the file\. If you don't specify a path, the content is downloaded to a path relative to your command ID\.  
Type: String  
Required: No

## aws:psModule<a name="aws-psModule"></a>

Install PowerShell modules on an EC2 instance\. This plugin only runs on Windows Server operating systems\. For more information, see [AWS Systems Manager documents](sysman-ssm-docs.md)\.

### Syntax<a name="psModule-syntax"></a>

#### Schema 2\.2<a name="psModule-syntax-2.2"></a>

------
#### [ YAML ]

```
---
schemaVersion: '2.2'
description: aws:psModule
parameters:
  source:
    description: "(Required) The URL or local path on the instance to the application
      .zip file."
    type: String
mainSteps:
- action: aws:psModule
  name: psModule
  inputs:
    source: "{{ source }}"
```

------
#### [ JSON ]

```
{
  "schemaVersion": "2.2",
  "description": "aws:psModule",
  "parameters": {
    "source": {
      "description": "(Required) The URL or local path on the instance to the application .zip file.",
      "type": "String"
    }
  },
  "mainSteps": [
    {
      "action": "aws:psModule",
      "name": "psModule",
      "inputs": {
        "source": "{{ source }}"
      }
    }
  ]
}
```

------

#### Schema 1\.2<a name="domainJoin-syntax-1.2"></a>

------
#### [ YAML ]

```
---
runtimeConfig:
  aws:psModule:
    properties:
    - runCommand: "{{ commands }}"
      source: "{{ source }}"
      sourceHash: "{{ sourceHash }}"
      workingDirectory: "{{ workingDirectory }}"
      timeoutSeconds: "{{ executionTimeout }}"
```

------
#### [ JSON ]

```
{
   "runtimeConfig":{
      "aws:psModule":{
         "properties":[
            {
               "runCommand":"{{ commands }}",
               "source":"{{ source }}",
               "sourceHash":"{{ sourceHash }}",
               "workingDirectory":"{{ workingDirectory }}",
               "timeoutSeconds":"{{ executionTimeout }}"
            }
         ]
      }
   }
}
```

------

### Properties<a name="psModule-properties"></a>

**runCommand**  
The PowerShell command to run after the module is installed\.  
Type: StringList  
Required: No

**source**  
The URL or local path on the instance to the application `.zip` file\.  
Type: String  
Required: Yes

**sourceHash**  
The SHA256 hash of the `.zip` file\.  
Type: String  
Required: No

**timeoutSeconds**  
The time in seconds for a command to be completed before it is considered to have failed\.  
Type: String  
Required: No

**workingDirectory**  
The path to the working directory on your instance\.  
Type: String  
Required: No

## aws:refreshAssociation<a name="aws-refreshassociation"></a>

\(Schema version 2\.0 or later\) Refresh \(force apply\) an association on demand\. This action will change the system state based on what is defined in the selected association or all associations bound to the targets\. This plugin runs on Linux and Microsoft Windows Server operating systems\. For more information, see [AWS Systems Manager documents](sysman-ssm-docs.md)\.

### Syntax<a name="refreshassociation-syntax"></a>

#### Schema 2\.2<a name="refreshassociation-syntax-2.2"></a>

------
#### [ YAML ]

```
---
schemaVersion: '2.2'
description: aws:refreshAssociation
parameters:
  associationIds:
    description: "(Optional) List of association IDs. If empty, all associations bound
      to the specified target are applied."
    type: StringList
mainSteps:
- action: aws:refreshAssociation
  name: refreshAssociation
  inputs:
    associationIds:
    - "{{ associationIds }}"
```

------
#### [ JSON ]

```
{
  "schemaVersion": "2.2",
  "description": "aws:refreshAssociation",
  "parameters": {
    "associationIds": {
      "description": "(Optional) List of association IDs. If empty, all associations bound to the specified target are applied.",
      "type": "StringList"
    }
  },
  "mainSteps": [
    {
      "action": "aws:refreshAssociation",
      "name": "refreshAssociation",
      "inputs": {
        "associationIds": [
          "{{ associationIds }}"
        ]
      }
    }
  ]
}
```

------

### Inputs<a name="refreshassociation-properties"></a>

**associationIds**  
List of association IDs\. If empty, all associations bound to the specified target are applied\.  
Type: StringList  
Required: No

## aws:runDockerAction<a name="aws-rundockeraction"></a>

\(Schema version 2\.0 or later\) Run Docker actions on containers\. This plugin runs on Linux and Microsoft Windows Server operating systems\. For more information, see [AWS Systems Manager documents](sysman-ssm-docs.md)\.

### Syntax<a name="rundockeraction-syntax"></a>

#### Schema 2\.2<a name="rundockeraction-syntax-2.2"></a>

------
#### [ YAML ]

```
---
mainSteps:
- action: aws:runDockerAction
  name: RunDockerAction
  inputs:
    action: "{{ action }}"
    container: "{{ container }}"
    image: "{{ image }}"
    memory: "{{ memory }}"
    cpuShares: "{{ cpuShares }}"
    volume: "{{ volume }}"
    cmd: "{{ cmd }}"
    env: "{{ env }}"
    user: "{{ user }}"
    publish: "{{ publish }}"
```

------
#### [ JSON ]

```
{
   "mainSteps":[
      {
         "action":"aws:runDockerAction",
         "name":"RunDockerAction",
         "inputs":{
            "action":"{{ action }}",
            "container":"{{ container }}",
            "image":"{{ image }}",
            "memory":"{{ memory }}",
            "cpuShares":"{{ cpuShares }}",
            "volume":"{{ volume }}",
            "cmd":"{{ cmd }}",
            "env":"{{ env }}",
            "user":"{{ user }}",
            "publish":"{{ publish }}"
         }
      }
   ]
}
```

------

### Inputs<a name="rundockeraction-properties"></a>

**action**  
The type of action to perform\.  
Type: String  
Required: Yes

**container**  
The Docker container ID\.  
Type: String  
Required: No

**image**  
The Docker image name\.  
Type: String  
Required: No

**cmd**  
The container command\.  
Type: String  
Required: No

**memory**  
The container memory limit\.  
Type: String  
Required: No

**cpuShares**  
The container CPU shares \(relative weight\)\.  
Type: String  
Required: No

**volume**  
The container volume mounts\.  
Type: StringList  
Required: No

**env**  
The container environment variables\.  
Type: String  
Required: No

**user**  
The container user name\.  
Type: String  
Required: No

**publish**  
The container published ports\.  
Type: String  
Required: No

## aws:runDocument<a name="aws-rundocument"></a>

\(Schema version 2\.0 or later\) Runs SSM documents stored in Systems Manager or on a local share\. You can use this plugin with the [aws:downloadContent](#aws-downloadContent) plugin to download an SSM document from a remote location to a local share, and then run it\. This plugin is supported on Linux and Windows Server operating systems\.

### Syntax<a name="rundocument-syntax"></a>

#### Schema 2\.2<a name="aws-rundocument-syntax-2.2"></a>

------
#### [ YAML ]

```
---
schemaVersion: '2.2'
description: aws:runDocument
parameters:
  documentType:
    description: "(Required) The document type to run."
    type: String
    allowedValues:
    - LocalPath
    - SSMDocument
mainSteps:
- action: aws:runDocument
  name: runDocument
  inputs:
    documentType: "{{ documentType }}"
```

------
#### [ JSON ]

```
{
  "schemaVersion": "2.2",
  "description": "aws:runDocument",
  "parameters": {
    "documentType": {
      "description": "(Required) The document type to run.",
      "type": "String",
      "allowedValues": [
        "LocalPath",
        "SSMDocument"
      ]
    }
  },
  "mainSteps": [
    {
      "action": "aws:runDocument",
      "name": "runDocument",
      "inputs": {
        "documentType": "{{ documentType }}"
      }
    }
  ]
}
```

------

### Inputs<a name="rundocument-properties"></a>

**documentType**  
The document type to run\. You can run local documents \(`LocalPath`\) or documents stored in Systems Manager \(`SSMDocument`\)\.  
Type: String  
Required: Yes

**documentPath**  
The path to the document\. If `documentType` is `LocalPath`, then specify the path to the document on the local share\. If `documentType` is `SSMDocument`, then specify the name of the document\.  
Type: String  
Required: No

**documentParameters**  
Parameters for the document\.  
Type: StringMap  
Required: No

## aws:runPowerShellScript<a name="aws-runPowerShellScript"></a>

Run PowerShell scripts or specify the path to a script to run\. This plugin runs on Microsoft Windows Server and Linux operating systems\. For more information, see [AWS Systems Manager documents](sysman-ssm-docs.md)\.

### Syntax<a name="runPowerShellScript-syntax"></a>

#### Schema 2\.2<a name="runPowerShellScript-syntax-2.2"></a>

------
#### [ YAML ]

```
---
schemaVersion: '2.2'
description: aws:runPowerShellScript
parameters:
  commands:
    type: String
    description: "(Required) The commands to run or the path to an existing script
      on the instance."
    default: Write-Host "Hello World"
mainSteps:
- action: aws:runPowerShellScript
  name: runPowerShellScript
  inputs:
    timeoutSeconds: '60'
    runCommand:
    - "{{ commands }}"
```

------
#### [ JSON ]

```
{
  "schemaVersion": "2.2",
  "description": "aws:runPowerShellScript",
  "parameters": {
    "commands": {
      "type": "String",
      "description": "(Required) The commands to run or the path to an existing script on the instance.",
      "default": "Write-Host \"Hello World\""
    }
  },
  "mainSteps": [
    {
      "action": "aws:runPowerShellScript",
      "name": "runPowerShellScript",
      "inputs": {
        "timeoutSeconds": "60",
        "runCommand": [
          "{{ commands }}"
        ]
      }
    }
  ]
}
```

------

#### Schema 1\.2<a name="runPowerShellScript-syntax-1.2"></a>

------
#### [ YAML ]

```
---
runtimeConfig:
  aws:runPowerShellScript:
    properties:
    - id: 0.aws:runPowerShellScript
      runCommand: "{{ commands }}"
      workingDirectory: "{{ workingDirectory }}"
      timeoutSeconds: "{{ executionTimeout }}"
```

------
#### [ JSON ]

```
{
   "runtimeConfig":{
      "aws:runPowerShellScript":{
         "properties":[
            {
               "id":"0.aws:runPowerShellScript",
               "runCommand":"{{ commands }}",
               "workingDirectory":"{{ workingDirectory }}",
               "timeoutSeconds":"{{ executionTimeout }}"
            }
         ]
      }
   }
}
```

------

### Properties<a name="runPowerShellScript-properties"></a>

**runCommand**  
Specify the commands to run or the path to an existing script on the instance\.  
Type: StringList  
Required: Yes

**timeoutSeconds**  
The time in seconds for a command to be completed before it is considered to have failed\. When the timeout is reached, Systems Manager stops the command execution\.  
Type: String  
Required: No

**workingDirectory**  
The path to the working directory on your instance\.  
Type: String  
Required: No

## aws:runShellScript<a name="aws-runShellScript"></a>

Run Linux shell scripts or specify the path to a script to run\. This plugin only runs on Linux operating systems\. For more information, see [AWS Systems Manager documents](sysman-ssm-docs.md)\.

### Syntax<a name="runShellScript-syntax"></a>

#### Schema 2\.2<a name="runShellScript-syntax-2.2"></a>

------
#### [ YAML ]

```
---
schemaVersion: '2.2'
description: aws:runShellScript
parameters:
  commands:
    type: String
    description: "(Required) The commands to run or the path to an existing script
      on the instance."
    default: echo Hello World
mainSteps:
- action: aws:runShellScript
  name: runShellScript
  inputs:
    timeoutSeconds: '60'
    runCommand:
    - "{{ commands }}"
```

------
#### [ JSON ]

```
{
  "schemaVersion": "2.2",
  "description": "aws:runShellScript",
  "parameters": {
    "commands": {
      "type": "String",
      "description": "(Required) The commands to run or the path to an existing script on the instance.",
      "default": "echo Hello World"
    }
  },
  "mainSteps": [
    {
      "action": "aws:runShellScript",
      "name": "runShellScript",
      "inputs": {
        "timeoutSeconds": "60",
        "runCommand": [
          "{{ commands }}"
        ]
      }
    }
  ]
}
```

------

#### Schema 1\.2<a name="runShellScript-syntax-1.2"></a>

------
#### [ YAML ]

```
---
runtimeConfig:
  aws:runShellScript:
    properties:
    - runCommand: "{{ commands }}"
      workingDirectory: "{{ workingDirectory }}"
      timeoutSeconds: "{{ executionTimeout }}"
```

------
#### [ JSON ]

```
{
   "runtimeConfig":{
      "aws:runShellScript":{
         "properties":[
            {
               "runCommand":"{{ commands }}",
               "workingDirectory":"{{ workingDirectory }}",
               "timeoutSeconds":"{{ executionTimeout }}"
            }
         ]
      }
   }
}
```

------

### Properties<a name="runShellScript-properties"></a>

**runCommand**  
Specify the commands to run or the path to an existing script on the instance\.  
Type: StringList  
Required: Yes

**timeoutSeconds**  
The time in seconds for a command to be completed before it is considered to have failed\. When the timeout is reached, Systems Manager stops the command execution\.  
Type: String  
Required: No

**workingDirectory**  
The path to the working directory on your instance\.  
Type: String  
Required: No

## aws:softwareInventory<a name="aws-softwareinventory"></a>

\(Schema version 2\.0 or later\) Gather metadata about applications, files, and configurations on your managed instances\. This plugin runs on Linux and Microsoft Windows Server operating systems\. When you configure inventory collection, you start by creating a Systems Manager State Manager association\. Systems Manager collects the inventory data when the association is run\. If you don't create the association first, and attempt to invoke the aws:softwareInventory plugin the system returns the following error:

```
The aws:softwareInventory plugin can only be invoked via ssm-associate.
```

Also note that an instance can have only have one inventory association configured at a time\. If you configure an instance with two or more associations, the inventory association doesn't run and no inventory data is collected\. For more information about collecting inventory, see [AWS Systems Manager Inventory](systems-manager-inventory.md)\.

### Syntax<a name="softwareinventory-syntax"></a>

#### Schema 2\.2<a name="softwareinventory-syntax-2.2"></a>

------
#### [ YAML ]

```
---
mainSteps:
- action: aws:softwareInventory
  name: collectSoftwareInventoryItems
  inputs:
    applications: "{{ applications }}"
    awsComponents: "{{ awsComponents }}"
    networkConfig: "{{ networkConfig }}"
    files: "{{ files }}"
    services: "{{ services }}"
    windowsRoles: "{{ windowsRoles }}"
    windowsRegistry: "{{ windowsRegistry}}"
    windowsUpdates: "{{ windowsUpdates }}"
    instanceDetailedInformation: "{{ instanceDetailedInformation }}"
    customInventory: "{{ customInventory }}"
```

------
#### [ JSON ]

```
{
   "mainSteps":[
      {
         "action":"aws:softwareInventory",
         "name":"collectSoftwareInventoryItems",
         "inputs":{
            "applications":"{{ applications }}",
            "awsComponents":"{{ awsComponents }}",
            "networkConfig":"{{ networkConfig }}",
            "files":"{{ files }}",
            "services":"{{ services }}",
            "windowsRoles":"{{ windowsRoles }}",
            "windowsRegistry":"{{ windowsRegistry}}",
            "windowsUpdates":"{{ windowsUpdates }}",
            "instanceDetailedInformation":"{{ instanceDetailedInformation }}",
            "customInventory":"{{ customInventory }}"
         }
      }
   ]
}
```

------

### Inputs<a name="softwareinventory-properties"></a>

**applications**  
\(Optional\) Collect metadata for installed applications\.  
Type: String  
Required: No

**awsComponents**  
\(Optional\) Collect metadata for AWS components like amazon\-ssm\-agent\.  
Type: String  
Required: No

**files**  
\(Optional, requires SSM Agent version 2\.2\.64\.0 or later\) Collect metadata for files, including file names, the time files were created, the time files were last modified and accessed, and file sizes, to name a few\. For more information about collecting file inventory, see [Working with file and Windows registry inventory](sysman-inventory-file-and-registry.md)\.  
Type: String  
Required: No

**networkConfig**  
\(Optional\) Collect metadata for network configurations\.  
Type: String  
Required: No

**windowsUpdates**  
\(Optional\) Collect metadata for all Windows updates\.  
Type: String  
Required: No

**instanceDetailedInformation**  
\(Optional\) Collect more instance information than is provided by the default inventory plugin \(aws:instanceInformation\), including CPU model, speed, and the number of cores, to name a few\.  
Type: String  
Required: No

**services**  
\(Optional, Windows OS only, requires SSM Agent version 2\.2\.64\.0 or later\) Collect metadata for service configurations\.  
Type: String  
Required: No

**windowsRegistry**  
\(Optional, Windows OS only, requires SSM Agent version 2\.2\.64\.0 or later\) Collect Windows Registry keys and values\. You can choose a key path and collect all keys and values recursively\. You can also collect a specific registry key and its value for a specific path\. Inventory collects the key path, name, type, and the value\. For more information about collecting Windows Registry inventory, see [Working with file and Windows registry inventory](sysman-inventory-file-and-registry.md)\.  
Type: String  
Required: No

**windowsRoles**  
\(Optional, Windows OS only, requires SSM Agent version 2\.2\.64\.0 or later\) Collect metadata for Microsoft Windows role configurations\.  
Type: String  
Required: No

**customInventory**  
\(Optional\) Collect custom inventory data\. For more information about custom inventory, see [Working with custom inventory](sysman-inventory-custom.md)  
Type: String  
Required: No

## aws:updateAgent<a name="aws-updateagent"></a>

Update the EC2Config service to the latest version or specify an older version\. This plugin only runs on Microsoft Windows Server operating systems\. For more information about the EC2Config service, see [Configuring a Windows Instance Using the EC2Config Service](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/UsingConfig_WinAMI.html)\. For more information about documents, see [AWS Systems Manager documents](sysman-ssm-docs.md)\.

### Syntax<a name="updateagent-syntax"></a>

#### Schema 2\.2<a name="updateagent-syntax-2.2"></a>

------
#### [ YAML ]

```
---
schemaVersion: '2.2'
description: aws:updateAgent
mainSteps:
- action: aws:updateAgent
  name: updateAgent
  inputs:
    agentName: Ec2Config
    source: https://s3.{Region}.amazonaws.com/aws-ssm-{Region}/manifest.json
```

------
#### [ JSON ]

```
{
  "schemaVersion": "2.2",
  "description": "aws:updateAgent",
  "mainSteps": [
    {
      "action": "aws:updateAgent",
      "name": "updateAgent",
      "inputs": {
        "agentName": "Ec2Config",
        "source": "https://s3.{Region}.amazonaws.com/aws-ssm-{Region}/manifest.json"
      }
    }
  ]
}
```

------

#### Schema 1\.2<a name="updateagent-syntax-1.2"></a>

------
#### [ YAML ]

```
---
runtimeConfig:
  aws:updateAgent:
    properties:
      agentName: Ec2Config
      source: https://s3.{Region}.amazonaws.com/aws-ssm-{Region}/manifest.json
      allowDowngrade: "{{ allowDowngrade }}"
      targetVersion: "{{ version }}"
```

------
#### [ JSON ]

```
{
   "runtimeConfig":{
      "aws:updateAgent":{
         "properties":{
            "agentName":"Ec2Config",
            "source":"https://s3.{Region}.amazonaws.com/aws-ssm-{Region}/manifest.json",
            "allowDowngrade":"{{ allowDowngrade }}",
            "targetVersion":"{{ version }}"
         }
      }
   }
}
```

------

### Properties<a name="updateagent-properties"></a>

**agentName**  
EC2Config\. This is the name of the agent that runs the EC2Config service\.  
Type: String  
Required: Yes

**allowDowngrade**  
Allow the EC2Config service to be downgraded to an earlier version\. If set to false, the service can be upgraded to newer versions only \(default\)\. If set to true, specify the earlier version\.   
Type: Boolean  
Required: No

**source**  
The location where Systems Manager copies the version of EC2Config to install\. You can't change this location\.  
Type: String  
Required: Yes

**targetVersion**  
A specific version of the EC2Config service to install\. If not specified, the service will be updated to the latest version\.  
Type: String  
Required: No

## aws:updateSsmAgent<a name="aws-updatessmagent"></a>

Update the SSM Agent to the latest version or specify an older version\. This plugin runs on Linux and Windows Server operating systems\. For more information, see [Working with SSM Agent](ssm-agent.md)\. For more information about documents, see [AWS Systems Manager documents](sysman-ssm-docs.md)\.

### Syntax<a name="updateSSMagent-syntax"></a>

#### Schema 2\.2<a name="updateaSSMgent-syntax-2.2"></a>

------
#### [ YAML ]

```
---
schemaVersion: '2.2'
description: aws:updateSsmAgent
parameters:
  allowDowngrade:
    default: 'false'
    description: "(Optional) Allow the Amazon SSM Agent service to be downgraded to
      an earlier version. If set to false, the service can be upgraded to newer versions
      only (default). If set to true, specify the earlier version."
    type: String
    allowedValues:
    - 'true'
    - 'false'
mainSteps:
- action: aws:updateSsmAgent
  name: updateSSMAgent
  inputs:
    agentName: amazon-ssm-agent
    source: https://s3.{Region}.amazonaws.com/amazon-ssm-{Region}/ssm-agent-manifest.json
    allowDowngrade: "{{ allowDowngrade }}"
```

------
#### [ JSON ]

```
{
  "schemaVersion": "2.2",
  "description": "aws:updateSsmAgent",
  "parameters": {
    "allowDowngrade": {
      "default": "false",
      "description": "(Required) Allow the Amazon SSM Agent service to be downgraded to an earlier version. If set to false, the service can be upgraded to newer versions only (default). If set to true, specify the earlier version.",
      "type": "String",
      "allowedValues": [
        "true",
        "false"
      ]
    }
  },
  "mainSteps": [
    {
      "action": "aws:updateSsmAgent",
      "name": "updateSSMAgent",
      "inputs": {
        "agentName": "amazon-ssm-agent",
        "source": "https://s3.{Region}.amazonaws.com/amazon-ssm-{Region}/ssm-agent-manifest.json",
        "allowDowngrade": "{{ allowDowngrade }}"
      }
    }
  ]
}
```

------

#### Schema 1\.2<a name="updateaSSMgent-syntax-1.2"></a>

------
#### [ YAML ]

```
---
runtimeConfig:
  aws:updateSsmAgent:
    properties:
    - agentName: amazon-ssm-agent
      source: https://s3.{Region}.amazonaws.com/aws-ssm-{Region}/manifest.json
      allowDowngrade: "{{ allowDowngrade }}"
```

------
#### [ JSON ]

```
{
   "runtimeConfig":{
      "aws:updateSsmAgent":{
         "properties":[
            {
               "agentName":"amazon-ssm-agent",
               "source":"https://s3.{Region}.amazonaws.com/aws-ssm-{Region}/manifest.json",
               "allowDowngrade":"{{ allowDowngrade }}"
            }
         ]
      }
   }
}
```

------

### Properties<a name="updateSSMagent-properties"></a>

**agentName**  
amazon\-ssm\-agent\. This is the name of the Systems Manager agent that processes requests and runs commands on the instance\.  
Type: String  
Required: Yes

**allowDowngrade**  
Allow the SSM Agent to be downgraded to an earlier version\. If set to false, the agent can be upgraded to newer versions only \(default\)\. If set to true, specify the earlier version\.   
Type: Boolean  
Required: Yes

**source**  
The location where Systems Manager copies the SSM Agent version to install\. You can't change this location\.  
Type: String  
Required: Yes

**targetVersion**  
A specific version of SSM Agent to install\. If not specified, the agent will be updated to the latest version\.  
Type: String  
Required: No