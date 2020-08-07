# Creating dynamic Automation workflows with conditional branching<a name="automation-branchdocs"></a>

By default, the steps that you define in the `mainSteps` section of an Automation document run in sequential order\. After one action is completed, the next action specified in the `mainSteps` section begins\. Furthermore, if an action fails to run, the entire Automation workflow fails \(by default\)\. You can use the `aws:branch` Automation action and the Automation document options described in this section to create Automation workflows that perform *conditional branching*\. This means that you can create Automation workflows that jump to a different step after evaluating different choices or that dynamically respond to changes when a step completes\. Here is a list of options that you can use to create dynamic Automation workflows:
+ **aws:branch**: This automation action enables you to create a dynamic Automation workflow that evaluates multiple choices in a single step and then jumps to a different step in the Automation document based on the results of that evaluation\.
+ **nextStep**: This option specifies which step in an Automation workflow to process next after successfully completing a step\. 
+ **isEnd**: This option stops an Automation execution at the end of a specific step\. The default value for this option is false\.
+ **isCritical**: This option designates a step as critical for the successful completion of the Automation\. If a step with this designation fails, then Automation reports the final status of the Automation as Failed\. The default value for this option is true\.
+ **onFailure**: This option indicates whether the workflow should abort, continue, or go to a different step on failure\. The default value for this option is Abort\.

The following section describes the `aws:branch` Automation action\. For more information about the `nextStep`, `isEnd`, `isCritical`, and `onFailure` workflow options, see [Examples of how to use dynamic workflow options](#automation-branchdocs-examples)\.

## Working with the aws:branch action<a name="automation-branchdocs-awsbranch"></a>

The `aws:branch` action offers the most dynamic conditional branching options for Automation workflows\. As noted earlier, this action enables your Automation workflow to evaluate multiple conditions in a single step and then jump to a new step based on the results of that evaluation\. The `aws:branch` action functions like an `IF-ELIF-ELSE` statement in programming\.

Here is a YAML example of an `aws:branch` step:

```
- name: ChooseOSforCommands
  action: aws:branch
  inputs:
    Choices:
    - NextStep: runPowerShellCommand
      Variable: "{{GetInstance.platform}}"
      StringEquals: Windows
    - NextStep: runShellCommand
      Variable: "{{GetInstance.platform}}"
      StringEquals: Linux
    Default:
      PostProcessing
```

When you specify the `aws:branch` action for a step, you specify `Choices` that the workflow must evaluate\. The workflow can evaluate `Choices` based on the value of a parameter that you specified in the `Parameters` section of the Automation document\. The workflow can also evaluate `Choices` based on output from a previous step\.

The Automation workflow evaluates each choice by using a Boolean expression\. If the evaluation determines that the first choice is true, then the workflow jumps to the step designated for that choice\. If the evaluation determines that the first choice is false, then the workflow evaluates the next choice\. If your step includes three or more `Choices`, then the workflow evaluates each choice in sequential order until it evaluates a choice that is true\. The workflow then jumps to the designated step for the true choice\.

If none of the `Choices` are true, the workflow checks to see if the step contains a `Default` value\. A `Default` value defines a step that the workflow should jump to if none of the choices are true\. If no `Default` value is specified for the step, then the Automation workflow processes the next step in the document\.

Here is an `aws:branch` step in YAML named **chooseOSfromParameter**\. The step includes two `Choices`: \(`NextStep: runWindowsCommand`\) and \(`NextStep: runLinuxCommand`\)\. The Automation workflow evaluates these `Choices` to determine which command to run for the appropriate operating system\. The `Variable` for each choice uses `{{OSName}}`, which is a parameter that the document author defined in the `Parameters` section of the document\.

```
mainSteps:
- name: chooseOSfromParameter
  action: aws:branch
  inputs:
    Choices:
    - NextStep: runWindowsCommand
      Variable: "{{OSName}}"
      StringEquals: Windows
    - NextStep: runLinuxCommand
      Variable: "{{OSName}}"
      StringEquals: Linux
```

Here is an `aws:branch` step in YAML named **chooseOSfromOutput**\. The step includes two `Choices`: \(`NextStep: runPowerShellCommand`\) and \(`NextStep: runShellCommand`\)\. The Automation workflow evaluates these `Choices` to determine which command to run for the appropriate operating system\. The `Variable` for each choice uses `{{GetInstance.platform}}`, which is the output from an earlier step in the document\. This example also includes an option called `Default`\. If the workflow evaluates both `Choices`, and neither choice is true, then the Automation workflow jumps to a step called `PostProcessing`\.

```
mainSteps:
- name: chooseOSfromOutput
  action: aws:branch
  inputs:
    Choices:
    - NextStep: runPowerShellCommand
      Variable: "{{GetInstance.platform}}"
      StringEquals: Windows
    - NextStep: runShellCommand
      Variable: "{{GetInstance.platform}}"
      StringEquals: Linux
    Default:
      PostProcessing
```

### Creating an `aws:branch` step in an Automation document<a name="automation-branchdocs-awsbranch-creating"></a>

When you create an `aws:branch` step in an Automation document, you define the `Choices` the workflow should evaluate to determine which step the workflow should jump to next\. As noted earlier, `Choices` are evaluated by using a Boolean expression\. Each choice must define the following options:
+ **NextStep**: The next step in the Automation document to process if the designated choice is true\.
+ **Variable**: Specify either the name of a parameter that is defined in the `Parameters` section of the Automation document, or specify an output object from a previous step in the Automation document\.

  Specify parameter variables by using the following form:

  `Variable: "{{name_of_parameter}}"`

  Specify output object variables by using the following form:

  `Variable: "{{previousStepName.outputFieldName}}"`
**Note**  
Creating the output variable is described in more detail in the next section, [About creating the output variable](#automation-branchdocs-awsbranch-creating-output)\.
+ **Operation**: The criteria used to evaluate the choice, such as `StringEquals: Linux`\. The `aws:branch` action supports the following operations:

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
+ **Default**: Specify a fallback step that the workflow should jump to if none of the `Choices` are true\. 
**Note**  
If you don't want to specify a `Default` value, then you can specify the `isEnd` workflow option\. If none of the `Choices` are true and no `Default` value is specified, then the Automation workflow stops at the end of the step\.

Use the following templates to help you construct the `aws:branch` step in your Automation documents:

------
#### [ YAML ]

```
mainSteps:
- name: a name for the step
  action: aws:branch
  inputs:
    Choices:
    - NextStep: step to jump to if evaluation for this choice is true
      Variable: "{{parameter name or output from previous step}}"
      Operation type: Operation value
    - NextStep: step to jump to if evaluation = true
      Variable: "{{parameter name or output from previous step}}"
      Operation type: Operation value
    Default:
      step to jump to if all choices are false
```

------
#### [ JSON ]

```
{
   "mainSteps":[
      {
         "name":"a name for the step",
         "action":"aws:branch",
         "inputs":{
            "Choices":[
               {
                  "NextStep":"step to jump to if evaluation for this choice is true",
                  "Variable":"{{parameter name or output from previous step}}",
                  "Operation type":"Operation value"
               },
               {
                  "NextStep":"step to jump to if evaluation = true",
                  "Variable":"{{parameter name or output from previous step}}",
                  "Operation type":"Operation value"
               }
            ],
            "Default":"step to jump to if all choices are false"
         }
      }
   ]
}
```

------

#### About creating the output variable<a name="automation-branchdocs-awsbranch-creating-output"></a>

To create an `aws:branch` choice that references the output from a previous step, you need to identify the name of the previous step and the name of the output field\. You then combine the names of the step and the field by using the following format:

`Variable: "{{previousStepName.outputFieldName}}"`

For example, the first step below is named `GetInstance`\. And then, under `outputs`, there is a field called `platform`\. In the second step \(`ChooseOSforCommands`\), the author wants to reference the output from the platform field as a variable\. To create the variable, simply combine the step name \(GetInstance\) and the output field name \(platform\) to create `Variable: "{{GetInstance.platform}}"`\.

```
mainSteps:
- Name: GetInstance
  action: aws:executeAwsApi
  inputs:
    Service: ssm
    Api: DescribeInstanceInformation
    Filters:
    - Key: InstanceIds
      Values: ["{{ InstanceId }}"]
  outputs:
  - Name: myInstance
    Selector: "$.InstanceInformationList[0].InstanceId"
    Type: String
  - Name: platform
    Selector: "$.InstanceInformationList[0].PlatformType"
    Type: String
- name: ChooseOSforCommands
  action: aws:branch
  inputs:
    Choices:
    - NextStep: runPowerShellCommand
      Variable: "{{GetInstance.platform}}"
      StringEquals: Windows
    - NextStep: runShellCommand
      Variable: "{{GetInstance.platform}}"
      StringEquals: Linux
    Default:
      Sleep
```

Here is a JSON example that shows how * "Variable": "\{\{ describeInstance\.Platform \}\}"* is created from the previous step \(*"describeInstance"*\) and the output field \(*"Platform"*\)\.

```
{
      "name": "describeInstance",
      "action": "aws:executeAwsApi",
      "onFailure": "Abort",
      "inputs": {
        "Service": "ec2",
        "Api": "DescribeInstances",
        "InstanceIds": [
          "{{ InstanceId }}"
        ]
      },
      "outputs": [
        {
          "Name": "Platform",
          "Selector": "$.Reservations[0].Instances[0].Platform",
          "Type": "String"
        }
      ],
      "nextStep": "branchOnInstancePlatform"
    },
    {
      "name": "branchOnInstancePlatform",
      "action": "aws:branch",
      "inputs": {
        "Choices": [
          {
            "NextStep": "runEC2RescueForWindows",
            "Variable": "{{ describeInstance.Platform }}",
            "StringEquals": "windows"
          }
        ],
        "Default": "runEC2RescueForLinux"
      }
    }
```

### Example aws:branch Automation documents<a name="automation-branchdocs-awsbranch-examples-docs"></a>

Here are some example Automation documents that use `aws:branch`\.

**Example 1: Using aws:branch with an output variable to run commands based on the operating system type**

In the first step of this sample \(`GetInstance`\), the document author uses the `aws:executeAwsApi` action to call the `ssm` `DescribeInstanceInformation` API action\. The author uses this action to determine the type of operating system being used by an instance\. The `aws:executeAwsApi` action outputs the instance ID and the platform type\.

In the second step \(`ChooseOSforCommands`\), the author uses the `aws:branch` action with two `Choices` \(`NextStep: runPowerShellCommand`\) and \(`NextStep: runShellCommand`\)\. The Automation workflow evaluates the operating system of the instance by using the output from the previous step \(`Variable: "{{GetInstance.platform}}"`\)\. The Automation workflow jumps to a step for the designated operating system\.

```
---
schemaVersion: '0.3'
assumeRole: "{{AutomationAssumeRole}}"
parameters:
  AutomationAssumeRole:
    default: ""
    type: String
mainSteps:
- name: GetInstance
  action: aws:executeAwsApi
  inputs:
    Service: ssm
    Api: DescribeInstanceInformation
  outputs:
  - Name: myInstance
    Selector: "$.InstanceInformationList[0].InstanceId"
    Type: String
  - Name: platform
    Selector: "$.InstanceInformationList[0].PlatformType"
    Type: String
- name: ChooseOSforCommands
  action: aws:branch
  inputs:
    Choices:
    - NextStep: runPowerShellCommand
      Variable: "{{GetInstance.platform}}"
      StringEquals: Windows
    - NextStep: runShellCommand
      Variable: "{{GetInstance.platform}}"
      StringEquals: Linux
    Default:
      Sleep
- name: runShellCommand
  action: aws:runCommand
  inputs:
    DocumentName: AWS-RunShellScript
    InstanceIds:
    - "{{GetInstance.myInstance}}"
    Parameters:
      commands:
      - ls
  isEnd: true
- name: runPowerShellCommand
  action: aws:runCommand
  inputs:
    DocumentName: AWS-RunPowerShellScript
    InstanceIds:
    - "{{GetInstance.myInstance}}"
    Parameters:
      commands:
      - ls
  isEnd: true
- name: Sleep
  action: aws:sleep
  inputs:
    Duration: PT3S
```

**Example 2: Using aws:branch with a parameter variable to run commands based on the operating system type**

The document author defines several parameter options at the beginning of the document in the `parameters` section\. One parameter is named `OperatingSystemName`\. In the first step \(`ChooseOS`\), the author uses the `aws:branch` action with two `Choices` \(`NextStep: runWindowsCommand`\) and \(`NextStep: runLinuxCommand`\)\. The variable for these `Choices` references the parameter option specified in the parameters section \(`Variable: "{{OperatingSystemName}}"`\)\. When the user runs this Automation workflow, they specify a value at runtime for `OperatingSystemName`\. The Automation workflow uses the runtime parameter during the `Choices` evaluation\. The Automation workflow jumps to a step for the designated operating system based on the runtime parameter specified for `OperatingSystemName`\.

```
---
schemaVersion: '0.3'
assumeRole: "{{AutomationAssumeRole}}"
parameters:
  AutomationAssumeRole:
    default: ""
    type: String
  OperatingSystemName:
    type: String
  LinuxInstanceId:
    type: String
  WindowsInstanceId:
    type: String
mainSteps:
- name: ChooseOS
  action: aws:branch
  inputs:
    Choices:
    - NextStep: runWindowsCommand
      Variable: "{{OperatingSystemName}}"
      StringEquals: windows
    - NextStep: runLinuxCommand
      Variable: "{{OperatingSystemName}}"
      StringEquals: linux
    Default:
      Sleep
- name: runLinuxCommand
  action: aws:runCommand
  inputs:
    DocumentName: "AWS-RunShellScript"
    InstanceIds:
    - "{{LinuxInstanceId}}"
    Parameters:
      commands:
      - ls
  isEnd: true
- name: runWindowsCommand
  action: aws:runCommand
  inputs:
    DocumentName: "AWS-RunPowerShellScript"
    InstanceIds:
    - "{{WindowsInstanceId}}"
    Parameters:
      commands:
      - date
  isEnd: true
- name: Sleep
  action: aws:sleep
  inputs:
    Duration: PT3S
```

### Creating complex branching documents with operators<a name="automation-branchdocs-awsbranch-operators"></a>

You can create complex branching documents by using the `And`, `Or`, and `Not` operators in your `aws:branch` steps\.

**The 'And' Operator**  
Use the `And` operator when you want multiple variables to be true for a choice\. In the following example, the first choice evaluates if an instance is `running` and uses the `Windows` operating system\. If the evaluation of *both* of these variables is true, then the Automation workflow jumps to the `runPowerShellCommand` step\. If one or more of the variables is false, then the workflow evaluates the variables for the second choice\.

```
mainSteps:
- name: switch2
  action: aws:branch
  inputs:
    Choices:
    - And:
      - Variable: "{{GetInstance.pingStatus}}"
        StringEquals: running
      - Variable: "{{GetInstance.platform}}"
        StringEquals: Windows
      NextStep: runPowerShellCommand

    - And:
      - Variable: "{{GetInstance.pingStatus}}"
        StringEquals: running
      - Variable: "{{GetInstance.platform}}"
        StringEquals: Linux
      NextStep: runShellCommand
    Default:
      sleep3
```

**The 'Or' Operator**  
Use the `Or` operator when you want *any* of multiple variables to be true for a choice\. In the following example, the first choice evaluates if a parameter string is `Windows` and if the output from an AWS Lambda step is true\. If the evaluation determines that *either* of these variables is true, then the Automation workflow jumps to the `RunPowerShellCommand` step\. If both variables are false, then the workflow evaluates the variables for the second choice\.

```
- Or:
  - Variable: "{{parameter1}}"
    StringEquals: Windows
  - Variable: "{{BooleanParam1}}"
    BooleanEquals: true
  NextStep: RunPowershellCommand
- Or:
  - Variable: "{{parameter2}}"
    StringEquals: Linux
  - Variable: "{{BooleanParam2}}"
    BooleanEquals: true
  NextStep: RunShellScript
```

**The 'Not' Operator**  
Use the `Not` operator when you want to jump to a step defined when a variable is *not* true\. In the following example, the first choice evaluates if a parameter string is `Not Linux`\. If the evaluation determines that the variable is not Linux, then the Automation workflow jumps to the `sleep2` step\. If the evaluation of the first choice determines that it *is* Linux, then the workflow evaluates the next choice\.

```
mainSteps:
- name: switch
  action: aws:branch
  inputs:
    Choices:
    - NextStep: sleep2
      Not:
        Variable: "{{testParam}}"
        StringEquals: Linux
    - NextStep: sleep1
      Variable: "{{testParam}}"
      StringEquals: Windows
    Default:
      sleep3
```

## Examples of how to use dynamic workflow options<a name="automation-branchdocs-examples"></a>

This section includes different examples of how to use dynamic workflow options in an Automation document\. Each example in this section extends the following Automation document\. This document has two actions\. The first action is named `InstallMsiPackage`\. It uses the `aws:runCommand` action to install an application on a Windows Server instance\. The second action is named `TestInstall`\. It uses the `aws:invokeLambdaFunction` action to perform a test of the installed application if the application installed successfully\. Step one specifies `onFailure: Abort`\. This means that if the application did not install successfully, the Automation workflow execution stops before step two\.

**Example 1: Automation document with two linear actions**

```
---
schemaVersion: '0.3'
description: Install MSI package and run validation.
assumeRole: "{{automationAssumeRole}}"
parameters:
  automationAssumeRole:
    type: String
    description: "(Required) Assume role."
  packageName:
    type: String
    description: "(Required) MSI package to be installed."
  instanceIds:
    type: String
    description: "(Required) Comma separated list of instances."
mainSteps:
- name: InstallMsiPackage
  action: aws:runCommand
  maxAttempts: 2
  onFailure: Abort
  inputs:
    InstanceIds:
    - i-02573cafcfEXAMPLE
    - i-0471e04240EXAMPLE
    - i-07782c72faEXAMPLE
    DocumentName: AWS-RunPowerShellScript
    Parameters:
      commands:
      - msiexec /i {{packageName}}
- name: TestInstall
  action: aws:invokeLambdaFunction
  maxAttempts: 1
  timeoutSeconds: 500
  inputs:
    FunctionName: TestLambdaFunction
...
```

**Creating a Dynamic Workflow that Jumps to Different Steps by Using the onFailure Option**

The following example uses the `onFailure: step:step_name`, `nextStep`, and `isEnd` options to create a dynamic Automation workflow\. With this example, if the `InstallMsiPackage` action fails, then the workflow jumps to an action called *PostFailure* \(`onFailure: step:PostFailure`\) to run an AWS Lambda function to perform some action in the event the install failed\. If the install succeeds, then the workflow process jumps to the TestInstall action \(`nextStep: TestInstall`\)\. Both the `TestInstall` and the `PostFailure` steps use the `isEnd` option \(`isEnd: true`\) so that the workflow finishes the workflow execution when either of those steps is completed\.

**Note**  
Using the `isEnd` option in the last step of the `mainSteps` section is optional\. If the last step does not jump to other steps, then the Automation workflow stops after running the action in the last step\.

**Example 2: A dynamic workflow that jumps to different steps**

```
mainSteps
- name: InstallMsiPackage
  action: aws:runCommand
  onFailure: step:PostFailure
  maxAttempts: 2
  inputs:
    InstanceIds:
    - i-02573cafcfEXAMPLE
    - i-0471e04240EXAMPLE
    DocumentName: AWS-RunPowerShellScript
    Parameters:
      commands:
      - msiexec /i {{packageName}}
  nextStep: TestInstall
- name: TestInstall
  action: aws:invokeLambdaFunction
  maxAttempts: 1
  timeoutSeconds: 500
  inputs:
    FunctionName: TestLambdaFunction
  isEnd: true
- name: PostFailure
  action: aws:invokeLambdaFunction
  maxAttempts: 1
  timeoutSeconds: 500
  inputs:
    FunctionName: PostFailureRecoveryLambdaFunction
  isEnd: true
...
```

**Note**  
Before processing an Automation document, the system verifies that the document does not create an infinite loop\. If an infinite loop is detected, Automation returns an error and a circle trace showing which steps create the loop\.

**Creating a Dynamic Workflow that Defines Critical Steps**

You can specify that a step is critical for the overall success of the Automation workflow\. If a critical step fails, then Automation reports the status of the execution as failed, even if one or more steps ran successfully\. In the following example, the user identifies the *VerifyDependencies* step if the *InstallMsiPackage* step fails \(`onFailure: step:VerifyDependencies`\)\. The user specifies that the `InstallMsiPackage` step is not critical \(`isCritical: false`\)\. In this example, if the application failed to install, Automation processes the `VerifyDependencies` step to determine if one or more dependencies is missing, which therefore caused the application install to fail\. 

**Example 3: Defining critical steps for the Automation workflow**

```
---
name: InstallMsiPackage
action: aws:runCommand
onFailure: step:VerifyDependencies
isCritical: false
maxAttempts: 2
inputs:
  InstanceIds:
  - "{{instanceIds}}"
  DocumentName: AWS-RunPowerShellScript
  Parameters:
    commands:
    - msiexec /i {{packageName}}
nextStep: TestPackage
...
```