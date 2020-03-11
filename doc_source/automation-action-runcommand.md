# aws:runCommand – Run a command on a managed instance<a name="automation-action-runcommand"></a>

Runs the specified commands\.

**Note**  
Automation only supports *output* of one Run Command action\. A document can include multiple Run Command actions and plugins, but output is supported for only one action and plugin at a time\.

**Input**  
This action supports most send command parameters\. For more information, see [SendCommand](https://docs.aws.amazon.com/ssm/latest/APIReference/API_SendCommand.html)\.

------
#### [ YAML ]

```
name: installPowerShellModule
action: aws:runCommand
inputs:
  DocumentName: AWS-InstallPowerShellModule
  InstanceIds:
  - i-1234567890abcdef0
  Parameters:
    source: 'https://my-s3-url.com/MyModule.zip '
    sourceHash: ASDFWER12321WRW
```

------
#### [ JSON ]

```
{
    "name": "installPowerShellModule",
    "action": "aws:runCommand",
    "inputs": {
        "DocumentName": "AWS-InstallPowerShellModule",
        "InstanceIds": ["i-1234567890abcdef0"],
        "Parameters": {
            "source": "https://my-s3-url.com/MyModule.zip ",
            "sourceHash": "ASDFWER12321WRW"
        }
    }
}
```

------

DocumentName  
The name of the Run Command document\.  
Type: String  
Required: Yes

InstanceIds  
The instance IDs where you want the command to run\. You can specify a maximum of 50 IDs\.   
You can also use the pseudo parameter `{{RESOURCE_ID}}` in place of instance IDs to more easily run the command on all instances in the target group\. For more information about pseudo parameters, see [About Pseudo Parameters](mw-cli-register-tasks-parameters.md)\. \.  
Another alternative is to send commands to a fleet of instances by using the Targets parameter\. The Targets parameter accepts Amazon EC2 tags\. For more information about how to use the Targets parameter, see [Using Targets and Rate Controls to Send Commands to a Fleet](send-commands-multiple.md)\.  
Type: StringList  
Required: No \(If you don't specify InstanceIds or use the `{{RESOURCE_ID}}` pseudo parameter, then you must specify the Targets parameter\.\)

Targets  
An array of search criteria that targets instances by using a Key,Value combination that you specify\. Targets is required if you don't provide one or more instance IDs in the call\. For more information about how to use the Targets parameter, see [Using Targets and Rate Controls to Send Commands to a Fleet](send-commands-multiple.md)\.  
Type: MapList \(The schema of the map in the list must match the object\. For information, see [Target](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_Target.html) in the *AWS Systems Manager API Reference*\.  
Required: No \(If you don't specify Targets, then you must specify InstanceIds or use the `{{RESOURCE_ID}}` pseudo parameter\.\)  
Here is an example:  

```
{
    "name": "installPowerShellModule",
    "action": "aws:runCommand",
    "inputs": {
        "DocumentName": "AWS-InstallPowerShellModule",
        "Targets": [                   
            {
                "Key": "tag:Stage",
                "Values": [
                    "Gamma", "Beta"
                ]
            },
            {
                "Key": "tag-key",
                "Values": [
                    "Suite"
                ]
            }
        ],
        "Parameters": {
            "source": "https://my-s3-url.com/MyModule.zip",
            "sourceHash": "ASDFWER12321WRW"
        }
    }
}
```

Parameters  
The required and optional parameters specified in the document\.  
Type: Map  
Required: No

CloudWatchOutputConfig  
Configuration options for sending command output to Amazon CloudWatch Logs\. For more information about sending command output to CloudWatch Logs, see [Configuring Amazon CloudWatch Logs for Run Command](sysman-rc-setting-up-cwlogs.md)\.  
Type: StringMap \(The schema of the map must match the object\. For more information, see [CloudWatchOutputConfig](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CloudWatchOutputConfig.html) in the *AWS Systems Manager API Reference*\)\.  
Required: No  
Here is an example:  

```
{
    "name": "installPowerShellModule",
    "action": "aws:runCommand",
    "inputs": {
        "DocumentName": "AWS-InstallPowerShellModule",
        "InstanceIds": ["i-1234567890abcdef0"],
        "Parameters": {
            "source": "https://my-s3-url.com/MyModule.zip",
            "sourceHash": "ASDFWER12321WRW"
        },
        "CloudWatchOutputConfig" : { 
            "CloudWatchLogGroupName": "CloudWatchGroupForSSMAutomationService",
            "CloudWatchOutputEnabled": true
        }
    }
}
```

Comment  
User\-defined information about the command\.  
Type: String  
Required: No

DocumentHash  
The hash for the document\.  
Type: String  
Required: No

DocumentHashType  
The type of the hash\.  
Type: String  
Valid values: `Sha256` \| `Sha1`  
Required: No

NotificationConfig  
The configurations for sending notifications\.  
Required: No

OutputS3BucketName  
The name of the S3 bucket for command execution responses\.  
Type: String  
Required: No

OutputS3KeyPrefix  
The prefix\.  
Type: String  
Required: No

ServiceRoleArn  
The ARN of the IAM role\.  
Type: String  
Required: No

TimeoutSeconds  
The run\-command timeout value, in seconds\.  
Type: Integer  
Required: NoOutput

CommandId  
The ID of the command\.

Status  
The status of the command\.

ResponseCode  
The response code of the command\.

Output  
The output of the command\.