# SSM document syntax<a name="sysman-doc-syntax"></a>

The syntax of your document is defined by the schema version used to create it\. We recommended that you use schema version 2\.2 or later for Command documents\. Automation runbooks use schema version 0\.3\. Additionally, Automation runbooks support the use of Markdown, a markup language, which allows you to add wiki\-style descriptions to documents and individual steps within the document\. For more information about using Markdown, see [Using Markdown in AWS](https://docs.aws.amazon.com/general/latest/gr/aws-markdown.html)\.

The top\-level elements provide the structure of the SSM document\. The information in this topic pertains to `Command` and `Automation` SSM documents\.

## Top\-level elements<a name="top-level"></a>

**schemaVersion**  
The schema version to use\.  
Type: Version  
Required: Yes

**description**  
Information you provide to describe the purpose of the document\. You can also use this field to specify whether a parameter requires a value for a document to run, or if providing a value for the parameter is optional\. Required and optional parameters can be seen in the examples throughout this topic\.  
Type: String  
Required: No

**parameters**  
A structure that defines the parameters the document accepts\. For parameters that you reference often, we recommend that you store those parameters in Parameter Store, a capability of AWS Systems Manager, and then reference them\. You can reference `String` and `StringList` Parameter Store parameters in this section of a document\. You can't reference `SecureString` Parameter Store parameters in this section of a document\. You can reference a Parameter Store parameter using the following format:  

```
{{ssm:parameter-name}}
```

```
AMI:
  type: String
  description: "(Optional) The AMI to use when launching the instance."
  default: {{ssm:/aws/service/list/ami-windows-latest}}
```

```
"AMI": {
  "type": "String",
  "description": "(Optional) The AMI to use when launching the instance.",
  "default": "{{ssm:/aws/service/list/ami-windows-latest}}"
}
```
For more information about Parameter Store, see [AWS Systems Manager Parameter Store](systems-manager-parameter-store.md)\.  
Type: Structure  
The `parameters` structure accepts the following fields and values:  
+ `type`: \(Required\) Allowed values include the following: `String`, `StringList`, `Integer` `Boolean`, `MapList`, and `StringMap`\. To view examples of each type, see [SSM document parameter `type` examples](#top-level-properties-type) in the next section\.
**Note**  
To use a number as a parameter value, use `String` as the parameter type\.
+ `description`: \(Optional\) A description of the parameter\.
+ `default`: \(Optional\) The default value of the parameter or a reference to a parameter in Parameter Store\.
+ `allowedValues`: \(Optional\) An array of values allowed for the parameter\. Defining allowed values for the parameter validates the user input\. If a user inputs a value that isn't allowed, the execution fails to start\.

------
#### [ YAML ]

  ```
  DirectoryType:
    type: String
    description: "(Required) The directory type to launch."
    default: AwsMad
    allowedValues:
    - AdConnector
    - AwsMad
    - SimpleAd
  ```

------
#### [ JSON ]

  ```
  "DirectoryType": {
    "type": "String",
    "description": "(Required) The directory type to launch.",
    "default": "AwsMad",
    "allowedValues": [
      "AdConnector",
      "AwsMad",
      "SimpleAd"
    ]
  }
  ```

------
+ `allowedPattern`: \(Optional\) A regular expression that validates whether the user input matches the defined pattern for the parameter\. If the user input doesn't match the allowed pattern, the execution fails to start\.
**Note**  
In SSM documents, the `allowedPattern` field supports the [Google re2 regex syntax](https://github.com/google/re2/wiki/Syntax), which doesn't include support for lookaround\.

------
#### [ YAML ]

  ```
  InstanceId:
    type: String
    description: "(Required) The instance ID to target."
    allowedPattern: "^i-[a-z0-9]{8,17}$"
    default: ''
  ```

------
#### [ JSON ]

  ```
  "InstanceId": {
    "type": "String",
    "description": "(Required) The instance ID to target.",
    "allowedPattern": "^i-[a-z0-9]{8,17}$",
    "default": ""
  }
  ```

------
+ `displayType`: \(Optional\) Used to display either a `textfield` or a `textarea` in the AWS Management Console\. `textfield` is a single\-line text box\. `textarea` is a multi\-line text area\.
+ `minItems`: \(Optional\) The minimum number of items allowed\.
+ `maxItems`: \(Optional\) The maximum number of items allowed\.
+ `minChars`: \(Optional\) The minimum number of parameter characters allowed\.
+ `maxChars`: \(Optional\) The maximum number of parameter characters allowed\.
Required: No

**runtimeConfig**  
\(Schema version 1\.2 only\) The configuration for the instance as applied by one or more Systems Manager plugins\. Plugins aren't guaranteed to run in sequence\.   
Type: Dictionary<string,PluginConfiguration>  
Required: No

**mainSteps**  
\(Schema version 0\.3, 2\.0, and 2\.2 only\) An object that can include multiple steps \(plugins\)\. Plugins are defined within steps\. Steps run in sequential order as listed in the document\.   
Type: Dictionary<string,PluginConfiguration>  
Required: Yes

**outputs**  
\(Schema version 0\.3 only\) Data generated by the execution of this document that can be used in other processes\. For example, if your document creates a new AMI, you might specify "CreateImage\.ImageId" as the output value, and then use this output to create new instances in a subsequent automation execution\. For more information about outputs, see [Working with inputs and outputs](automation-aws-apis-calling.md#automation-aws-apis-calling-input-output)\.  
Type: Dictionary<string,OutputConfiguration>  
Required: No

**files**  
\(Schema version 0\.3 only\) The script files \(and their checksums\) attached to the document and run during an automation execution\. Applies only to documents that include the `aws:executeScript` action and for which attachments have been specified in one or more steps\.   
For script runtime support, Automation runbooks support scripts for Python 3\.6, Python 3\.7, and PowerShell Core 6\.0\. For more information about including scripts in Automation runbooks, see [Creating runbooks that run scripts](automation-document-script.md) and [ Walkthrough: Using Document Builder to create a custom runbook](automation-walk-document-builder.md)\.  
When you create an Automation runbook you specify attachment files using the `--attachments` option \(for AWS CLI\) or `Attachments` \(for API and SDK\)\. You can specify the file location for both local files and files stored in Amazon Simple Storage Service \(Amazon S3\) buckets\.  

```
---
files:
  launch.py:
    checksums:
      sha256: 18871b1311b295c43d0f...[truncated]...772da97b67e99d84d342ef4aEXAMPLE
```

```
"files": {
    "launch.py": {
        "checksums": {
            "sha256": "18871b1311b295c43d0f...[truncated]...772da97b67e99d84d342ef4aEXAMPLE"
        }
    }
}
```
Type: Dictionary<string,FilesConfiguration>  
Required: No

## SSM document parameter `type` examples<a name="top-level-properties-type"></a>

Parameter types in SSM documents are static\. This means the parameter type can't be changed after it's defined\. When using parameters with SSM document plugins, the type of a parameter can't be dynamically changed within a plugin's input\. For example, you can't reference an `Integer` parameter within the `runCommand` input of the `aws:runShellScript` plugin because this input accepts a string or list of strings\. To use a parameter for a plugin input, the parameter type must match the accepted type\. For example, you must specify a `Boolean` type parameter for the `allowDowngrade` input of the `aws:updateSsmAgent` plugin\. If your parameter type doesn't match the input type for a plugin, the SSM document fails to validate and the system doesn't create the document\. This is also true when using parameters downstream within inputs for other plugins or AWS Systems Manager Automation actions\. For example, you can't reference a `StringList` parameter within the `documentParameters` input of the `aws:runDocument` plugin\. The `documentParameters` input accepts a map of strings even if the downstream SSM document parameter type is a `StringList` parameter and matches the parameter you're referencing\.

When using parameters with Automation actions, parameter types aren't validated when you create the SSM document in most cases\. Only when you use the `aws:runCommand` action are parameter types validated when you create the SSM document\. In all other cases, the parameter validation occurs during the automation execution when an action's input is verified before running the action\. For example, if your input parameter is a `String` and you reference it as the value for the `MaxInstanceCount` input of the `aws:runInstances` action, the SSM document is created\. However, when running the document, the automation fails while validating the `aws:runInstances` action because the `MaxInstanceCount` input requires an `Integer`\.

The following are examples of each parameter `type`\.

String  
A sequence of zero or more Unicode characters wrapped in quotation marks\. For example, "i\-1234567890abcdef0"\. Use backslashes to escape\.  

```
---
InstanceId:
  type: String
  description: "(Optional) The target EC2 instance ID."
```

```
"InstanceId":{
  "type":"String",
  "description":"(Optional) The target EC2 instance ID."
}
```

StringList  
A list of String items separated by commas\. For example, \["cd \~", "pwd"\]\.  

```
---
commands:
  type: StringList
  description: "(Required) Specify a shell script or a command to run."
  minItems: 1
  displayType: textarea
```

```
"commands":{
  "type":"StringList",
  "description":"(Required) Specify a shell script or a command to run.",
  "minItems":1,
  "displayType":"textarea"
}
```

Boolean  
Accepts only `true` or `false`\. Doesn't accept "true" or 0\.  

```
---
canRun:
  type: Boolean
  description: ''
  default: true
```

```
"canRun": {
  "type": "Boolean",
  "description": "",
  "default": true
}
```

Integer  
Integral numbers\. Doesn't accept decimal numbers, for example 3\.14159, or numbers wrapped in quotation marks, for example "3"\.  

```
---
timeout:
  type: Integer
  description: The type of action to perform.
  default: 100
```

```
"timeout": {
  "type": "Integer",
  "description": "The type of action to perform.",
  "default": 100    
}
```

StringMap  
A mapping of keys to values\. A key can only be a string\. For example, \{"Env": "Prod"\}\.  

```
---
notificationConfig:
  type: StringMap
  description: The configuration for events to be notified about
  default:
    NotificationType: Command
    NotificationEvents:
    - Failed
    NotificationArn: "$dependency.topicArn"
  maxChars: 150
```

```
"notificationConfig" : {
  "type" : "StringMap",
  "description" : "The configuration for events to be notified about",
  "default" : {
    "NotificationType" : "Command",
    "NotificationEvents" : ["Failed"],
    "NotificationArn" : "$dependency.topicArn"
  },
  "maxChars" : 150
}
```

MapList  
A list of StringMap items\.  

```
blockDeviceMappings:
  type: MapList
  description: The mappings for the create image inputs
  default:
  - DeviceName: "/dev/sda1"
    Ebs:
      VolumeSize: '50'
  - DeviceName: "/dev/sdm"
    Ebs:
      VolumeSize: '100'
  maxItems: 2
```

```
"blockDeviceMappings":{
  "type":"MapList",
  "description":"The mappings for the create image inputs",
  "default":[
    {
      "DeviceName":"/dev/sda1",
      "Ebs":{
        "VolumeSize":"50"
      }
    },
    {
      "DeviceName":"/dev/sdm",
      "Ebs":{
        "VolumeSize":"100"
      }
    }
  ],
  "maxItems":2
}
```