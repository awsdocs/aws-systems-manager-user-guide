# Creating Dynamic Automation Workflows<a name="automation-branchdocs"></a>

By default, the steps \(or actions\) that you define in the mainSteps section of an Automation document execute in sequential order\. After one action is completed, the next action specified in the mainSteps section begins\. Furthermore, if an action fails to execute, the entire Automation workflow fails \(by default\)\. You can use the options described in this section to create Automation workflows that perform *conditional branching*\. This means that you can create Automation workflows that dynamically respond to changes in conditions when a step completes\. Here is a list of options that you can use to create dynamic Automation workflows\.
+ **nextStep**: Specifies which step in an Automation workflow to process next after successfully completing a step\. 
+ **isEnd**: Stops an Automation execution at the end of a specific step\. The Automation execution stops if the step execution failed or succeeded\. The default value for this option is false\.
+ **isCritical**: Designates a step as critical for the successful completion of the Automation\. If a step with this designation fails, then Automation reports the final status of the Automation as Failed\. The default value for this option is true\.
+ **onFailure**: Indicates whether the workflow should abort, continue, or go to a different step on failure\. The default value for this option is abort\.

## Examples of How to Use Dynamic Workflow Options<a name="automation-branchdocs-examples"></a>

This section includes different examples of how to use dynamic workflow options in an Automation document\. Each example in this section extends the following Automation document\. This document has two actions\. The first action is named *InstallMsiPackage*\. It uses the aws:runCommand action to install an application on a Windows Server instance\. The second action is named *TestInstall*\. It uses the aws:invokeLambdaFunction action to perform a test of the installed application if the application installed successfully\. Step one specifies `"onFailure":"Abort"`\. This means that if the application did not install successfully, the Automation workflow execution stops before step two\.

**Example 1: Automation document with two linear actions**

```
{
   "schemaVersion":"0.3",
   "description":"Install MSI package and run validation.",
   "assumeRole":"{{automationAssumeRole}}",
   "parameters":{
      "automationAssumeRole":{
         "type":"String",
         "description":"(Required) Assume role."
      },
      "packageName":{
         "type":"String",
         "description":"(Required) MSI package to be installed."
      },
      "instanceIds":{
         "type":"String",
         "description":"(Required) Comma separated list of instances."
      }
   }   "mainSteps":[
      {
         "name":"InstallMsiPackage",
         "action":"aws:runCommand",
         "maxAttempts":2,
         "onFailure":"Abort",
         "inputs":{
            "InstanceIds":[
               {
                  {
                     instanceIds
                  }
               }
            ],
            "DocumentName":"AWS-RunPowerShellScript",
            "Parameters":{
               "commands":[
                  "msiexec /i {{packageName}}"
               ]
            }
         }
      },
      {
         "name":"TestInstall",
         "action":"aws:invokeLambdaFunction",
         "maxAttempts":1,
         "timeoutSeconds":500,
         "inputs":{
            "FunctionName":"TestLambdaFunction"
         }
      }
   ]
}
```

**Creating a Dynamic Workflow that Jumps to Different Steps**

The following example uses the "onFailure":"step:*step\_name*", nextStep, and isEnd options to create a dynamic Automation workflow\. With this example, if the InstallMsiPackage action fails, then the workflow jumps to an action called PostFailure \("onFailure":"step:PostFailure"\) to run an AWS Lambda function to perform some action in the event the install failed\. If the install succeeds, then the workflow process jumps to the TestInstall action \("nextStep":"TestInstall"\)\. Both the TestInstall and the PostFailure steps use the isEnd option \("isEnd":"true"\) so that the workflow finishes the workflow execution when either of those steps is completed\.

**Note**  
Using the isEnd option in the last step of the mainSteps section is optional\. If the last step does not jump to other steps, then the Automation workflow stops after executing the action in the last step\.

**Example 2: A dynamic workflow that jumps to different steps**

```
"mainSteps":[
   {
      "name":"InstallMsiPackage",
      "action":"aws:runCommand",
      "onFailure":"step:PostFailure",
      
      "maxAttempts":2,
      "inputs":{
         "InstanceIds":[
            {
               {
                  "i-1234567890abcdef0,i-0598c7d356eba48d7"
               }
            }
         ],
         "DocumentName":"AWS-RunPowerShellScript",
         "Parameters":{
            "commands":[
               "msiexec /i {{packageName}}"
            ]
         }
      },
      "nextStep":"TestInstall"      
   },
   {
      "name":"TestInstall",
      "action":"aws:invokeLambdaFunction",
      "maxAttempts":1,
      "timeoutSeconds":500,
      "inputs":{
         "FunctionName":"TestLambdaFunction"
      },
      "isEnd":"true"
   },   {
      "name":"PostFailure",
      "action":"aws:invokeLambdaFunction",
      "maxAttempts":1,
      "timeoutSeconds":500,
      "inputs":{
         "FunctionName":"PostFailureRecoveryLambdaFunction"
      },
     "isEnd":"true"
   }
```

**Note**  
Before processing an Automation document, the system verifies that the document does not create an infinite loop\. If an infinite loop is detected, Automation returns an error and a circle trace showing which steps create the loop\.

**Creating a Dynamic Workflow that Defines Critical Steps**

You can specify that a step is critical for the overall success of the Automation workflow\. If a critical step fails, then Automation reports the status of the execution as failed, even if one or more steps executed successfully\. In the following example, the user identifies the VerifyDependencies step if the InstallMsiPackage step fails \("onFailure":"step:VerifyDependencies"\)\. The user specifies that the InstallMsiPackage step is not critical \("isCritical":false\)\. In this example, if the application failed to install, Automation processes the VerifyDependencies step to determine if one or more dependencies is missing, which therefore caused the application install to fail\. 

**Example 3: Defining critical steps for the Automation workflow**

```
{
   "name":"InstallMsiPackage",
   "action":"aws:runCommand",
   "onFailure":"step:VerifyDependencies",
   "isCritical":false,
   "maxAttempts":2,
   "inputs":{
      "InstanceIds":[
         {
            {
               instanceIds
            }
         }
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
```