# `aws:executeScript` – Run a script<a name="automation-action-executeScript"></a>

Runs the Python or PowerShell script provided using the specified runtime and handler\. Each `aws:executeScript` action can run up to a maximum duration of 600 seconds \(10 minutes\)\. You can limit the timeout by specifying the `timeoutSeconds` parameter for an `aws:executeScript` step\.

Use return statements in your function to add outputs to your output payload\. For examples of defining outputs for your `aws:executeScript` action, see [Example 2: Scripted runbook](automation-authoring-runbooks-scripted-example.md)\. You can also send the output from `aws:executeScript` actions in your runbooks to the Amazon CloudWatch Logs log group you specify\. For more information, see [Logging Automation action output with CloudWatch Logs](automation-action-logging.md)\.

If you want to send output from `aws:executeScript` actions to CloudWatch Logs, or if the scripts you specify for `aws:executeScript` actions call AWS API operations, an AWS Identity and Access Management \(IAM\) service role \(or assume role\) is always required to run the runbook\.

The `aws:executeScript` action contains the following preinstalled PowerShell Core modules:
+ Microsoft\.PowerShell\.Host
+ Microsoft\.PowerShell\.Management
+ Microsoft\.PowerShell\.Security
+ Microsoft\.PowerShell\.Utility
+ PackageManagement
+ PowerShellGet

To use PowerShell Core modules that aren't preinstalled, your script must install the module with the `-Force` flag, as shown in the following command\. The `AWSPowerShell.NetCore` module isn't supported\. Replace *ModuleName* with the module you want to install\.

```
Install-Module ModuleName -Force
```

To use PowerShell Core cmdlets in your script, we recommend using the `AWS.Tools` modules, as shown in the following commands\. Replace each *example resource placeholder* with your own information\.
+ Amazon S3 cmdlets\.

  ```
  Install-Module AWS.Tools.S3 -Force
  Get-S3Bucket -BucketName bucketname
  ```
+ Amazon EC2 cmdlets\.

  ```
  Install-Module AWS.Tools.EC2 -Force
  Get-EC2InstanceStatus -InstanceId instanceId
  ```
+ Common, or service independent AWS Tools for Windows PowerShell cmdlets\.

  ```
  Install-Module AWS.Tools.Common -Force
  Get-AWSRegion
  ```

If your script initializes new objects in addition to using PowerShell Core cmdlets, you must also import the module as shown in the following command\.

```
Install-Module AWS.Tools.EC2 -Force
Import-Module AWS.Tools.EC2

$tag = New-Object Amazon.EC2.Model.Tag
$tag.Key = "Tag"
$tag.Value = "TagValue"

New-EC2Tag -Resource i-02573cafcfEXAMPLE -Tag $tag
```

For examples of installing and importing `AWS.Tools` modules, and using PowerShell Core cmdlets in runbooks, see [Using Document Builder to create runbooks](automation-document-builder.md)\.

**Input**  
Provide the information required to run your script\. Replace each *example resource placeholder* with your own information\.

**Note**  
The attachment for a Python script can be a \.py file or a \.zip file that contains the script\. PowerShell scripts must be stored in \.zip files\.

------
#### [ YAML ]

```
action: "aws:executeScript"
inputs: 
 Runtime: runtime
 Handler: "functionName"
 InputPayload: 
  scriptInput: '{{parameterValue}}'
 Script: |-
   def functionName(events, context):
   ...
 Attachment: "scriptAttachment.zip"
```

------
#### [ JSON ]

```
{
    "action": "aws:executeScript",
    "inputs": {
        "Runtime": "runtime",
        "Handler": "functionName",
        "InputPayload": {
            "scriptInput": "{{parameterValue}}"
        },
        "Attachment": "scriptAttachment.zip"
    }
}
```

------

Runtime  
The runtime language to be used for running the provided script\. `aws:executeScript` supports Python 3\.6 \(python3\.6\), Python 3\.7 \(python3\.7\), Python 3\.8 \(python3\.8\), PowerShell Core 6\.0 \(dotnetcore2\.1\), and PowerShell 7\.0 \(dotnetcore3\.1\) scripts\.  
Supported values: **python3\.6** \| **python3\.7** \| **python3\.8** \| **PowerShell Core 6\.0** \| **PowerShell 7\.0**  
Type: String  
Required: Yes

Handler  
The name of your function\. You must ensure the function defined in the handler has two parameters, `events` and `context`\. The PowerShell runtime does not support this parameter\.  
Type: String  
Required: Yes \(Python\) \| Not supported \(PowerShell\)

InputPayload  
A JSON or YAML object that will be passed to the first parameter of the handler\. This can be used to pass input data to the script\.  
Type: String  
Required: No  

```
description: Tag an instance
schemaVersion: '0.3'
assumeRole: '{{AutomationAssumeRole}}'
parameters:
    AutomationAssumeRole:
    type: String
    description: '(Required) The Amazon Resource Name (ARN) of the IAM role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses your IAM permissions to operate this runbook.'
    InstanceId:
    type: String
    description: (Required) The ID of the EC2 instance you want to tag.
mainSteps:
    - name: tagInstance
    action: 'aws:executeScript'
    inputs:
        Runtime: python3.8
        Handler: tagInstance
        InputPayload:
          instanceId: '{{InstanceId}}'
        Script: |-
          def tagInstance(events,context):
            import boto3

            #Initialize client
            ec2 = boto3.client('ec2')
            instanceId = events['instanceId']
            tag = {
                "Key": "Env",
                "Value": "Example"
            }
            ec2.create_tags(
                Resources=[instanceId],
                Tags=[tag]
            )
```

```
description: Tag an instance
schemaVersion: '0.3'
assumeRole: '{{AutomationAssumeRole}}'
parameters:
    AutomationAssumeRole:
    type: String
    description: '(Required) The Amazon Resource Name (ARN) of the IAM role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses your IAM permissions to operate this runbook.'
    InstanceId:
    type: String
    description: (Required) The ID of the EC2 instance you want to tag.
mainSteps:
    - name: tagInstance
    action: 'aws:executeScript'
    inputs:
        Runtime: PowerShell 7.0
        InputPayload:
          instanceId: '{{InstanceId}}'
        Script: |-
          Install-Module AWS.Tools.EC2 -Force
          Import-Module AWS.Tools.EC2

          $input = $env:InputPayload | ConvertFrom-Json

          $tag = New-Object Amazon.EC2.Model.Tag
          $tag.Key = "Env"
          $tag.Value = "Example"

          New-EC2Tag -Resource $input.instanceId -Tag $tag
```

Script  
An embedded script that you want to run during the automation\. This parameter is not supported for JSON runbooks\. JSON runbooks must provide script content using the `Attachment` input parameter\.  
Type: String  
Required: No \(Python\) \| Yes \(PowerShell\)

Attachment  
The name of a standalone script file or \.zip file that can be invoked by the action\. Specify the same value as the `Name` of the document attachment file you specify in the `Attachments` request parameter\. For more information, see [Attachments](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateDocument.html#systemsmanager-CreateDocument-request-Attachments) in the *AWS Systems Manager API Reference*\. If you're providing a script using an attachment, you must also define a `files` section in the top\-level elements of your runbook\. For more information, see [Schema version 0\.3](document-schemas-features.md#automation-doc-syntax-examples)\.  
To invoke a file for Python, use the `filename.method_name` format in `Handler`\.   
The attachment for a Python script can be a \.py file or a \.zip file that contains the script\. PowerShell scripts must be stored in \.zip files\.
When including Python libraries in your attachment, we recommend adding an empty `__init__.py` file in each module directory\. This allows you to import the modules from the library in your attachment within your script content\. For example: `from library import module`  
Type: String  
Required: NoOutput

Payload  
The JSON representation of the object returned by your function\. Up to 100KB is returned\. If you output a list, a maximum of 100 items is returned\.