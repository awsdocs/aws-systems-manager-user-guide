# Systems Manager Automation actions reference<a name="automation-actions"></a>

This reference describes the Automation actions that you can specify in an AWS Systems Manager Automation document\. These actions cannot be used in other types of SSM documents\. For information about plugins for other types of SSM documents, see [SSM document plugin reference](ssm-plugins.md)\.

Systems Manager Automation runs steps defined in Automation documents\. Each step is associated with a particular action\. The action determines the inputs, behavior, and outputs of the step\. Steps are defined in the `mainSteps` section of your Automation document\.

You don't need to specify the outputs of an action or step\. The outputs are predetermined by the action associated with the step\. When you specify step inputs in your Automation documents, you can reference one or more outputs from an earlier step\. For example, you can make the output of `aws:runInstances` available for a subsequent `aws:runCommand` action\. You can also reference outputs from earlier steps in the `Output` section of the Automation document\. 

**Important**  
If you run an automation workflow that invokes other services by using an AWS Identity and Access Management \(IAM\) service role, be aware that the service role must be configured with permission to invoke those services\. This requirement applies to all AWS Automation documents \(`AWS-*` documents\) such as the `AWS-ConfigureS3BucketLogging`, `AWS-CreateDynamoDBBackup`, and `AWS-RestartEC2Instance` documents, to name a few\. This requirement also applies to any custom Automation documents you create that invoke other AWS services by using actions that call other services\. For example, if you use the `aws:executeAwsApi`, `aws:createStack`, or `aws:copyImage` actions, then you must configure the service role with permission to invoke those services\. You can enable permissions to other AWS services by adding an IAM inline policy to the role\. For more information, see [\(Optional\) add an Automation inline policy to invoke other AWS services](automation-permissions.md#automation-role-add-inline-policy)\.

**Topics**
+ [Properties shared by all actions](#automation-common)
+ [aws:approve – Pause an execution for manual approval](automation-action-approve.md)
+ [aws:assertAwsResourceProperty – Assert an AWS resource state or event state](automation-action-assertAwsResourceProperty.md)
+ [aws:branch – Run conditional automation steps](automation-action-branch.md)
+ [aws:changeInstanceState – Change or assert instance state](automation-action-changestate.md)
+ [aws:copyImage – Copy or encrypt an Amazon Machine Image](automation-action-copyimage.md)
+ [aws:createImage – Create an Amazon Machine Image](automation-action-create.md)
+ [aws:createStack – Create an AWS CloudFormation stack](automation-action-createstack.md)
+ [aws:createTags – Create tags for AWS resources](automation-action-createtag.md)
+ [aws:deleteImage – Delete an Amazon Machine Image](automation-action-delete.md)
+ [aws:deleteStack – Delete an AWS CloudFormation stack](automation-action-deletestack.md)
+ [aws:executeAutomation – Run another automation execution](automation-action-executeAutomation.md)
+ [aws:executeAwsApi – Call and run AWS API actions](automation-action-executeAwsApi.md)
+ [aws:executeScript – Run a script](automation-action-executeScript.md)
+ [aws:executeStateMachine – Run an AWS Step Functions state machine](automation-action-executeStateMachine.md)
+ [aws:invokeLambdaFunction – Invoke an AWS Lambda function](automation-action-lamb.md)
+ [aws:pause – Pause an automation execution](automation-action-pause.md)
+ [aws:runCommand – Run a command on a managed instance](automation-action-runcommand.md)
+ [aws:runInstances – Launch an EC2 instance](automation-action-runinstance.md)
+ [aws:sleep – Delay an automation execution](automation-action-sleep.md)
+ [aws:waitForAwsResourceProperty – Wait on an AWS resource property](automation-action-waitForAwsResourceProperty.md)

## Properties shared by all actions<a name="automation-common"></a>

Common properties are parameters or options that are found in all actions\. Some options define execution behavior for a step, such as how long to wait for a step to complete and what to do if the step fails\. The following properties are common to all actions\.

------
#### [ YAML ]

```
mainSteps:
- name: name
  action: action
  maxAttempts: value
  timeoutSeconds: value
  onFailure: value
  inputs:
    ...
- name: name
  action: action
  maxAttempts: value
  timeoutSeconds: value
  onFailure: value
  inputs:
      ...
```

------
#### [ JSON ]

```
"mainSteps": [
    {
        "name": "name",
        "action": "action",
        "maxAttempts": value,
        "timeoutSeconds": value,
        "onFailure": "value",
        "inputs": {
            ...
        }      
    },
    {
        "name": "name",
        "action": "action",
        "maxAttempts": value,
        "timeoutSeconds": value,
        "onFailure": "value",
        "inputs": {
            ...
        }        
    }
]
```

------

name  
An identifier that must be unique across all step names in the document\.  
Type: String  
Required: Yes

action  
The name of the action the step is to run\. [aws:runCommand – Run a command on a managed instance](automation-action-runcommand.md) is an example of an action you can specify here\. This document provides detailed information about all available actions\.  
Type: String  
Required: Yes

maxAttempts  
The number of times the step should be retried in case of failure\. If the value is greater than 1, the step is not considered to have failed until all retry attempts have failed\. The default value is 1\.  
Type: Integer  
Required: No

timeoutSeconds  
The execution timeout value for the step\. If the timeout is reached and the value of `maxAttempts` is greater than 1, then the step is not considered to have timed out until all retries have been attempted\.   
Type: Integer  
Required: No

onFailure  
Indicates whether the workflow should abort, continue, or go to a different step on failure\. The default value for this option is abort\.  
Type: String  
Valid values: Abort \| Continue \| step:*step\_name*  
Required: No

isEnd  
This option stops an Automation execution at the end of a specific step\. The Automation execution stops if the step execution failed or succeeded\. The default value is false\.  
Type: Boolean  
Valid values: true \| false  
Required: No  
Here is an example of how to enter this option in the mainSteps section of your document:  

```
mainSteps:
- name: InstallMsiPackage
  action: aws:runCommand
  onFailure: step:PostFailure
  maxAttempts: 2
  inputs:
    InstanceIds:
    - i-1234567890EXAMPLE
    - i-abcdefghiEXAMPLE
    DocumentName: AWS-RunPowerShellScript
    Parameters:
      commands:
      - msiexec /i {{packageName}}
  nextStep: TestPackage
- name: TestPackage
  action: aws:invokeLambdaFunction
  maxAttempts: 1
  timeoutSeconds: 500
  inputs:
    FunctionName: TestLambdaFunction
  isEnd: true
```

```
"mainSteps":[
   {
      "name":"InstallMsiPackage",
      "action":"aws:runCommand",
      "onFailure":"step:PostFailure",
      "maxAttempts":2,
      "inputs":{
         "InstanceIds":[
            "i-1234567890EXAMPLE","i-abcdefghiEXAMPLE"
        ],
         "DocumentName":"AWS-RunPowerShellScript",
         "Parameters":{
            "commands":[
               "msiexec /i {{packageName}}"
            ]
         }
      },
      "nextStep":"TestPackage"
   },
   {
      "name":"TestPackage",
      "action":"aws:invokeLambdaFunction",
      "maxAttempts":1,
      "timeoutSeconds":500,
      "inputs":{
         "FunctionName":"TestLambdaFunction"
      },
      "isEnd":true
   }
]
```

nextStep  
Specifies which step in an Automation workflow to process next after successfully completing a step\.  
Here is an example of how to enter this option in the mainSteps section of your document:  

```
mainSteps:
- name: InstallMsiPackage
  action: aws:runCommand
  onFailure: step:PostFailure
  maxAttempts: 2
  inputs:
    InstanceIds:
    - i-1234567890EXAMPLE
    - i-abcdefghiEXAMPLE
    DocumentName: AWS-RunPowerShellScript
    Parameters:
      commands:
      - msiexec /i {{packageName}}
  nextStep: TestPackage
```

```
"mainSteps":[
    {
        "name":"InstallMsiPackage",
        "action":"aws:runCommand",
        "onFailure":"step:PostFailure",
        "maxAttempts":2,
        "inputs":{
            "InstanceIds":[
                "i-1234567890EXAMPLE","i-abcdefghiEXAMPLE"
            ],
            "DocumentName":"AWS-RunPowerShellScript",
            "Parameters":{
                "commands":[
                    "msiexec /i {{packageName}}"
                ]
            }
        },
        "nextStep":"TestPackage"
    }
]
```

isCritical  
Designates a step as critical for the successful completion of the Automation\. If a step with this designation fails, then Automation reports the final status of the Automation as Failed\. The default value for this option is true\.  
Type: Boolean  
Valid values: true \| false  
Required: No  
Here is an example of how to enter this option in the mainSteps section of your document:  

```
mainSteps:
- name: InstallMsiPackage
  action: aws:runCommand
  onFailure: step:SomeOtherStep
  isCritical: false
  maxAttempts: 2
  inputs:
    InstanceIds:
    - i-1234567890EXAMPLE,i-abcdefghiEXAMPLE
    DocumentName: AWS-RunPowerShellScript
    Parameters:
      commands:
      - msiexec /i {{packageName}}
  nextStep: TestPackage
```

```
"mainSteps":[
    {
       "name":"InstallMsiPackage",
       "action":"aws:runCommand",
       "onFailure":"step:SomeOtherStep",
       "isCritical":false,
       "maxAttempts":2,
       "inputs":{
          "InstanceIds":["i-1234567890EXAMPLE","i-abcdefghiEXAMPLE"],
          "DocumentName":"AWS-RunPowerShellScript",
          "Parameters":{
             "commands":[
                "msiexec /i {{packageName}}"
             ]
          }
       },
       "nextStep":"TestPackage"
    }
]
```

inputs  
The properties specific to the action\.  
Type: Map  
Required: Yes
