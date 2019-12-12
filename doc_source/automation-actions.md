# Systems Manager Automation Actions Reference<a name="automation-actions"></a>

This reference describes the actions \(or plugins\) that you can specify in an AWS Systems Manager Automation document\. For information about plugins for other types of SSM documents, see [SSM Document Plugin Reference](ssm-plugins.md)\.

Systems Manager Automation runs steps defined in Automation documents\. Each step is associated with a particular action\. The action determines the inputs, behavior, and outputs of the step\. Steps are defined in the `mainSteps` section of your Automation document\.

You don't need to specify the outputs of an action or step\. The outputs are predetermined by the action associated with the step\. When you specify step inputs in your Automation documents, you can reference one or more outputs from an earlier step\. For example, you can make the output of `aws:runInstances` available for a subsequent `aws:runCommand` action\. You can also reference outputs from earlier steps in the `Output` section of the Automation document\. 

**Important**  
If you run an automation that invokes other services by using an AWS Identity and Access Management \(IAM\) service role, be aware that the service role must be configured with permission to invoke those services\. This requirement applies to all AWS Automation documents \(`AWS-*` documents\) such as the `AWS-ConfigureS3BucketLogging`, `AWS-CreateDynamoDBBackup`, and `AWS-RestartEC2Instance` documents, to name a few\. This requirement also applies to any custom Automation documents you create that invoke other AWS services by using actions that call other services\. For example, if you use the `aws:executeAwsApi`, `aws:createStack`, or `aws:copyImage` actions, then you must configure the service role with permission to invoke those services\. You can enable permissions to other AWS services by adding an IAM inline policy to the role\. For more information, see [\(Optional\) Add an Automation Inline Policy to Invoke Other AWS Services](automation-permissions.md#automation-role-add-inline-policy)\.

**Topics**
+ [Properties Shared by All Actions](#automation-common)
+ [aws:approve – Pause an execution for manual approval](#automation-action-approve)
+ [aws:assertAwsResourceProperty – Assert an AWS resource state or event state](#automation-action-assertAwsResourceProperty)
+ [aws:branch – Run conditional automation steps](#automation-action-branch)
+ [aws:changeInstanceState – Change or assert instance state](#automation-action-changestate)
+ [aws:copyImage – Copy or encrypt an Amazon Machine Image](#automation-action-copyimage)
+ [aws:createImage – Create an Amazon Machine Image](#automation-action-create)
+ [aws:createStack – Create an AWS CloudFormation stack](#automation-action-createstack)
+ [aws:createTags – Create tags for AWS resources](#automation-action-createtag)
+ [aws:deleteImage – Delete an Amazon Machine Image](#automation-action-delete)
+ [aws:deleteStack – Delete an AWS CloudFormation stack](#automation-action-deletestack)
+ [aws:executeAutomation – Run another automation execution](#automation-action-executeAutomation)
+ [aws:executeAwsApi – Call and run AWS API actions](#automation-action-executeAwsApi)
+ [aws:executeScript – Run a script](#automation-action-executeScript)
+ [aws:executeStateMachine – Run an AWS Step Functions state machine](#automation-action-executeStateMachine)
+ [aws:invokeLambdaFunction – Invoke an AWS Lambda function](#automation-action-lamb)
+ [aws:pause – Pause an automation execution](#automation-action-pause)
+ [aws:runCommand – Run a command on a managed instance](#automation-action-runcommand)
+ [aws:runInstances – Launch an Amazon EC2 instance](#automation-action-runinstance)
+ [aws:sleep – Delay an automation execution](#automation-action-sleep)
+ [aws:waitForAwsResourceProperty – Wait on an AWS resource property](#automation-action-waitForAwsResourceProperty)

## Properties Shared by All Actions<a name="automation-common"></a>

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
The name of the action the step is to run\. [aws:runCommand – Run a command on a managed instance](#automation-action-runcommand) is an example of an action you can specify here\. This document provides detailed information about all available actions\.  
Type: String  
Required: Yes

maxAttempts  
The number of times the step should be retried in case of failure\. If the value is greater than 1, the step is not considered to have failed until all retry attempts have failed\. The default value is 1\.  
Type: Integer  
Required: No

timeoutSeconds  
The execution timeout value for the step\. If the timeout is reached and the value of `maxAttempts` is greater than 1, then the step is not considered to have timed out until all retries have been attempted\.   
The `aws:changeInstanceState` action has a default `timeoutSeconds` value of 3600\. For all other actions, there is no default value\.  
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

## aws:approve – Pause an execution for manual approval<a name="automation-action-approve"></a>

Temporarily pauses an Automation execution until designated principals either approve or reject the action\. After the required number of approvals is reached, the Automation execution resumes\. You can insert the approval step any place in the mainSteps section of your Automation document\. 

**Note**  
The default timeout for this action is 7 days \(604800 seconds\)\. You can limit or extend the timeout by specifying the `timeoutSeconds` parameter for an aws:approve step\. If the automation step reaches the timeout value before receiving all required approval decisions, then the step and the automation stop running and return a status of Timed Out\.

In the following example, the aws:approve action temporarily pauses the Automation workflow until one approver either accepts or rejects the workflow\. Upon approval, the document runs a simple PowerShell command\. 

------
#### [ YAML ]

```
---
description: RunInstancesDemo1
schemaVersion: '0.3'
assumeRole: "{{ assumeRole }}"
parameters:
  assumeRole:
    type: String
  message:
    type: String
mainSteps:
- name: approve
  action: aws:approve
  timeoutSeconds: 1000
  onFailure: Abort
  inputs:
    NotificationArn: arn:aws:sns:us-east-2:12345678901:AutomationApproval
    Message: "{{ message }}"
    MinRequiredApprovals: 1
    Approvers:
    - arn:aws:iam::12345678901:user/AWS-User-1
- name: run
  action: aws:runCommand
  inputs:
    InstanceIds:
    - i-1a2b3c4d5e6f7g
    DocumentName: AWS-RunPowerShellScript
    Parameters:
      commands:
      - date
```

------
#### [ JSON ]

```
{
   "description":"RunInstancesDemo1",
   "schemaVersion":"0.3",
   "assumeRole":"{{ assumeRole }}",
   "parameters":{
      "assumeRole":{
         "type":"String"
      },
      "message":{
         "type":"String"
      }
   },
   "mainSteps":[
      {
         "name":"approve",
         "action":"aws:approve",
         "timeoutSeconds":1000,
         "onFailure":"Abort",
         "inputs":{
            "NotificationArn":"arn:aws:sns:us-east-2:12345678901:AutomationApproval",
            "Message":"{{ message }}",
            "MinRequiredApprovals":1,
            "Approvers":[
               "arn:aws:iam::12345678901:user/AWS-User-1"
            ]
         }
      },
      {
         "name":"run",
         "action":"aws:runCommand",
         "inputs":{
            "InstanceIds":[
               "i-1a2b3c4d5e6f7g"
            ],
            "DocumentName":"AWS-RunPowerShellScript",
            "Parameters":{
               "commands":[
                  "date"
               ]
            }
         }
      }
   ]
}
```

------

You can approve or deny Automations that are waiting for approval in the console\.

**To approve or deny waiting Automations**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Automation**\.

1. Choose the option next to an Automation with a status of **Waiting**\.  
![\[Accessing the Approve/Deny Automation page\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/automation-approve-action-aws.png)

1. Choose **Approve/Deny**\.

1. Review the details of the Automation\.

1. Choose either **Approve** or **Deny**, type an optional comment, and then choose **Submit**\.

**Input**

------
#### [ YAML ]

```
NotificationArn: arn:aws:sns:us-west-1:12345678901:Automation-ApprovalRequest
Message: Please approve this step of the Automation.
MinRequiredApprovals: 3
Approvers:
- IamUser1
- IamUser2
- arn:aws:iam::12345678901:user/IamUser3
- arn:aws:iam::12345678901:role/IamRole
```

------
#### [ JSON ]

```
{
   "NotificationArn":"arn:aws:sns:us-west-1:12345678901:Automation-ApprovalRequest",
   "Message":"Please approve this step of the Automation.",
   "MinRequiredApprovals":3,
   "Approvers":[
      "IamUser1",
      "IamUser2",
      "arn:aws:iam::12345678901:user/IamUser3",
      "arn:aws:iam::12345678901:role/IamRole"
   ]
}
```

------

NotificationArn  
The ARN of an Amazon SNS topic for Automation approvals\. When you specify an `aws:approve` step in an Automation document, Automation sends a message to this topic letting principals know that they must either approve or reject an Automation step\. The title of the Amazon SNS topic must be prefixed with "Automation"\.  
Type: String  
Required: No

Message  
The information you want to include in the SNS topic when the approval request is sent\. The maximum message length is 4096 characters\.   
Type: String  
Required: No

MinRequiredApprovals  
The minimum number of approvals required to resume the Automation execution\. If you don't specify a value, the system defaults to one\. The value for this parameter must be a positive number\. The value for this parameter can't exceed the number of approvers defined by the `Approvers` parameter\.   
Type: Integer  
Required: No

Approvers  
A list of AWS authenticated principals who are able to either approve or reject the action\. The maximum number of approvers is 10\. You can specify principals by using any of the following formats:  
+ An AWS Identity and Access Management \(IAM\) user name
+ An IAM user ARN
+ An IAM role ARN
+ An IAM assume role user ARN
Type: StringList  
Required: Yes

**Output**

ApprovalStatus  
The approval status of the step\. The status can be one of the following: Approved, Rejected, or Waiting\. Waiting means that Automation is waiting for input from approvers\.  
Type: String

ApproverDecisions  
A JSON map that includes the approval decision of each approver\.  
Type: MapList

## aws:assertAwsResourceProperty – Assert an AWS resource state or event state<a name="automation-action-assertAwsResourceProperty"></a>

The aws:assertAwsResourceProperty action enables you to assert a specific resource state or event state for a specific Automation step\. For example, you can specify that an Automation step must wait for an Amazon EC2 instance to start\. Then it will call the Amazon EC2 [DescribeInstanceStatus](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DescribeInstanceStatus.html) API action with the DesiredValue property of `running`\. This ensures that the Automation workflow waits for a running instance and then continues when the instance is, in fact, running\.

For more information and examples of how to use this action, see [Invoking Other AWS Services from a Systems Manager Automation Workflow](automation-aws-apis-calling.md)\.

**Note**  
The default timeout value for this action is 3600 seconds \(one hour\)\. You can limit or extend the timeout by specifying the `timeoutSeconds` parameter for an `aws:waitForAwsResourceProperty` step\.

**Input**  
Inputs are defined by the API action that you choose\. 

------
#### [ YAML ]

```
action: aws:assertAwsResourceProperty
inputs:
  Service: The official namespace of the service
  Api: The API action or method name
  API action inputs or parameters: A value
  PropertySelector: Response object
  DesiredValues:
  - Desired property values
```

------
#### [ JSON ]

```
{
  "action": "aws:assertAwsResourceProperty",
  "inputs": {
    "Service":"The official namespace of the service",
    "Api":"The API action or method name",
    "API action inputs or parameters":"A value",
    "PropertySelector": "Response object",
    "DesiredValues": [
      "Desired property values"
    ]
  }
}
```

------

Service  
The AWS service namespace that contains the API action that you want to run\. For example, the namespace for Systems Manager is `ssm`\. The namespace for Amazon EC2 is `ec2`\. You can view a list of supported AWS service namespaces in the [Available Services](https://docs.aws.amazon.com/cli/latest/reference/#available-services) section of the *AWS CLI Command Reference*\.  
Type: String  
Required: Yes

Api  
The name of the API action that you want to run\. You can view the API actions \(also called methods\) by choosing a service in the left navigation on the following [Services Reference](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all API actions \(methods\) for Amazon Relational Database Service \(Amazon RDS\) are listed on the following page: [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\.  
Type: String  
Required: Yes

API action inputs  
One or more API action inputs\. You can view the available inputs \(also called parameters\) by choosing a service in the left navigation on the following [Services Reference](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all methods for Amazon RDS are listed on the following page: [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\. Choose the [describe\_db\_instances](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) method and scroll down to see the available parameters, such as **DBInstanceIdentifier**, **Name**, and **Values**\. Use the following format to specify more than one input\.  

```
inputs:
  Service: The official namespace of the service
  Api: The API action name
  API input 1: A value
  API Input 2: A value
  API Input 3: A value
```

```
"inputs":{
      "Service":"The official namespace of the service",
      "Api":"The API action name",
      "API input 1":"A value",
      "API Input 2":"A value",
      "API Input 3":"A value"
}
```
Type: Determined by chosen API action  
Required: Yes

PropertySelector  
The JSONPath to a specific attribute in the response object\. You can view the response objects by choosing a service in the left navigation on the following [Services Reference](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all methods for Amazon RDS are listed on the following page: [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\. Choose the [describe\_db\_instances](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) method and scroll down to the **Response Structure** section\. **DBInstances** is listed as a response object\.  
Type: Integer, Boolean, String, StringList, StringMap, or MapList  
Required: Yes

DesiredValues  
The expected status or state on which to continue the Automation workflow\. If you specify a Boolean value, you must use a capital letter such as True or False\.  
Type: Varies  
Required: Yes

## aws:branch – Run conditional automation steps<a name="automation-action-branch"></a>

The `aws:branch` action enables you to create a dynamic Automation workflow that evaluates different choices in a single step and then jumps to a different step in the Automation document based on the results of that evaluation\. 

When you specify the `aws:branch` action for a step, you specify `Choices` that the workflow must evaluate\. The `Choices` can be based on either a value that you specified in the `Parameters` section of the Automation document, or a dynamic value generated as the output from the previous step\. The Automation workflow evaluates each choice by using a Boolean expression\. If the first choice is true, then the workflow jumps to the step designated for that choice\. If the first choice is false, the workflow evaluates the next choice\. The workflow continues evaluating each choice until it process a true choice\. The workflow then jumps to the designated step for the true choice\.

If none of the choices are true, the workflow checks to see if the step contains a `default` value\. A default value defines a step that the workflow should jump to if none of the choices are true\. If no `default` value is specified for the step, then the Automation workflow processes the next step in the document\.

The `aws:branch` action supports complex choice evaluations by using a combination of `And`, `Not`, and `Or` operators\. For more information about how to use `aws:branch`, including example documents and examples that use different operators, see [Creating Dynamic Automation Workflows with Conditional Branching](automation-branchdocs.md)\.

**Input**  
Specify one or more `Choices` in a step\. The `Choices` can be based on either a value that you specified in the `Parameters` section of the Automation document, or a dynamic value generated as the output from the previous step\. Here is a YAML sample that evaluates a parameter\.

```
mainSteps:
- name: chooseOS
  action: aws:branch
  inputs:
    Choices:
    - NextStep: runWindowsCommand
      Variable: "{{Name of a parameter defined in the Parameters section. For example: OS_name}}"
      StringEquals: windows
    - NextStep: runLinuxCommand
      Variable: "{{Name of a parameter defined in the Parameters section. For example: OS_name}}"
      StringEquals: linux
    Default:
      sleep3
```

Here is a YAML sample that evaluates output from a previous step\.

```
mainSteps:
- name: chooseOS
  action: aws:branch
  inputs:
    Choices:
    - NextStep: runPowerShellCommand
      Variable: "{{Name of a response object. For example: GetInstance.platform}}"
      StringEquals: Windows
    - NextStep: runShellCommand
      Variable: "{{Name of a response object. For example: GetInstance.platform}}"
      StringEquals: Linux
    Default:
      sleep3
```

Choices  
One or more expressions that the Automation should evaluate when determining the next step to process\. Choices are evaluated by using a Boolean expression\. Each choice must define the following options:  
+ **NextStep**: The next step in the Automation document to process if the designated choice is true\.
+ **Variable**: Specify either the name of a parameter that is defined in the `Parameters` section of the Automation document\. Or specify an output object from a previous step in the Automation document\. For more information about creating variables for `aws:branch`, see [About Creating the Output Variable](automation-branchdocs.md#automation-branchdocs-awsbranch-creating-output)\.
+ **Operation**: The criteria used to evaluate the choice\. The `aws:branch` action supports the following operations:

**String operations**
  + StringEquals
  + EqualsIgnoreCase
  + StartsWith
  + EndsWith
  + Contains

**Numeric operations**
  + NumericEquals
  + NumericGreater
  + NumericLesser
  + NumericGreaterOrEquals
  + NumericLesser
  + NumericLesserOrEquals

**Boolean operation**
  + BooleanEquals
**Important**  
When you create an Automation document, the system validates each operation in the document\. If an operation is not supported, the system returns an error when you try to create the document\.

Default  
The name of a step the workflow should jump to if none of the `Choices` are true\.  
Type: String  
Required: No

**Note**  
The `aws:branch` action supports `And`, `Or`, and `Not` operators\. For examples of `aws:branch` that use operators, see [Creating Dynamic Automation Workflows with Conditional Branching](automation-branchdocs.md)\.

## aws:changeInstanceState – Change or assert instance state<a name="automation-action-changestate"></a>

Changes or asserts the state of the instance\.

This action can be used in assert mode \(do not run the API to change the state but verify the instance is in the desired state\.\) To use assert mode, set the `CheckStateOnly` parameter to true\. This mode is useful when running the Sysprep command on Windows, which is an asynchronous command that can run in the background for a long time\. You can ensure that the instance is stopped before you create an AMI\.

**Note**  
The default timeout value for this action is 3600 seconds \(one hour\)\. You can limit or extend the timeout by specifying the `timeoutSeconds` parameter for an `aws:changeInstanceState` step\.

**Input**

------
#### [ YAML ]

```
name: stopMyInstance
action: aws:changeInstanceState
maxAttempts: 3
timeoutSeconds: 3600
onFailure: Abort
inputs:
  InstanceIds:
  - i-1234567890abcdef0
  CheckStateOnly: true
  DesiredState: stopped
```

------
#### [ JSON ]

```
{
    "name":"stopMyInstance",
    "action": "aws:changeInstanceState",
    "maxAttempts": 3,
    "timeoutSeconds": 3600,
    "onFailure": "Abort",
    "inputs": {
        "InstanceIds": ["i-1234567890abcdef0"],
        "CheckStateOnly": true,
        "DesiredState": "stopped"
    }
}
```

------

InstanceIds  
The IDs of the instances\.  
Type: StringList  
Required: Yes

CheckStateOnly  
If false, sets the instance state to the desired state\. If true, asserts the desired state using polling\.  
Default: `false`  
Type: Boolean  
Required: No

DesiredState  
The desired state\. When set to `running`, this action waits for the Amazon EC2 state to be `Running`, the Instance Status to be `OK`, and the System Status to be `OK` before completing\.  
Type: String  
Valid values: `running` \| `stopped` \| `terminated`  
Required: Yes

Force  
If set, forces the instances to stop\. The instances do not have an opportunity to flush file system caches or file system metadata\. If you use this option, you must perform file system check and repair procedures\. This option is not recommended for Windows instances\.  
Type: Boolean  
Required: No

AdditionalInfo  
Reserved\.  
Type: String  
Required: No

**Output**  
None

## aws:copyImage – Copy or encrypt an Amazon Machine Image<a name="automation-action-copyimage"></a>

Copies an AMI from any region into the current region\. This action can also encrypt the new AMI\.

**Input**  
This action supports most CopyImage parameters\. For more information, see [CopyImage](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CopyImage.html)\.

The following example creates a copy of an AMI in the Seoul region \(`SourceImageID`: ami\-0fe10819\. `SourceRegion`: ap\-northeast\-2\)\. The new AMI is copied to the region where you initiated the Automation action\. The copied AMI will be encrypted because the optional `Encrypted` flag is set to `true`\.

------
#### [ YAML ]

```
name: createEncryptedCopy
action: aws:copyImage
maxAttempts: 3
onFailure: Abort
inputs:
  SourceImageId: ami-0fe10819
  SourceRegion: ap-northeast-2
  ImageName: Encrypted Copy of LAMP base AMI in ap-northeast-2
  Encrypted: true
```

------
#### [ JSON ]

```
{   
    "name": "createEncryptedCopy",
    "action": "aws:copyImage",
    "maxAttempts": 3,
    "onFailure": "Abort",
    "inputs": {
        "SourceImageId": "ami-0fe10819",
        "SourceRegion": "ap-northeast-2",
        "ImageName": "Encrypted Copy of LAMP base AMI in ap-northeast-2",
        "Encrypted": true
    }   
}
```

------

SourceRegion  
The region where the source AMI currently exists\.  
Type: String  
Required: Yes

SourceImageId  
The AMI ID to copy from the source region\.  
Type: String  
Required: Yes

ImageName  
The name for the new image\.  
Type: String  
Required: Yes

ImageDescription  
A description for the target image\.  
Type: String  
Required: No

Encrypted  
Encrypt the target AMI\.  
Type: Boolean  
Required: No

KmsKeyId  
The full Amazon Resource Name \(ARN\) of the AWS Key Management Service CMK to use when encrypting the snapshots of an image during a copy operation\. For more information, see [CopyImage](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/api_copyimage.html)\.  
Type: String  
Required: No

ClientToken  
A unique, case\-sensitive identifier that you provide to ensure request idempotency\. For more information, see [CopyImage](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/api_copyimage.html)\.  
Type: String  
Required: NoOutput

ImageId  
The ID of the copied image\.

ImageState  
The state of the copied image\.  
Valid values: `available` \| `pending` \| `failed`

## aws:createImage – Create an Amazon Machine Image<a name="automation-action-create"></a>

Creates a new AMI from an instance that is either running or stopped\. 

**Input**  
This action supports most CreateImage parameters\. For more information, see [CreateImage](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CreateImage.html)\.

------
#### [ YAML ]

```
name: createMyImage
action: aws:createImage
maxAttempts: 3
onFailure: Abort
inputs:
  InstanceId: i-1234567890abcdef0
  ImageName: AMI Created on{{global:DATE_TIME}}
  NoReboot: true
  ImageDescription: My newly created AMI
```

------
#### [ JSON ]

```
{
    "name": "createMyImage",
    "action": "aws:createImage",
    "maxAttempts": 3,
    "onFailure": "Abort",
    "inputs": {
        "InstanceId": "i-1234567890abcdef0",
        "ImageName": "AMI Created on{{global:DATE_TIME}}",
        "NoReboot": true,
        "ImageDescription": "My newly created AMI"
    }
}
```

------

InstanceId  
The ID of the instance\.  
Type: String  
Required: Yes

ImageName  
The name for the image\.  
Type: String  
Required: Yes

ImageDescription  
A description of the image\.  
Type: String  
Required: No

NoReboot  
A boolean literal\.  
By default, Amazon EC2 attempts to shut down and reboot the instance before creating the image\. If the **No Reboot** option is set to `true`, Amazon EC2 doesn't shut down the instance before creating the image\. When this option is used, file system integrity on the created image can't be guaranteed\.   
If you do not want the instance to run after you create an AMI image from it, first use the [aws:changeInstanceState – Change or assert instance state](#automation-action-changestate) plugin to stop the instance, and then use this `aws:createImage` plugin with the **NoReboot** option set to `true`\.  
Type: Boolean  
Required: No

BlockDeviceMappings  
The block devices for the instance\.  
Type: Map  
Required: NoOutput

ImageId  
The ID of the newly created image\.

ImageState  
An execution script provided as a string literal value\. If a literal value is entered, then it must be Base64\-encoded\.  
Required: No

## aws:createStack – Create an AWS CloudFormation stack<a name="automation-action-createstack"></a>

Creates a new AWS CloudFormation stack from a template\.

**Input**

------
#### [ YAML ]

```
name: makeStack
action: aws:createStack
maxAttempts: 1
onFailure: Abort
inputs:
  Capabilities:
  - CAPABILITY_IAM
  StackName: myStack
  TemplateURL: http://s3.amazonaws.com/mybucket/myStackTemplate
  TimeoutInMinutes: 5
```

------
#### [ JSON ]

```
{
    "name": "makeStack",
    "action": "aws:createStack",
    "maxAttempts": 1,
    "onFailure": "Abort",
    "inputs": {
        "Capabilities": [
            "CAPABILITY_IAM"
        ],
        "StackName": "myStack",
        "TemplateURL": "http://s3.amazonaws.com/mybucket/myStackTemplate",
        "TimeoutInMinutes": 5
    }
}
```

------

Capabilities  
A list of values that you specify before AWS CloudFormation can create certain stacks\. Some stack templates include resources that can affect permissions in your AWS account\. For example, creating new AWS Identity and Access Management \(IAM\) users can affect permissions in your account\. For those stacks, you must explicitly acknowledge their capabilities by specifying this parameter\.   
The only valid values are `CAPABILITY_IAM` and `CAPABILITY_NAMED_IAM`\. The following resources require you to specify this parameter\.  
+ [AWS::IAM::AccessKey](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-iam-accesskey.html)
+ [AWS::IAM::Group](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-iam-group.html)
+ [AWS::IAM::InstanceProfile](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-iam-instanceprofile.html)
+ [AWS::IAM::Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-iam-policy.html)
+ [AWS::IAM::Role](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-iam-role.html)
+ [AWS::IAM::User](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-iam-user.html)
+ [AWS::IAM::UserToGroupAddition](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-iam-addusertogroup.html)
If your stack template contains these resources, we recommend that you review all permissions associated with them and edit their permissions, if necessary\.   
If you have IAM resources, you can specify either capability\. If you have IAM resources with custom names, you must specify `CAPABILITY_NAMED_IAM`\. If you don't specify this parameter, this action returns an `InsufficientCapabilities` error\.   
For more information, see [Acknowledging IAM Resources in AWS CloudFormation Templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-iam-template.html#capabilities)\.   
Type: array of Strings  
Valid Values: `CAPABILITY_IAM | CAPABILITY_NAMED_IAM`  
Required: No

ClientRequestToken  
A unique identifier for this CreateStack request\. Specify this token if you set maxAttempts in this step to a value greater than 1\. By specifying this token, AWS CloudFormation knows that you're not attempting to create a new stack with the same name\.  
Type: String  
Required: No  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: \[a\-zA\-Z0\-9\]\[\-a\-zA\-Z0\-9\]\*

DisableRollback  
Set to `true` to disable rollback of the stack if stack creation failed\.  
Conditional: You can specify either the `DisableRollback` parameter or the `OnFailure` parameter, but not both\.   
Default: `false`  
Type: Boolean  
Required: No

NotificationARNs  
The Amazon SNS topic ARNs for publishing stack\-related events\. You can find SNS topic ARNs using the Amazon SNS console, [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.   
Type: array of Strings  
Array Members: Maximum number of 5 items\.  
Required: No

OnFailure  
Determines the action to take if stack creation failed\. You must specify `DO_NOTHING`, `ROLLBACK`, or `DELETE`\.  
Conditional: You can specify either the `OnFailure` parameter or the `DisableRollback` parameter, but not both\.   
Default: `ROLLBACK`  
Type: String  
Valid Values:` DO_NOTHING | ROLLBACK | DELETE`  
Required: No

Parameters  
A list of `Parameter` structures that specify input parameters for the stack\. For more information, see the [Parameter](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_Parameter.html) data type\.   
Type: array of [Parameter](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_Parameter.html) objects   
Required: No

ResourceTypes  
The template resource types that you have permissions to work with for this create stack action\. For example: `AWS::EC2::Instance`, `AWS::EC2::*`, or `Custom::MyCustomInstance`\. Use the following syntax to describe template resource types\.  
+ For all AWS resources:

  ```
  AWS::*
  ```
+ For all custom resources:

  ```
  Custom::*
  ```
+ For a specific custom resource:

  ```
  Custom::logical_ID
  ```
+ For all resources of a particular AWS service:

  ```
  AWS::service_name::*
  ```
+ For a specific AWS resource:

  ```
  AWS::service_name::resource_logical_ID
  ```
If the list of resource types doesn't include a resource that you're creating, the stack creation fails\. By default, AWS CloudFormation grants permissions to all resource types\. IAM uses this parameter for AWS CloudFormation\-specific condition keys in IAM policies\. For more information, see [Controlling Access with AWS Identity and Access Management](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-iam-template.html)\.   
Type: array of Strings  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Required: No

RoleARN  
The Amazon Resource Name \(ARN\) of an IAM role that AWS CloudFormation assumes to create the stack\. AWS CloudFormation uses the role's credentials to make calls on your behalf\. AWS CloudFormation always uses this role for all future operations on the stack\. As long as users have permission to operate on the stack, AWS CloudFormation uses this role even if the users don't have permission to pass it\. Ensure that the role grants the least amount of privileges\.   
If you don't specify a value, AWS CloudFormation uses the role that was previously associated with the stack\. If no role is available, AWS CloudFormation uses a temporary session that is generated from your user credentials\.   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Required: No

StackName  
The name that is associated with the stack\. The name must be unique in the region in which you are creating the stack\.  
A stack name can contain only alphanumeric characters \(case sensitive\) and hyphens\. It must start with an alphabetic character and cannot be longer than 128 characters\. 
Type: String  
Required: Yes

StackPolicyBody  
Structure containing the stack policy body\. For more information, see [Prevent Updates to Stack Resources](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/protect-stack-resources.html)\.  
Conditional: You can specify either the `StackPolicyBody` parameter or the `StackPolicyURL` parameter, but not both\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 16384\.  
Required: No

StackPolicyURL  
Location of a file containing the stack policy\. The URL must point to a policy located in an Amazon S3 bucket in the same region as the stack\. The maximum file size allowed for the stack policy is 16 KB\.  
Conditional: You can specify either the `StackPolicyBody` parameter or the `StackPolicyURL` parameter, but not both\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1350\.  
Required: No

Tags  
Key\-value pairs to associate with this stack\. AWS CloudFormation also propagates these tags to the resources created in the stack\. You can specify a maximum number of 10 tags\.   
Type: array of [Tag](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_Tag.html) objects   
Required: No

TemplateBody  
Structure containing the template body with a minimum length of 1 byte and a maximum length of 51,200 bytes\. For more information, see [Template Anatomy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html)\.   
Conditional: You can specify either the `TemplateBody` parameter or the `TemplateURL` parameter, but not both\.   
Type: String  
Length Constraints: Minimum length of 1\.  
Required: No

TemplateURL  
Location of a file containing the template body\. The URL must point to a template that is located in an Amazon S3 bucket\. The maximum size allowed for the template is 460,800 bytes\. For more information, see [Template Anatomy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html)\.   
Conditional: You can specify either the `TemplateBody` parameter or the `TemplateURL` parameter, but not both\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Required: No

TimeoutInMinutes  
The amount of time that can pass before the stack status becomes `CREATE_FAILED`\. If `DisableRollback` is not set or is set to `false`, the stack will be rolled back\.   
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: No

### Outputs<a name="automation-action-createstack-output"></a>

StackId  
Unique identifier of the stack\.  
Type: String

StackStatus  
Current status of the stack\.  
Type: String  
Valid Values: `CREATE_IN_PROGRESS | CREATE_FAILED | CREATE_COMPLETE | ROLLBACK_IN_PROGRESS | ROLLBACK_FAILED | ROLLBACK_COMPLETE | DELETE_IN_PROGRESS | DELETE_FAILED | DELETE_COMPLETE | UPDATE_IN_PROGRESS | UPDATE_COMPLETE_CLEANUP_IN_PROGRESS | UPDATE_COMPLETE | UPDATE_ROLLBACK_IN_PROGRESS | UPDATE_ROLLBACK_FAILED | UPDATE_ROLLBACK_COMPLETE_CLEANUP_IN_PROGRESS | UPDATE_ROLLBACK_COMPLETE | REVIEW_IN_PROGRESS`  
Required: Yes

StackStatusReason  
Success or failure message associated with the stack status\.  
Type: String  
Required: No  
For more information, see [CreateStack](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_CreateStack.html)\.

### Security Considerations<a name="automation-action-createstack-security"></a>

Before you can use the `aws:createStack` action, you must assign the following policy to the IAM Automation assume role\. For more information about the assume role, see [Task 1: Create a Service Role for Automation](automation-permissions.md#automation-role)\. 

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "sqs:*",
            "cloudformation:CreateStack",
            "cloudformation:DescribeStacks"
         ],
         "Resource":"*"
      }
   ]
}
```

## aws:createTags – Create tags for AWS resources<a name="automation-action-createtag"></a>

Create new tags for Amazon EC2 instances or Systems Manager managed instances\.

**Input**  
This action supports most EC2 CreateTags and SSM AddTagsToResource parameters\. For more information, see [CreateTags](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/api_createtags.html) and [AddTagsToResource](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/api_addtagstoresource.html)\.

The following example shows how to tag an AMI and an instance as being production resources for a particular department\.

------
#### [ YAML ]

```
name: createTags
action: aws:createTags
maxAttempts: 3
onFailure: Abort
inputs:
  ResourceType: EC2
  ResourceIds:
  - ami-9a3768fa
  - i-02951acd5111a8169
  Tags:
  - Key: production
    Value: ''
  - Key: department
    Value: devops
```

------
#### [ JSON ]

```
{
    "name": "createTags",
    "action": "aws:createTags",
    "maxAttempts": 3,
    "onFailure": "Abort",
    "inputs": {
        "ResourceType": "EC2",
        "ResourceIds": [
            "ami-9a3768fa",
            "i-02951acd5111a8169"
        ],
        "Tags": [
            {
                "Key": "production",
                "Value": ""
            },
            {
                "Key": "department",
                "Value": "devops"
            }
        ]
    }
}
```

------

ResourceIds  
The IDs of the resource\(s\) to be tagged\. If resource type is not “EC2”, this field can contain only a single item\.  
Type: String List  
Required: Yes

Tags  
The tags to associate with the resource\(s\)\.  
Type: List of Maps  
Required: Yes

ResourceType  
The type of resource\(s\) to be tagged\. If not supplied, the default value of “EC2” is used\.  
Type: String  
Required: No  
Valid Values: `EC2` \| `ManagedInstance` \| `MaintenanceWindow` \| `Parameter`

**Output**  
None

## aws:deleteImage – Delete an Amazon Machine Image<a name="automation-action-delete"></a>

Deletes the specified image and all related snapshots\.

**Input**  
This action supports only one parameter\. For more information, see the documentation for [DeregisterImage](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DeregisterImage.html) and [DeleteSnapshot](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DeleteSnapshot.html)\.

------
#### [ YAML ]

```
name: deleteMyImage
action: aws:deleteImage
maxAttempts: 3
timeoutSeconds: 180
onFailure: Abort
inputs:
  ImageId: ami-12345678
```

------
#### [ JSON ]

```
{
    "name": "deleteMyImage",
    "action": "aws:deleteImage",
    "maxAttempts": 3,
    "timeoutSeconds": 180,
    "onFailure": "Abort",
    "inputs": {
        "ImageId": "ami-12345678"
    }
}
```

------

ImageId  
The ID of the image to be deleted\.  
Type: String  
Required: Yes

**Output**  
None

## aws:deleteStack – Delete an AWS CloudFormation stack<a name="automation-action-deletestack"></a>

Deletes an AWS CloudFormation stack\.

**Input**

------
#### [ YAML ]

```
name: deleteStack
action: aws:deleteStack
maxAttempts: 1
onFailure: Abort
inputs:
  StackName: "{{stackName}}"
```

------
#### [ JSON ]

```
{
   "name":"deleteStack",
   "action":"aws:deleteStack",
   "maxAttempts":1,
   "onFailure":"Abort",
   "inputs":{
      "StackName":"{{stackName}}"
   }
}
```

------

ClientRequestToken  
A unique identifier for this `DeleteStack` request\. Specify this token if you plan to retry requests so that AWS CloudFormation knows that you're not attempting to delete a stack with the same name\. You can retry `DeleteStack` requests to verify that AWS CloudFormation received them\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: \[a\-zA\-Z\]\[\-a\-zA\-Z0\-9\]\*  
Required: No

RetainResources\.member\.N  
This input applies only to stacks that are in a `DELETE_FAILED` state\. A list of logical resource IDs for the resources you want to retain\. During deletion, AWS CloudFormation deletes the stack, but does not delete the retained resources\.  
Retaining resources is useful when you can't delete a resource, such as a non\-empty Amazon S3 bucket, but you want to delete the stack\.  
Type: array of strings  
Required: No

RoleARN  
The Amazon Resource Name \(ARN\) of an IAM role that AWS CloudFormation assumes to create the stack\. AWS CloudFormation uses the role's credentials to make calls on your behalf\. AWS CloudFormation always uses this role for all future operations on the stack\. As long as users have permission to operate on the stack, AWS CloudFormation uses this role even if the users don't have permission to pass it\. Ensure that the role grants the least amount of privileges\.   
If you don't specify a value, AWS CloudFormation uses the role that was previously associated with the stack\. If no role is available, AWS CloudFormation uses a temporary session that is generated from your user credentials\.   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Required: No

StackName  
The name or the unique stack ID that is associated with the stack\.  
Type: String  
Required: Yes

### Security Considerations<a name="automation-action-deletestack-security"></a>

Before you can use the `aws:deleteStack` action, you must assign the following policy to the IAM Automation assume role\. For more information about the assume role, see [Task 1: Create a Service Role for Automation](automation-permissions.md#automation-role)\. 

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "sqs:*",
            "cloudformation:DeleteStack",
            "cloudformation:DescribeStacks"
         ],
         "Resource":"*"
      }
   ]
}
```

## aws:executeAutomation – Run another automation execution<a name="automation-action-executeAutomation"></a>

Runs a secondary Automation workflow by calling a secondary Automation document\. With this action, you can create Automation documents for your most common workflows, and reference those documents during an Automation execution\. This action can simplify your Automation documents by removing the need to duplicate steps across similar documents\.

The secondary Automation runs in the context of the user who initiated the primary Automation\. This means that the secondary Automation uses the same IAM role or user account as the user who started the first Automation\.

**Important**  
If you specify parameters in a secondary Automation that use an assume role \(a role that uses the iam:passRole policy\), then the user or role that initiated the primary Automation must have permission to pass the assume role specified in the secondary Automation\. For more information about setting up an assume role for Automation, see [Method 2: Use IAM to Configure Roles for Automation](automation-permissions.md)\.

**Input**

------
#### [ YAML ]

```
name: Secondary_Automation_Workflow
action: aws:executeAutomation
maxAttempts: 3
timeoutSeconds: 3600
onFailure: Abort
inputs:
  DocumentName: secondaryWorkflow
  RuntimeParameters:
    instanceIds:
    - i-1234567890abcdef0
```

------
#### [ JSON ]

```
{
   "name":"Secondary_Automation_Workflow",
   "action":"aws:executeAutomation",
   "maxAttempts":3,
   "timeoutSeconds":3600,
   "onFailure":"Abort",
   "inputs":{
      "DocumentName":"secondaryWorkflow",
      "RuntimeParameters":{
         "instanceIds":[
            "i-1234567890abcdef0"
         ]
      }
   }
}
```

------

DocumentName  
The name of the secondary Automation document to run during the step\. The document must belong to the same AWS account as the primary Automation document\.  
Type: String  
Required: Yes

DocumentVersion  
The version of the secondary Automation document to run\. If not specified, Automation runs the default document version\.  
Type: String  
Required: Yes

RuntimeParameters  
Required parameters for the secondary document execution\. The mapping uses the following format: \{"parameter1" : \["value1"\], "parameter2" : \["value2"\] \}  
Type: Map  
Required: NoOutput

Output  
The output generated by the secondary execution\. You can reference the output by using the following format: *Secondary\_Automation\_Step\_Name*\.Output  
Type: StringList

ExecutionId  
The execution ID of the secondary execution\.  
Type: String

Status  
The status of the secondary execution\.  
Type: String

## aws:executeAwsApi – Call and run AWS API actions<a name="automation-action-executeAwsApi"></a>

Calls and runs AWS API actions\. Most API actions are supported, although not all API actions have been tested\. For example, the following API actions are supported: [CreateImage](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CreateImage.html), [Delete bucket](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketDELETE.html), [RebootDBInstance](https://docs.aws.amazon.com/AmazonRDS/latest/APIReference/API_RebootDBInstance.html), and [CreateGroups](https://docs.aws.amazon.com/IAM/latest/APIReference/API_CreateGroup.html), to name a few\. Streaming API actions, such as the [Get Object](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectGET.html) action, aren't supported\. For more information and examples of how to use this action, see [Invoking Other AWS Services from a Systems Manager Automation Workflow](automation-aws-apis-calling.md)\.

**Input**  
Inputs are defined by the API action that you choose\. 

------
#### [ YAML ]

```
action: aws:executeAwsApi
inputs:
  Service: The official namespace of the service
  Api: The API action or method name
  API action inputs or parameters: A value
outputs: # These are user-specified outputs
- Name: The name for a user-specified output key
  Selector: A response object specified by using jsonpath format
  Type: The data type
```

------
#### [ JSON ]

```
{
   "action":"aws:executeAwsApi",
   "inputs":{
      "Service":"The official namespace of the service",
      "Api":"The API action or method name",
      "API action inputs or parameters":"A value"
   },
   "outputs":[ These are user-specified outputs
      {
         "Name":"The name for a user-specified output key",
         "Selector":"A response object specified by using JSONPath format",
         "Type":"The data type"
      }
   ]
}
```

------

Service  
The AWS service namespace that contains the API action that you want to run\. For example, the namespace for Systems Manager is `ssm`\. The namespace for Amazon EC2 is `ec2`\. You can view a list of supported AWS service namespaces in the [Available Services](https://docs.aws.amazon.com/cli/latest/reference/#available-services) section of the *AWS CLI Command Reference*\.  
Type: String  
Required: Yes

Api  
The name of the API action that you want to run\. You can view the API actions \(also called methods\) by choosing a service in the left navigation on the following [Services Reference](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all API actions \(methods\) for Amazon RDS are listed on the following page: [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\.  
Type: String  
Required: Yes

API action inputs  
One or more API action inputs\. You can view the available inputs \(also called parameters\) by choosing a service in the left navigation on the following [Services Reference](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all methods for Amazon RDS are listed on the following page: [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\. Choose the [describe\_db\_instances](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) method and scroll down to see the available parameters, such as **DBInstanceIdentifier**, **Name**, and **Values**\.  

```
inputs:
  Service: The official namespace of the service
  Api: The API action name
  API input 1: A value
  API Input 2: A value
  API Input 3: A value
```

```
"inputs":{
      "Service":"The official namespace of the service",
      "Api":"The API action name",
      "API input 1":"A value",
      "API Input 2":"A value",
      "API Input 3":"A value"
}
```
Type: Determined by chosen API action  
Required: Yes

Name  
A name for the output\.  
Type: String  
Required: Yes

Selector  
The JSONPath to a specific attribute in the response object\. You can view the response objects by choosing a service in the left navigation on the following [Services Reference](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all methods for Amazon RDS are listed on the following page: [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\. Choose the [describe\_db\_instances](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) method and scroll down to the **Response Structure** section\. **DBInstances** is listed as a response object\.  
Type: Integer, Boolean, String, StringList, StringMap, or MapList  
Required: Yes

Type  
The data type for the response element\.  
Type: Varies  
Required: Yes

## aws:executeScript – Run a script<a name="automation-action-executeScript"></a>

Runs the python or PowerShell script provided using the specified runtime and handler\. \(For PowerShell, handler is not required\.\)

**Input**  
Provide the runtime and handler required to run the provided Python 3\.6, Python 3\.7, or PowerShell Core 6\.0 script\.

------
#### [ YAML ]

```
action: "aws:executeScript"
inputs: 
 Runtime: "python3.6"
 Handler: "script_handler"
 InputPayload: 
  "parameter1": "parameter_value1"
  "parameter2": "parameter_value2"
 Script: 
  - 
   "def script_handler(events, context):"
  - 
   "(script commands)"
 Attachment: "zip-file-name-1.zip"
```

------
#### [ JSON ]

```
{
    "action": "aws:executeScript",
    "inputs": {
        "Runtime": "python3.6",
        "Handler": "script_handler",
        "InputPayload": {
            "parameter1": "parameter_value1",
            "parameter2": "parameter_value2"
        },
        "Script": [
            "def script_handler(events, context):",
            "(script commands)"
        ],
        "Attachment": "zip-file-name-1.zip"
    }
}
```

------

Runtime  
The runtime language to be used for executing the provided script\. Currently, aws:executeScript supports Python 3\.6 \(python3\.6\), Python 3\.7 \(python3\.7\), and PowerShell Core 6\.0 \(dotnetcore2\.1\) scripts\.  
Supported values: **python3\.6** \| **python3\.7** \| **dotnetcore2\.1**  
Type: String  
Required: Yes

Handler  
The entry for script execution, usually a function name\. You must ensure the function defined in the handler has two parameters, `events` and `context`\. \(Not required for PowerShell\.\)  
Type: String  
Required: Yes \(Python\) \| No \(PowerShell\)

InputPayload  
A JSON or YAML object that will be passed to the first parameter of the handler\. This can be used to pass input data to the script\.  
Type: String  
Required: No

Script  
An embedded script that you want to run during the automation execution\.  
Type: String  
Required: No \(Python\) \| Yes \(PowerShell\)

Attachment  
The name of a standalone script file or \.zip file that can be invoked by the action\. To invoke a file for python, use the `filename.method_name` format in `Handler`\. For PowerShell, invoke the attachment using and inline script\. Gzip is not supported\.  
Type: String  
Required: No

## aws:executeStateMachine – Run an AWS Step Functions state machine<a name="automation-action-executeStateMachine"></a>

Run an AWS Step Functions state machine\.

**Input**

This action supports most parameters for the Step Functions [StartExecution](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html) API action\.

------
#### [ YAML ]

```
name: executeTheStateMachine
action: aws:executeStateMachine
inputs:
  stateMachineArn: StateMachine_ARN
  input: '{"parameters":"values"}'
  name: name
```

------
#### [ JSON ]

```
{
    "name": "executeTheStateMachine",
    "action": "aws:executeStateMachine",
    "inputs": {
        "stateMachineArn": "StateMachine_ARN",
        "input": "{\"parameters\":\"values\"}",
        "name": "name"
    }
}
```

------

stateMachineArn  
The ARN of the Step Functions state machine\.  
Type: String  
Required: Yes

name  
The name of the execution\.  
Type: String  
Required: No

input  
A string that contains the JSON input data for the execution\.  
Type: String  
Required: No

## aws:invokeLambdaFunction – Invoke an AWS Lambda function<a name="automation-action-lamb"></a>

Invokes the specified Lambda function\.

**Input**  
This action supports most invoked parameters for the Lambda service\. For more information, see [Invoke](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html)\.

------
#### [ YAML ]

```
name: invokeMyLambdaFunction
action: aws:invokeLambdaFunction
maxAttempts: 3
timeoutSeconds: 120
onFailure: Abort
inputs:
  FunctionName: MyLambdaFunction
```

------
#### [ JSON ]

```
{
    "name": "invokeMyLambdaFunction",
    "action": "aws:invokeLambdaFunction",
    "maxAttempts": 3,
    "timeoutSeconds": 120,
    "onFailure": "Abort",
    "inputs": {
        "FunctionName": "MyLambdaFunction"
    }
}
```

------

FunctionName  
The name of the Lambda function\. This function must exist\.  
Type: String  
Required: Yes

Qualifier  
The function version or alias name\.  
Type: String  
Required: No

InvocationType  
The invocation type\. The default is `RequestResponse`\.  
Type: String  
Valid values: `Event` \| `RequestResponse` \| `DryRun`  
Required: No

LogType  
If `Tail`, the invocation type must be `RequestResponse`\. AWS Lambda returns the last 4 KB of log data produced by your Lambda function, base64\-encoded\.  
Type: String  
Valid values: `None` \| `Tail`  
Required: No

ClientContext  
The client\-specific information\.  
Required: No

Payload  
The JSON input for your Lambda function\.  
Required: NoOutput

StatusCode  
The function execution status code\.

FunctionError  
Indicates whether an error occurred while running the Lambda function\. If an error occurred, this field will show either `Handled` or `Unhandled`\. `Handled` errors are reported by the function\. `Unhandled` errors are detected and reported by AWS Lambda\.

LogResult  
The base64\-encoded logs for the Lambda function invocation\. Logs are present only if the invocation type is `RequestResponse`, and the logs were requested\.

Payload  
The JSON representation of the object returned by the Lambda function\. Payload is present only if the invocation type is `RequestResponse`\.

## aws:pause – Pause an automation execution<a name="automation-action-pause"></a>

This action pauses the Automation execution\. Once paused, the execution status is *Waiting*\. To continue the Automation execution, use the [SendAutomationSignal](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendAutomationSignal.html) API action with the Resume signal type\. 

**Input**  
The input is as follows\.

------
#### [ YAML ]

```
name: pauseThis
action: aws:pause
inputs: {}
```

------
#### [ JSON ]

```
{
    "name": "pauseThis",
    "action": "aws:pause",
    "inputs": {}
}
```

------Output

None  

## aws:runCommand – Run a command on a managed instance<a name="automation-action-runcommand"></a>

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
The instance IDs where you want the command to run\. You can specify a maximum of 50 IDs\. If you don't want to specify individual instance IDs, then you can send commands to a fleet of instances by using the Targets parameter\. The Targets parameter accepts Amazon EC2 tags\. For more information about how to use the Targets parameter, see [Using Targets and Rate Controls to Send Commands to a Fleet](send-commands-multiple.md)\.  
Type: StringList  
Required: No \(If you don't specify InstanceIds, then you must specify the Targets parameter\.\)

Targets  
An array of search criteria that targets instances by using a Key,Value combination that you specify\. Targets is required if you don't provide one or more instance IDs in the call\. For more information about how to use the Targets parameter, see [Using Targets and Rate Controls to Send Commands to a Fleet](send-commands-multiple.md)\.  
Type: MapList \(The schema of the map in the list must match the object\. For information, see [Target](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_Target.html) in the *AWS Systems Manager API Reference*\.  
Required: No \(If you don't specify Targets, then you must specify InstanceIds\.\)  
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
        ]
        "Parameters": {
            "source": "https://my-s3-url.com/MyModule.zip ",
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
            "source": "https://my-s3-url.com/MyModule.zip ",
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

## aws:runInstances – Launch an Amazon EC2 instance<a name="automation-action-runinstance"></a>

Launches a new instance\.

**Input**  
The action supports most API parameters\. For more information, see the [RunInstances](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_RunInstances.html) API documentation\.

------
#### [ YAML ]

```
name: launchInstance
action: aws:runInstances
maxAttempts: 3
timeoutSeconds: 1200
onFailure: Abort
inputs:
  ImageId: ami-12345678
  InstanceType: t2.micro
  MinInstanceCount: 1
  MaxInstanceCount: 1
  IamInstanceProfileName: myRunCmdRole
  TagSpecifications:
  - ResourceType: instance
    Tags:
    - Key: LaunchedBy
      Value: SSMAutomation
    - Key: Category
      Value: HighAvailabilityFleetHost
```

------
#### [ JSON ]

```
{
   "name":"launchInstance",
   "action":"aws:runInstances",
   "maxAttempts":3,
   "timeoutSeconds":1200,
   "onFailure":"Abort",
   "inputs":{
      "ImageId":"ami-12345678",
      "InstanceType":"t2.micro",
      "MinInstanceCount":1,
      "MaxInstanceCount":1,
      "IamInstanceProfileName":"myRunCmdRole",
      "TagSpecifications":[
         {
            "ResourceType":"instance",
            "Tags":[
               {
                  "Key":"LaunchedBy",
                  "Value":"SSMAutomation"
               },
               {
                  "Key":"Category",
                  "Value":"HighAvailabilityFleetHost"
               }
            ]
         }
      ]
   }
}
```

------

ImageId  
The ID of the Amazon Machine Image \(AMI\)\.  
Type: String  
Required: Yes

InstanceType  
The instance type\.  
If an instance type value is not provided, the m1\.small instance type is used\.
Type: String  
Required: No

MinInstanceCount  
The minimum number of instances to be launched\.  
Type: String  
Required: No

MaxInstanceCount  
The maximum number of instances to be launched\.  
Type: String  
Required: No

AdditionalInfo  
Reserved\.  
Type: String  
Required: No

BlockDeviceMappings  
The block devices for the instance\.  
Type: MapList  
Required: No

ClientToken  
The identifier to ensure idempotency of the request\.  
Type: String  
Required: No

DisableApiTermination  
Enables or disables instance API termination  
Type: Boolean  
Required: No

EbsOptimized  
Enables or disabled EBS optimization\.  
Type: Boolean  
Required: No

IamInstanceProfileArn  
The ARN of the IAM instance profile for the instance\.  
Type: String  
Required: No

IamInstanceProfileName  
The name of the IAM instance profile for the instance\.  
Type: String  
Required: No

InstanceInitiatedShutdownBehavior  
Indicates whether the instance stops or terminates on system shutdown\.  
Type: String  
Required: No

KernelId  
The ID of the kernel\.  
Type: String  
Required: No

KeyName  
The name of the key pair\.  
Type: String  
Required: No

MaxInstanceCount  
The maximum number of instances to filter when searching for offerings\.  
Type: Integer  
Required: No

MinInstanceCount  
The minimum number of instances to filter when searching for offerings\.  
Type: Integer  
Required: No

Monitoring  
Enables or disables detailed monitoring\.  
Type: Boolean  
Required: No

NetworkInterfaces  
The network interfaces\.  
Type: MapList  
Required: No

Placement  
The placement for the instance\.  
Type: StringMap  
Required: No

PrivateIpAddress  
The primary IPv4 address\.  
Type: String  
Required: No

RamdiskId  
The ID of the RAM disk\.  
Type: String  
Required: No

SecurityGroupIds  
The IDs of the security groups for the instance\.  
Type: StringList  
Required: No

SecurityGroups  
The names of the security groups for the instance\.  
Type: StringList  
Required: No

SubnetId  
The subnet ID\.  
Type: String  
Required: No

TagSpecifications  
The tags to apply to the resources during launch\. You can only tag instances and volumes at launch\. The specified tags are applied to all instances or volumes that are created during launch\. To tag an instance after it has been launched, use the [aws:createTags – Create tags for AWS resources](#automation-action-createtag) action\.  
Type: MapList \(For more information, see [TagSpecification](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_TagSpecification.html)\.\)  
Required: No

UserData  
An execution script provided as a string literal value\. If a literal value is entered, then it must be Base64\-encoded\.  
Type: String  
Required: NoOutput

InstanceIds  
The IDs of the instances\.

## aws:sleep – Delay an automation execution<a name="automation-action-sleep"></a>

Delays Automation execution for a specified amount of time\. This action uses the International Organization for Standardization \(ISO\) 8601 date and time format\. For more information about this date and time format, see [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html)\.

**Input**  
You can delay execution for a specified duration\. 

------
#### [ YAML ]

```
name: sleep
action: aws:sleep
inputs:
  Duration: PT10M
```

------
#### [ JSON ]

```
{
   "name":"sleep",
   "action":"aws:sleep",
   "inputs":{
      "Duration":"PT10M"
   }
}
```

------

You can also delay execution until a specified date and time\. If the specified date and time has passed, the action proceeds immediately\. 

------
#### [ YAML ]

```
name: sleep
action: aws:sleep
inputs:
  Timestamp: '2020-01-01T01:00:00Z'
```

------
#### [ JSON ]

```
{
    "name": "sleep",
    "action": "aws:sleep",
    "inputs": {
        "Timestamp": "2020-01-01T01:00:00Z"
    }
}
```

------

**Note**  
Automation currently supports a maximum delay of 604800 seconds \(7 days\)\.

Duration  
An ISO 8601 duration\. You can't specify a negative duration\.   
Type: String  
Required: No

Timestamp  
An ISO 8601 timestamp\. If you don't specify a value for this parameter, then you must specify a value for the `Duration` parameter\.   
Type: String  
Required: NoOutput

None  

## aws:waitForAwsResourceProperty – Wait on an AWS resource property<a name="automation-action-waitForAwsResourceProperty"></a>

The aws:waitForAwsResourceProperty action enables your Automation workflow to wait for a specific resource state or event state before continuing the workflow\. For more information and examples of how to use this action, see [Invoking Other AWS Services from a Systems Manager Automation Workflow](automation-aws-apis-calling.md)\.

**Input**  
Inputs are defined by the API action that you choose\.

------
#### [ YAML ]

```
action: aws:waitForAwsResourceProperty
inputs:
  Service: The official namespace of the service
  Api: The API action or method name
  API action inputs or parameters: A value
  PropertySelector: Response object
  DesiredValues:
  - Desired property value
```

------
#### [ JSON ]

```
{
  "action": "aws:waitForAwsResourceProperty",
  "inputs": {
    "Service":"The official namespace of the service",
    "Api":"The API action or method name",
    "API action inputs or parameters":"A value",
    "PropertySelector": "Response object",
    "DesiredValues": [
      "Desired property value"
    ]
  }
}
```

------

Service  
The AWS service namespace that contains the API action that you want to run\. For example, the namespace for Systems Manager is `ssm`\. The namespace for Amazon EC2 is `ec2`\. You can view a list of supported AWS service namespaces in the [Available Services](https://docs.aws.amazon.com/cli/latest/reference/#available-services) section of the *AWS CLI Command Reference*\.  
Type: String  
Required: Yes

Api  
The name of the API action that you want to run\. You can view the API actions \(also called methods\) by choosing a service in the left navigation on the following [Services Reference](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all API actions \(methods\) for Amazon RDS are listed on the following page: [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\.  
Type: String  
Required: Yes

API action inputs  
One or more API action inputs\. You can view the available inputs \(also called parameters\) by choosing a service in the left navigation on the following [Services Reference](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all methods for Amazon RDS are listed on the following page: [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\. Choose the [describe\_db\_instances](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) method and scroll down to see the available parameters, such as **DBInstanceIdentifier**, **Name**, and **Values**\.  

```
inputs:
  Service: The official namespace of the service
  Api: The API action name
  API input 1: A value
  API Input 2: A value
  API Input 3: A value
```

```
"inputs":{
      "Service":"The official namespace of the service",
      "Api":"The API action name",
      "API input 1":"A value",
      "API Input 2":"A value",
      "API Input 3":"A value"
}
```
Type: Determined by chosen API action  
Required: Yes

PropertySelector  
The JSONPath to a specific attribute in the response object\. You can view the response objects by choosing a service in the left navigation on the following [Services Reference](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/index.html) page\. Choose a method in the **Client** section for the service that you want to invoke\. For example, all methods for Amazon RDS are listed on the following page: [Amazon RDS methods](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html)\. Choose the [describe\_db\_instances](http://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/rds.html#RDS.Client.describe_db_instances) method and scroll down to the **Response Structure** section\. **DBInstances** is listed as a response object\.  
Type: Integer, Boolean, String, StringList, StringMap, or MapList  
Required: Yes

DesiredValues  
The expected status or state on which to continue the Automation workflow\.  
Type: Varies  
Required: Yes