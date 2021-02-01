# Session document schema<a name="session-manager-schema"></a>

The following information describes the schema elements of a Session document\. Session Manager uses Session documents to determine which type of session to start, such as a standard session, a port forwarding session, or a session to run an interactive command\.

[schemaVersion](#version)  
The schema version of the Session document\. Currently, Session documents only support version 1\.0\.  
Type: String  
Required: Yes

[description](#descript)  
A description you specify for the Session document\. For example, "Document to start port forwarding session with Session Manager"\.  
Type: String  
Required: No

[sessionType](#type)  
The type of session the Session document is used to establish\.  
Type: String  
Required: Yes  
Valid values: `InteractiveCommands` \| `Port` \| `Standard_Stream`

[inputs](#in)  
The session preferences to use for sessions established using this Session document\. This element is required for Session documents that are used to create `Standard_Stream` sessions\.  
Type: StringMap  
Required: No    
[s3BucketName](#bucket)  
The Amazon Simple Storage Service \(Amazon S3\) bucket you want to send session logs to at the end of your sessions\.  
Type: String  
Required: No  
[s3KeyPrefix](#prefix)  
The prefix to use when sending logs to the S3 bucket you specified in the `s3BucketName` input\. For more information about using a shared prefix with objects stored in Amazon S3, see [How do I use folders in an S3 bucket?](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/using-folders.html) in the *Amazon Simple Storage Service Console User Guide*\.  
Type: String  
Required: No  
[s3EncryptionEnabled](#s3Encrypt)  
If set to `true`, the S3 bucket you specified in the `s3BucketName` input must be encrypted\.  
Type: Boolean  
Required: Yes  
[cloudWatchLogGroupName](#logGroup)  
The name of the Amazon CloudWatch Logs \(CloudWatch Logs\) group you want to send session logs to at the end of your sessions\.  
Type: String  
Required: No  
[cloudWatchEncryptionEnabled](#cwEncrypt)  
If set to `true`, the log group you specified in the `cloudWatchLogGroupName` input must be encrypted\.  
Type: Boolean  
Required: Yes  
[cloudWatchStreamingEnabled](#cwStream)  
If set to `true`, a continuous stream of session data logs are sent to the log group you specified in the `cloudWatchLogGroupName` input\. If set to `false`, session logs are sent to the log group you specified in the `cloudWatchLogGroupName` input at the end of your sessions\.  
Type: Boolean  
Required: Yes  
[kmsKeyId](#kms)  
The ID of the AWS Key Management Service \(AWS KMS\) key you want to use to further encrypt data between your local client machines and the Amazon Elastic Compute Cloud \(Amazon EC2\) instances you connect to\.  
Type: String  
Required: No  
[runAsEnabled](#run)  
If set to `true`, you must specify a user account that exists on the instances you'll be connecting to in the `runAsDefaultUser` input\. Otherwise, sessions will fail to start\. By default, sessions are started using the `ssm-user` account created by the SSM Agent\. The Run As feature is only supported for connecting to Linux instances\.  
Type: Boolean  
Required: Yes  
[runAsDefaultUser](#runUser)  
The name of the user account to start sessions with on Linux instances when the `runAsEnabled` input is set to `true`\. The user account you specify for this input must exist on the instances you'll be connecting to; otherwise, sessions will fail to start\.  
Type: String  
Required: No  
[idleSessionTimeout](#timeout)  
The amount of time of inactivity you want to allow before a session ends\. This input is measured in minutes\.  
Type: String  
Valid values: 1\-60  
Required: No  
[shellProfile](#shell)  
The preferences you specify per operating system to apply within sessions such as shell preferences, environment variables, working directories, and running multiple commands when a session is started\.  
Type: StringMap  
Required: No    
[windows](#win)  
The shell preferences, environment variables, working directories, and commands you specify for sessions on Windows instances\.  
Type: String  
Required: No  
[linux](#lin)  
The shell preferences, environment variables, working directories, and commands you specify for sessions on Linux instances\.  
Type: String  
Required: No

[parameters](#param)  
An object that defines the parameters the document accepts\. For more information about defining document parameters, see **parameters** in the [Top\-level elements](sysman-doc-syntax.md#top-level)\. For parameters that you reference often, we recommend that you store those parameters in Systems Manager Parameter Store and then reference them\. You can reference `String` and `StringList` Parameter Store parameters in this section of a document\. You can't reference `SecureString` Parameter Store parameters in this section of a document\. You can reference a Parameter Store parameter using the following format\.  

```
{{ssm:parameter-name}}
```
For more information about Parameter Store, see [AWS Systems Manager Parameter Store](systems-manager-parameter-store.md)\.  
Type: StringMap  
Required: No

[properties](#props)  
An object whose values you specify that are used in the `StartSession` API call\.  
For Session documents that are used for `InteractiveCommands` sessions, the properties object includes the commands to run on the operating systems you specify\. For more information, see [ \(Optional\) Restrict access to commands in a session](session-manager-restrict-command-access.md)\.  
For Session documents that are used for `Port` sessions, the properties object contains the port number where traffic should be redirected to\. For an example, see the `Port` type Session document example below\.  
Type: StringMap  
Required: No

`Standard_Stream` type Session document example

------
#### [ YAML ]

```
---
schemaVersion: '1.0'
description: Document to hold regional settings for Session Manager
sessionType: Standard_Stream
inputs:
  s3BucketName: ''
  s3KeyPrefix: ''
  s3EncryptionEnabled: true
  cloudWatchLogGroupName: ''
  cloudWatchEncryptionEnabled: true
  cloudWatchStreamingEnabled: true
  kmsKeyId: ''
  runAsEnabled: true
  runAsDefaultUser: ''
  idleSessionTimeout: '20'
  shellProfile:
    windows: ''
    linux: ''
```

------
#### [ JSON ]

```
{
    "schemaVersion": "1.0",
    "description": "Document to hold regional settings for Session Manager",
    "sessionType": "Standard_Stream",
    "inputs": {
        "s3BucketName": "",
        "s3KeyPrefix": "",
        "s3EncryptionEnabled": true,
        "cloudWatchLogGroupName": "",
        "cloudWatchEncryptionEnabled": true,
        "cloudWatchStreamingEnabled": true,
        "kmsKeyId": "",
        "runAsEnabled": true,
        "runAsDefaultUser": "",
        "idleSessionTimeout": "20",
        "shellProfile": {
            "windows": "date",
            "linux": "pwd;ls"
        }
    }
}
```

------

`InteractiveCommands` type Session document example

------
#### [ YAML ]

```
---
schemaVersion: '1.0'
description: Document to view a log file on a Linux instance
sessionType: InteractiveCommands
parameters:
  logpath:
    type: String
    description: The log file path to read.
    default: "/var/log/amazon/ssm/amazon-ssm-agent.log"
    allowedPattern: "^[a-zA-Z0-9-_/]+(.log)$"
properties:
  linux:
    commands: "tail -f {{ logpath }}"
    runAsElevated: true
```

------
#### [ JSON ]

```
{
    "schemaVersion": "1.0",
    "description": "Document to view a log file on a Linux instance",
    "sessionType": "InteractiveCommands",
    "parameters": {
        "logpath": {
            "type": "String",
            "description": "The log file path to read.",
            "default": "/var/log/amazon/ssm/amazon-ssm-agent.log",
            "allowedPattern": "^[a-zA-Z0-9-_/]+(.log)$"
        }
    },
    "properties": {
        "linux": {
            "commands": "tail -f {{ logpath }}",
            "runAsElevated": true
        }
    }
}
```

------

`Port` type Session document example

------
#### [ YAML ]

```
---
schemaVersion: '1.0'
description: Document to open given port connection over Session Manager
sessionType: Port
parameters:
  paramExample:
    type: string
    description: document parameter
properties:
  portNumber: anyPortNumber
```

------
#### [ JSON ]

```
{
    "schemaVersion": "1.0",
    "description": "Document to open given port connection over Session Manager",
    "sessionType": "Port",
    "parameters": {
        "paramExample": {
            "type": "string",
            "description": "document parameter"
        }
    },
    "properties": {
        "portNumber": "anyPortNumber"
    }
}
```

------

Session document example with special characters

------
#### [ YAML ]

```
---
schemaVersion: '1.0'
description: Example document with quotation marks
sessionType: InteractiveCommands
parameters:
  Test:
    type: String
    description: Test Input
    maxChars: 32
properties:
  windows:
    commands: |
        $Test = '{{ Test }}'
        $myVariable = \"Computer name is $env:COMPUTERNAME\"
        Write-Host "Test variable: $myVariable`.`nInput parameter: $Test"
    runAsElevated: false
```

------
#### [ JSON ]

```
{
   "schemaVersion":"1.0",
   "description":"Test document with quotation marks",
   "sessionType":"InteractiveCommands",
   "parameters":{
      "Test":{
         "type":"String",
         "description":"Test Input",
         "maxChars":32
      }
   },
   "properties":{
      "windows":{
         "commands":[
            "$Test = '{{ Test }}'",
            "$myVariable = \\\"Computer name is $env:COMPUTERNAME\\\"",
            "Write-Host \"Test variable: $myVariable`.`nInput parameter: $Test\""
         ],
         "runAsElevated":false
      }
   }
}
```

------