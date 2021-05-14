# Step 5: \(Optional\) Restrict access to commands in a session<a name="session-manager-restrict-command-access"></a>

You can restrict the commands a user can run in a Session Manager session by creating a custom `Session` type SSM document\. In the document content, you define which command is run when the user starts a session and what parameters they can provide to the command\. These are also referred to as interactive commands\. The `Session` document `schemaVersion` must be 1\.0, and the `sessionType` of the document must be `InteractiveCommands`\. You can then create AWS Identity and Access Management \(IAM\) policies that allow users to access only the `Session` documents you define\. For more information about using IAM policies to restrict access to commands in a session, see [IAM policy examples for interactive commands](#interactive-command-policy-examples)\.

The user specifies the allowed document in the `--document-name` option for the `start-session` command and provides any necessary parameter values for the command in the `--parameters` option\. For more information about running interactive commands, see [Starting a session \(interactive and noninteractive commands\)](session-manager-working-with-sessions-start.md#sessions-start-interactive-commands)\.

The following procedure describes how to create a custom `Session` type SSM document that defines the command a user is allowed to run\.

## Restrict access to commands in a session \(console\)<a name="restrict-command-access-console"></a>

**To restrict the commands a user can run in a Session Manager session \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

1. Choose **Create command or session**\.

1. For **Name**, enter a descriptive name for the document\.

1. For **Document type**, choose **Session document**\.

1. Enter your document content that defines the command a user can run in a Session Manager session using JSON or YAML, as shown in the following example\.

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

1. Choose **Create document**\.

## Restrict access to commands in a session \(command line\)<a name="restrict-command-access-commandline"></a>

**Before you begin**  
If you have not already, install and configure the AWS Command Line Interface \(AWS CLI\) or the AWS Tools for PowerShell\. For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

**To restrict the commands a user can run in a Session Manager session \(command line\)**

1. Create a JSON or YAML file for your document content that defines the command a user can run in a Session Manager session, as shown in the following example\.

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

1. Run the following commands to create an SSM document using your content that defines the command a user can run in a Session Manager session\.

------
#### [ Linux & macOS ]

   ```
   aws ssm create-document \
       --content file://path/to/file/documentContent.json \
       --name "exampleAllowedSessionDocument" \
       --document-type "Session"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-document ^
       --content file://C:\path\to\file\documentContent.json ^
       --name "exampleAllowedSessionDocument" ^
       --document-type "Session"
   ```

------
#### [ PowerShell ]

   ```
   $json = Get-Content -Path "C:\path\to\file\documentContent.json" | Out-String
   New-SSMDocument `
       -Content $json `
       -Name "exampleAllowedSessionDocument" `
       -DocumentType "Session"
   ```

------

## Interactive command parameters and the AWS CLI<a name="restrict-command-access-parameters-cli"></a>

There are a variety of ways you can provide interactive command parameters when using the AWS CLI\. Depending on the operating system \(OS\) of your client machine that you use to connect to instances with the AWS CLI, the syntax you provide for commands that contain special or escape characters might differ\. The following examples show some of the different ways you can provide command parameters when using the AWS CLI, and how to handle special or escape characters\.

Parameters stored in Parameter Store can be referenced in the AWS CLI for your command parameters as shown in the following example\.

------
#### [ Linux & macOS ]

```
aws ssm start-session \
    --target instance-id \
    --document-name MyInteractiveCommandDocument \ 
    --parameters '{"command":["{{ssm:mycommand}}"]}'
```

------
#### [ Windows ]

```
aws ssm start-session ^
    --target instance-id ^
    --document-name MyInteractiveCommandDocument ^
    --parameters '{"command":["{{ssm:mycommand}}"]}'
```

------

The following example shows how you can use a shorthand syntax with the AWS CLI to pass parameters\.

------
#### [ Linux & macOS ]

```
aws ssm start-session \
    --target instance-id \
    --document-name MyInteractiveCommandDocument \ 
    --parameters command="ifconfig"
```

------
#### [ Windows ]

```
aws ssm start-session ^
    --target instance-id ^
    --document-name MyInteractiveCommandDocument ^
    --parameters command="ipconfig"
```

------

You can also provide parameters in JSON as shown in the following example\.

------
#### [ Linux & macOS ]

```
aws ssm start-session \
    --target instance-id \
    --document-name MyInteractiveCommandDocument \ 
    --parameters '{"command":["ifconfig"]}'
```

------
#### [ Windows ]

```
aws ssm start-session ^
    --target instance-id ^
    --document-name MyInteractiveCommandDocument ^
    --parameters '{"command":["ipconfig"]}'
```

------

Parameters can also be stored in a JSON file and provided to the AWS CLI as shown in the following example\. For more information about using AWS CLI parameters from a file, see [Loading AWS CLI parameters from a file](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-parameters-file.html) in the *AWS Command Line Interface User Guide*\.

```
{
    "command": [
        "my command"
    ]
}
```

------
#### [ Linux & macOS ]

```
aws ssm start-session \
    --target instance-id \
    --document-name MyInteractiveCommandDocument \ 
    --parameters file://complete/path/to/file/parameters.json
```

------
#### [ Windows ]

```
aws ssm start-session ^
    --target instance-id ^
    --document-name MyInteractiveCommandDocument ^
    --parameters file://complete/path/to/file/parameters.json
```

------

You can also generate an AWS CLI skeleton from a JSON input file as shown in the following example\. For more information about generating AWS CLI skeletons from JSON input files, see [Generating AWS CLI skeleton and input parameters from a JSON or YAML input file](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-skeleton.html) in the *AWS Command Line Interface User Guide*\.

```
{
    "Target": "instance-id",
    "DocumentName": "MyInteractiveCommandDocument",
    "Parameters": {
        "command": [
            "my command"
        ]
    }
}
```

------
#### [ Linux & macOS ]

```
aws ssm start-session \
    --cli-input-json file://complete/path/to/file/parameters.json
```

------
#### [ Windows ]

```
aws ssm start-session ^
    --cli-input-json file://complete/path/to/file/parameters.json
```

------

To escape characters inside quotation marks, you must add additional backslashes to the escape characters as shown in the following example\.

------
#### [ Linux & macOS ]

```
aws ssm start-session \
    --target instance-id \
    --document-name MyInteractiveCommandDocument \ 
    --parameters '{"command":["printf \"abc\\\\tdef\""]}'
```

------
#### [ Windows ]

```
aws ssm start-session ^
    --target instance-id ^
    --document-name MyInteractiveCommandDocument ^
    --parameters '{"command":["printf \"abc\\\\tdef\""]}'
```

------

For information about using quotation marks with command parameters in the AWS CLI, see [Using quotation marks with strings in the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-parameters-quoting-strings.html) in the *AWS Command Line Interface User Guide*\.

## IAM policy examples for interactive commands<a name="interactive-command-policy-examples"></a>

You can create IAM policies that allow users to access only the `Session` documents you define\. This restricts the commands a user can run in a Session Manager session to only the commands defined in your custom `Session` type SSM documents\.

 **Allow a user to run an interactive command on a single instance**   

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":"ssm:StartSession",
         "Resource":[
            "arn:aws:ec2:region:987654321098:instance/i-02573cafcfEXAMPLE",
            "arn:aws:ssm:region:987654321098:document/exampleAllowedSessionDocument"
         ],
         "Condition":{
            "BoolIfExists":{
               "ssm:SessionDocumentAccessCheck":"true"
            }
         }
      }
   ]
}
```

 **Allow a user to run an interactive command on all instances**   

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":"ssm:StartSession",
         "Resource":[
            "arn:aws:ec2:us-west-2:987654321098:instance/*",
            "arn:aws:ssm:us-west-2:987654321098:document/exampleAllowedSessionDocument"
         ],
         "Condition":{
            "BoolIfExists":{
               "ssm:SessionDocumentAccessCheck":"true"
            }
         }
      }
   ]
}
```

 **Allow a user to run multiple interactive commands on all instances**   

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":"ssm:StartSession",
         "Resource":[
            "arn:aws:ec2:us-west-2:987654321098:instance/*",
            "arn:aws:ssm:us-west-2:987654321098:document/exampleAllowedSessionDocument",
            "arn:aws:ssm:us-west-2:987654321098:document/exampleAllowedSessionDocument2"
         ],
         "Condition":{
            "BoolIfExists":{
               "ssm:SessionDocumentAccessCheck":"true"
            }
         }
      }
   ]
}
```