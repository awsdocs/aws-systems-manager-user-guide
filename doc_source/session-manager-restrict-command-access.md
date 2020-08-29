# Step 5: \(Optional\) Restrict access to commands in a session<a name="session-manager-restrict-command-access"></a>

You can restrict the commands a user can run in a Session Manager session by creating a custom `Session` type SSM document\. In the document content, you define which command is run when the user starts a session and what parameters they can provide to the command\. These are also referred to as interactive commands\. The `Session` document `schemaVersion` must be 1\.0 and the `sessionType` of the document must be `InteractiveCommands`\. You can then create IAM policies that allow users to access only the `Session` documents you define\. For more information about using IAM policies to restrict access to commands in a session, see [IAM policy examples for interactive commands](#interactive-command-policy-examples)\.

The user specifies the allowed document in the `--document-name` option for the `start-session` command and provides any necessary parameter values for the command in the `--parameters` option\. For more information about running interactive commands, see [Starting a session \(interactive commands\)](session-manager-working-with-sessions-start.md#sessions-start-interactive-commands)\.

The following procedure describes how to create a custom `Session` type SSM document that defines the command a user is allowed to run\.

## Restrict access to commands in a session \(console\)<a name="restrict-command-access-console"></a>

**To restrict the commands a user can run in a Session Manager session \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

1. Choose **Create command or session**\.

1. For **Name**, type a descriptive name for the document\.

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
Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\. For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

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
#### [ Linux ]

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
            "arn:aws:ec2:us-west-2:987654321098:instance/i-02573cafcfEXAMPLE",
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