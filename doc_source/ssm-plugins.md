# SSM Document Plugin Reference<a name="ssm-plugins"></a>

This reference describes the actions, or plugins, that you can specify in an AWS Systems Manager \(SSM\) document\. This reference does not include information about AWS Systems Manager Automation document plugins\. For information about Automation document plugins, see [Systems Manager Automation Document Reference](automation-actions.md)\.

Systems Manager determines the actions to perform on a managed instance by reading the contents of a Systems Manager document\. Each document includes a code\-execution section\. Depending on the schema version of your document, this code\-execution section can include one or more plugins or steps\. For the purpose of this Help topic, plugins and steps are called *plugins*\. This section includes information about each of the Systems Manager plugins\. For more information about documents, including information about creating documents and the differences between schema versions, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

**Note**  
Some of the plugins described here run only on either Windows Server instances or Linux instances\. Platform dependencies are noted for each plugin\.

**Topics**
+ [Top\-level Elements](#top-level)
+ [`type` Examples](#top-level-properties-type)
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
+ [aws:updateSSMAgent](#aws-updatessmagent)

## Top\-level Elements<a name="top-level"></a>

The top\-level elements are common for all Systems Manager documents\. Top\-level elements provide the structure of the Systems Manager document\.

### Properties<a name="top-level-properties"></a>

**schemaVersion**  
The version of the schema\.  
Type: Version  
Required: Yes

**description**  
A description of the configuration\.  
Type: String  
Required: No

**parameters**  
`parameters` is a structure that contains one or more parameters to execute when processing the document\. You can specify parameters at runtime, in a document, or by using Systems Manager Parameter Store\. For more information, see [AWS Systems Manager Parameter Store](systems-manager-paramstore.md)\.  
Type: Structure  
The `parameters` structure accepts the following fields and values:  
+ `type`: \(Required\) Allowed values include the following: `String`, `StringList`, `Boolean`, `Integer`, `MapList`, and `StringMap`\. To view examples of each type, see [`type` Examples](#top-level-properties-type) in the next section\.
+ `description`: \(Optional\) A description of the parameter\.
+ `default`: \(Optional\) The default value of the parameter or a reference to a parameter in Parameter Store\.
+ `allowedValues`: \(Optional\) Allowed values for the parameter\.
+ `allowedPattern`: \(Optional\) The regular expression the parameter must match\.
+ `displayType`: \(Optional\) Used to display either a `textfield` or a `textarea` in the AWS console\. `textfield` is a single\-line text box\. `textarea` is a multi\-line text area\.
+ `minItems`: \(Optional\) The minimum number of items allowed\.
+ `maxItems`: \(Optional\) The maximum number of items allowed\.
+ `minChars`: \(Optional\) The minimum number of parameter characters allowed\.
+ `maxChars`: \(Optional\) The maximum number of parameter characters allowed\.

**runtimeConfig**  
\(Schema version 1\.2 only\) The configuration for the instance as applied by one or more Systems Manager plugins\. Plugins are not guaranteed to run in sequence\.   
Type: Dictionary<string,PluginConfiguration>  
Required: No

**mainSteps**  
\(Schema version 0\.3, 2\.0, and 2\.2 only\) The configuration for the instance as applied by one or more Systems Manager plugins\. Plugins are organized as *actions* within steps\. Steps execute in sequential order as listed in the document\.   
Type: Dictionary<string,PluginConfiguration>  
Required: No

## `type` Examples<a name="top-level-properties-type"></a>

This section includes examples of each parameter `type`\.


****  

| `type` | Description | Example | Example use case | 
| --- | --- | --- | --- | 
|  String  |  A sequence of zero or more Unicode characters wrapped in double quotes\. Use backslashes to escape\.   |  "i\-1234567890abcdef0"  |  <pre>"InstanceId":{<br />"type":"String",<br />"description":"(Required) The target EC2 instance ID."<br />}</pre>  | 
|  StringList  |  A list of `String` items separated by commas  |  \["cd \~", "pwd"\]  |  <pre>"commands":{<br />"type":"StringList",<br />"description":"(Required) Specify a shell script or a command to run.",<br />"minItems":1,<br />"displayType":"textarea"<br />},<br /></pre>  | 
|  Boolean  |  Accepts only `true` or `false`\. Does not accept "true" or 0\.  |  true  |  <pre>"canRun": {<br />"type": "Boolean",<br />"description": "",<br />"default": true,<br />}<br /></pre>  | 
|  Integer  |  Integral numbers\. Doesn't accept decimal numbers, for example 3\.14159, or numbers wrapped in double quotes, for example "3"\.  |  39 or \-5  |  <pre>"timeout": {<br />"type": "Integer",<br />"description": "The type of action to perform.",<br />"default": 100    <br />}<br /></pre>  | 
|  StringMap  |  A mapping of keys to values\. A key can only be a string\. For example: \{ "type": "object" \}  |  <pre>{<br />"NotificationType":"Command",<br />"NotificationEvents":[ "Failed" ],<br />"NotificationArn":"$dependency.topicArn"<br />}<br /></pre>  |  <pre>"notificationConfig" : {<br />      "type" : "StringMap",<br />      "description" : "The configuration for events to be notified about",<br />      "default" : {<br />        "NotificationType" : "Command",<br />        "NotificationEvents" : ["Failed"],<br />        "NotificationArn" : "$dependency.topicArn"<br />      },<br />      "maxChars" : 150<br />    }<br /></pre>  | 
|  MapList  |  A list of StringMap items\.  |  <pre>[<br />   {<br />      "DeviceName" : "/dev/sda1",<br />      "Ebs" : { "VolumeSize" : "50" }<br />   },<br />   {<br />      "DeviceName" : "/dev/sdm",<br />      "Ebs" : { "VolumeSize" : "100" }<br />   }<br />]<br /></pre>  |  <pre>"blockDeviceMappings" : {<br />"type" : "MapList",<br />"description" : "The mappings for the create image inputs",<br />"default" : [{"DeviceName":"/dev/sda1","Ebs":{"VolumeSize":"50"}},{"DeviceName":"/dev/sdm","Ebs":{ "VolumeSize":"100"}}],<br />"maxItems": 2<br />}<br /></pre>  | 

## aws:applications<a name="aws-applications"></a>

Install, repair, or uninstall applications on an EC2 instance\. This plugin only runs on Microsoft Windows Server operating systems\. For more information, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

### Syntax<a name="applications-syntax"></a>

```
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
```

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

Export data from Windows Server to Amazon CloudWatch or Amazon CloudWatch Logs and monitor the data using CloudWatch metrics\. This plugin only runs on Microsoft Windows Server operating systems\. For more information about configuring CloudWatch integration with Amazon EC2, see [Sending Logs, Events, and Performance Counters to Amazon CloudWatch](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/send_logs_to_cwl.html)\. For more information about documents, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

You can export and monitor the following data types:

**ApplicationEventLog**  
Sends application event log data to CloudWatch Logs\.

**CustomLogs**  
Sends any text\-based log file to CloudWatch Logs\. The CloudWatch plugin creates a fingerprint for log files\. The system then associates a data offset with each fingerprint\. The plugin uploads files when there are changes, records the offset, and associates the offset with a fingerprint\. This method is used to avoid a situation where a user enables the plugin, associates the service with a directory that contains a large number of files, and the system uploads all of the files\.  
Be aware that if your application truncates or attempts to clean logs during polling, any logs specified for `LogDirectoryPath` can lose entries\. If, for example, you want to limit log file size, create a new log file when that limit is reached, and then continue writing data to the new file\.

**ETW**  
Sends Event Tracing for Windows \(ETW\) data to CloudWatch Logs\. Microsoft Windows Server 2003 is not supported\. 

**IIS**  
Sends IIS log data to CloudWatch Logs\.

**PerformanceCounter**  
Sends Windows performance counters to CloudWatch\. You can select different categories to upload to CloudWatch as metrics\. For each performance counter that you want to upload, create a **PerformanceCounter** section with a unique ID \(for example, "PerformanceCounter2", "PerformanceCounter3", and so on\) and configure its properties\.  
If the SSM Agent or the CloudWatch plugin is stopped, performance counter data is not logged in CloudWatch This behavior is different than custom logs or Windows Event logs\. Custom logs and Windows Event logs preserve performance counter data and upload it to CloudWatch after the SSM Agent or the CloudWatch plugin is available\.

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

### Settings and Properties<a name="cloudWatch-properties"></a>

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
For CustomLogs, the path where logs are stored on your Amazon EC2 instance\. For IIS logs, the folder where IIS logs are stored for an individual site \(for example, **C:\\\\inetpub\\\\logs\\\\LogFiles\\\\W3SVC*n***\)\. For IIS logs, only W3C log format is supported\. IIS, NCSA, and Custom formats are not supported\.   
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
Valid values: Regions IDs of the AWS Regions supported by both Systems Manager and CloudWatch Logs, such as `us-east-2`, `eu-west-1`, and `ap-southeast-1`\. For lists of AWS Regions supported by each service, see [AWS Systems Manager](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) and [Amazon CloudWatch Logs](http://docs.aws.amazon.com/general/latest/gr/rande.html#cwl_region) in the *AWS General Reference*\.   
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

\(Schema version 2\.0 or later\) Configure an instance to work with containers and Docker\. This plugin runs only on Microsoft Windows Server operating systems\. For more information, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

### Syntax<a name="configuredocker-syntax"></a>

```
"mainSteps": [
    {
      "action": "aws:configureDocker",
      "name": "ConfigureDocker",
      "inputs": {
        "action": "{{ action }}"
      }
    }
  ]
```

### Inputs<a name="configuredocker-properties"></a>

**action**  
The type of action to perform\.  
Type: Enum  
Valid values: `Install` \| `Uninstall`  
Required: Yes

## aws:configurePackage<a name="aws-configurepackage"></a>

\(Schema version 2\.0 or later\) Install or uninstall an AWS package\. Available packages include the following: AWSPVDriver, AwsEnaNetworkDriver, IntelSriovDriver, AwsVssComponents, AmazonCloudWatchAgent, and AWSSupport\-EC2Rescue\. This plugin runs on Linux and Microsoft Windows Server operating systems\. For more information, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

### Syntax<a name="configurepackage-syntax"></a>

```
"mainSteps": [
   {
		"action": "aws:configurePackage",
		"name": "configurePackage",
		"inputs": {
			"name": "{{ name }}",
			"action": "{{ action }}",
			"version": "{{ version }}"
		}
	}
]
```

### Inputs<a name="configurepackage-properties"></a>

**name**  
The name of the AWS package to install or uninstall\. Available packages include the following: AWSPVDriver, AwsEnaNetworkDriver, IntelSriovDriver, AwsVssComponents, and AmazonCloudWatchAgent\.  
Type: String  
Required: Yes

**action**  
Install or uninstall a package\.  
Type: Enum  
Valid values: `Install` \| `Uninstall`  
Required: Yes

**version**  
A specific version of the package to install or uninstall\. If installing, the system installs the latest published version, by default\. If uninstalling, the system uninstalls the currently installed version, by default\. If no installed version is found, the latest published version is downloaded, and the uninstall action is run\.  
Type: String  
Required: No

## aws:domainJoin<a name="aws-domainJoin"></a>

Join an Amazon EC2 instance to a domain\. This plugin only runs on Microsoft Windows Server operating systems\. For more information, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

### Syntax<a name="domainJoin-syntax"></a>

```
"runtimeConfig":{
        "aws:domainJoin":{
            "properties":{
                "directoryId":"{{ directoryId }}",
                "directoryName":"{{ directoryName }}",
                "directoryOU":"{{ directoryOU }}",
                "dnsIpAddresses":"{{ dnsIpAddresses }}"
            }
        }
```

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

For examples, see [Joining a Windows Server Instance to an AWS Directory Service Domain](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-join-aws-domain.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## aws:downloadContent<a name="aws-downloadContent"></a>

\(Schema version 2\.0 or later\) Download SSM documents and scripts from remote locations\. This plugin is supported on Linux and Windows Server operating systems\.

### Syntax<a name="downloadContent-syntax"></a>

```
"mainSteps": [
{
   "action":"aws:downloadContent",
   "name":"downloadContent",
   "inputs":{
      "sourceType":"{{ sourceType }}",
      "sourceInfo":"{{ sourceInfo }}",
      "destinationPath":"{{ destinationPath }}"
   }
}
```

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
+ getOptions: Extra options to retrieve content from a different branch or a different commit\. This parameter uses the following format:
  + branch:*branch\_name*

    The default is `master`\.
  + commitID:*commitID*

    The default is `head`\.
+ tokenInfo: The Systems Manager parameter \(a SecureString parameter\) where you store your access token information\.

```
Example syntax:
{
"owner":"TestUser", 
"repository":"GitHubTest", 
"path": "scripts/python/test-script",
"getOptions" : "branch:master",
"tokenInfo":"{{ssm-secure:secure-string-token}}" 
}
```
**For sourceType S3, specify the following:**  
+ path: The URL to the file or directory you want to download from Amazon S3\.

```
Example syntax:
{
"path": "https://s3.amazonaws.com/aws-executecommand-test/powershell/helloPowershell.ps1" 
}
```
**For sourceType SSMDocument, specify *one* of the following:**  
+ name: The name and version of the document in the following format: `name:version`\. Version is optional\. 

  ```
  Example syntax:
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

Install PowerShell modules on an EC2 instance\. This plugin only runs on Microsoft Windows Server operating systems\. For more information, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

### Syntax<a name="psModule-syntax"></a>

```
"runtimeConfig":{
        "aws:psModule":{
            "properties":[
                {
                    "id":"0.aws:psModule",
                    "runCommand":"{{ commands }}",
                    "source":"{{ source }}",
                    "sourceHash":"{{ sourceHash }}",
                    "workingDirectory":"{{ workingDirectory }}",
                    "timeoutSeconds":"{{ executionTimeout }}"
                }
            ]
```

### Properties<a name="psModule-properties"></a>

**runCommand**  
The PowerShell command to run after the module is installed\.  
Type: List or Array  
Required: No

**source**  
The URL or local path on the instance to the application `.zip` file\.  
Type: String  
Required: No

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

\(Schema version 2\.0 or later\) Refresh \(force apply\) an association on demand\. This action will change the system state based on what is defined in the selected association or all associations bound to the targets\. This plugin runs on Linux and Microsoft Windows Server operating systems\. For more information, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

### Syntax<a name="refreshassociation-syntax"></a>

```
"action":"aws:refreshAssociation",
      "name":"refreshAssociation",
      "inputs": {
        "associationIds": "{{ associationIds }}"
      }
```

### Inputs<a name="refreshassociation-properties"></a>

**associationIds**  
List of association IDs\. If empty, all associations bound to the specified target are applied\.  
Type: StringList  
Required: No

## aws:runDockerAction<a name="aws-rundockeraction"></a>

\(Schema version 2\.0 or later\) Run Docker actions on containers\. This plugin runs on Linux and Microsoft Windows Server operating systems\. For more information, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

### Syntax<a name="rundockeraction-syntax"></a>

```
"mainSteps": [
    {
      "action": "aws:runDockerAction",
      "name": "RunDockerAction",
      "inputs": {
        "action": "{{ action }}",
        "container": "{{ container }}",
        "image": "{{ image }}",
        "memory": "{{ memory }}",
        "cpuShares": "{{ cpuShares }}",
        "volume": "{{ volume }}",
        "cmd": "{{ cmd }}",
        "env": "{{ env }}",
        "user": "{{ user }}",
        "publish": "{{ publish }}"
      }
```

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

\(Schema version 2\.0 or later\) Executes SSM documents stored in Systems Manager or on a local share\. You can use this plugin with the [aws:downloadContent](#aws-downloadContent) plugin to download an SSM document from a remote location to a local share, and then run it\. This plugin is supported on Linux and Windows Server operating systems\.

### Syntax<a name="rundocument-syntax"></a>

```
"mainSteps": [
{
   "action":"aws:runDocument",
   "name":"runDocument",
   "inputs":{
      "documentType":"{{ documentType }}",
      "documentPath":"{{ documentPath }}",
      "documentParameters":"{{ documentParameters }}"
   }
}
```

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

Run PowerShell scripts or specify the path to a script to run\. This plugin runs on Microsoft Windows and Linux operating systems\. For more information, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

### Syntax<a name="runPowerShellScript-syntax"></a>

**Syntax for 1\.2 SSM document**

```
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
```

**Syntax for 2\.2 SSM document**

```
"mainSteps": [
   {
      "action":"aws:runPowerShellScript",
      "name:":"step name",
      "inputs":{
         "timeoutSeconds":"Timeout in seconds",
         "runCommand:":"[Command to execute]"
                }
    }
   ]
```

Here is a schemaVersion 2\.2 example:

```
{
   "schemaVersion":"2.2",
   "description":"Simple test document using the aws:runPowerShellScript plugin.",
   "parameters":{
      "Salutation":{
         "type":"String",
         "description":"(Optional) This is an optional parameter that will be displayed in the output of the command if specified.",
         "allowedPattern":"[a-zA-Z]",
         "default":"World"
      }
   },
   "mainSteps":[
      {
         "action":"aws:runPowerShellScript",
         "name":"DisplaySalutation",
         "inputs":{
            "timeoutSeconds":60,
            "runCommand":[
               "$salutation = '{{ Salutation }}'",
               "",
               "if ( [String]::IsNullOrWhitespace( $salutation ) )",
               "{",
               "  $salutation = 'anonymous'",
               "}",
               "",
               "Write-Host ('Hello {0}' -f $salutation)"
            ]
         }
      }
   ]
}
```

### Properties<a name="runPowerShellScript-properties"></a>

**runCommand**  
Specify the commands to run or the path to an existing script on the instance\.  
Type: List or Array  
Required: Yes

**timeoutSeconds**  
The time in seconds for a command to be completed before it is considered to have failed\.  
Type: String  
Required: No

**workingDirectory**  
The path to the working directory on your instance\.  
Type: String  
Required: No

## aws:runShellScript<a name="aws-runShellScript"></a>

Run Linux shell scripts or specify the path to a script to run\. This plugin only runs on Linux operating systems\. For more information, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

### Syntax<a name="runShellScript-syntax"></a>

```
"runtimeConfig":{
        "aws:runShellScript":{
            "properties":[
                {
                    "id":"0.aws:runShellScript",
                    "runCommand":"{{ commands }}",
                    "workingDirectory":"{{ workingDirectory }}",
                    "timeoutSeconds":"{{ executionTimeout }}"
                }
            ]
        }
```

### Properties<a name="runShellScript-properties"></a>

**runCommand**  
Specify the commands to run or the path to an existing script on the instance\.  
Type: List or Array  
Required: Yes

**timeoutSeconds**  
The time in seconds for a command to be completed before it is considered to have failed\.  
Type: String  
Required: No

**workingDirectory**  
The path to the working directory on your instance\.  
Type: String  
Required: No

## aws:softwareInventory<a name="aws-softwareinventory"></a>

\(Schema version 2\.0 or later\) Gather an inventory of applications, AWS components, network configuration, Windows Updates, and custom inventory from an instance\. This plugin runs on Linux and Microsoft Windows Server operating systems\. For more information, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

### Syntax<a name="softwareinventory-syntax"></a>

```
"mainSteps": [
        {
            "action": "aws:softwareInventory",
            "name": "collectSoftwareInventoryItems",
            "inputs": {
                "applications": "{{ applications }}",
                "awsComponents": "{{ awsComponents }}",
                "networkConfig": "{{ networkConfig }}",
                "windowsUpdates": "{{ windowsUpdates }}",
                "customInventory": "{{ customInventory }}"
            }
        }
```

### Inputs<a name="softwareinventory-properties"></a>

**applications**  
Collect data for installed applications\.  
Type: String  
Required: No

**awsComponents**  
Collect data for AWS components like amazon\-ssm\-agent\.  
Type: String  
Required: No

**networkConfig**  
Collect data for network configuration\.  
Type: String  
Required: No

**windowsUpdates**  
Collect data for all Windows updates\.  
Type: String  
Required: No

**customInventory**  
Collect data for custom inventory\.  
Type: String  
Required: No

## aws:updateAgent<a name="aws-updateagent"></a>

Update the EC2Config service to the latest version or specify an older version\. This plugin only runs on Microsoft Windows Server operating systems\. For more information about the EC2Config service, see [Configuring a Windows Instance Using the EC2Config Service](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/UsingConfig_WinAMI.html)\. For more information about documents, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

### Syntax<a name="updateagent-syntax"></a>

```
"runtimeConfig": {
        "aws:updateAgent": {
            "properties": {
                "agentName": "Ec2Config",
                "source": "https://s3.region.amazonaws.com/aws-ssm-region/manifest.json",
                "allowDowngrade": "{{ allowDowngrade }}",
                "targetVersion": "{{ version }}"
            }
        }
     }
```

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

## aws:updateSSMAgent<a name="aws-updatessmagent"></a>

Update SSM Agent to the latest version or specify an older version\. This plugin runs on Linux and Windows Server operating systems\. For more information, see [Installing and Configuring SSM Agent](ssm-agent.md)\. For more information about documents, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

### Syntax<a name="updateSSMagent-syntax"></a>

```
"runtimeConfig": {
        "aws:updateSsmAgent": {
            "properties": [
                {
                "agentName": "amazon-ssm-agent",
                "source": "https://s3.region.amazonaws.com/aws-ssm-region/manifest.json",
                "allowDowngrade": "{{ allowDowngrade }}",
                "targetVersion": "{{ version }}"
                }
            ]
        }
    }
```

### Properties<a name="updateSSMagent-properties"></a>

**agentName**  
amazon\-ssm\-agent\. This is the name of the Systems Manager agent that processes requests and runs commands on the instance\.  
Type: String  
Required: Yes

**allowDowngrade**  
Allow SSM Agent to be downgraded to an earlier version\. If set to false, the agent can be upgraded to newer versions only \(default\)\. If set to true, specify the earlier version\.   
Type: Boolean  
Required: No

**source**  
The location where Systems Manager copies the SSM Agent version to install\. You can't change this location\.  
Type: String  
Required: Yes

**targetVersion**  
A specific version of SSM Agent to install\. If not specified, the agent will be updated to the latest version\.  
Type: String  
Required: No