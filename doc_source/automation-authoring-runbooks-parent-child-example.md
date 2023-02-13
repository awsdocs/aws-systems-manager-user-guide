# Example 1: Creating parent\-child runbooks<a name="automation-authoring-runbooks-parent-child-example"></a>

The following example demonstrates how to create two runbooks that patch tagged groups of Amazon Elastic Compute Cloud \(Amazon EC2\) instances in stages\. These runbooks are used in a parent\-child relationship with the parent runbook used to initiate a rate control automation of the child runbook\. For more information about rate control automations, see [Run automations at scale](running-automations-scale.md)\. For more information about the automation actions used in this example, see the [Systems Manager Automation actions reference](automation-actions.md)\.

## Create the child runbook<a name="automation-authoring-runbooks-child-runbook"></a>

This example runbook addresses the following scenario\. Emily is a Systems Engineer at AnyCompany Consultants, LLC\. She needs to configure patching for groups of Amazon Elastic Compute Cloud \(Amazon EC2\) instances that host primary and secondary databases\. Applications access these databases 24 hours a day, so one of the database instances must always be available\. 

She decides that patching the instances in stages is the best approach\. The primary group of database instances will be patched first, followed by the secondary group of database instances\. Also, to avoid incurring additional costs by leaving instances running that were previously stopped, Emily wants the patched instances to be returned to their original state before the patching occurred\. 

Emily identifies the primary and secondary groups of database instances by the tags associated with the instances\. She decides to create a parent runbook that starts a rate control automation of a child runbook\. By doing that, she can target the tags associated with the primary and secondary groups of database instances and manage the concurrency of the child automations\. After reviewing the available Systems Manager \(SSM\) documents for patching, she chooses the `AWS-RunPatchBaseline` document\. By using this SSM document, her colleagues can review the associated patch compliance information after the patching operation completes\.

To start creating her runbook content, Emily reviews the available automation actions and begins authoring the content for the child runbook as follows:

1. First, she provides values for the schema and description of the runbook, and defines the input parameters for the child runbook\.

   By using the `AutomationAssumeRole` parameter, Emily and her colleagues can use an existing IAM role that allows Automation to perform the actions in the runbook on their behalf\. Emily uses the `InstanceId` parameter to determine the instance that should be patched\. Optionally, the `Operation`, `RebootOption`, and `SnapshotId` parameters can be used to provide values to document parameters for `AWS-RunPatchBaseline`\. To prevent invalid values from being provided to those document parameters, she defines the `allowedValues` as needed\.

------
#### [ YAML ]

   ```
   schemaVersion: '0.3'
   description: 'An example of an Automation runbook that patches groups of Amazon EC2 instances in stages.'
   assumeRole: '{{AutomationAssumeRole}}'
   parameters:
     AutomationAssumeRole:
       type: String
       description: >-
         '(Optional) The Amazon Resource Name (ARN) of the IAM role that allows Automation to perform the
         actions on your behalf. If no role is specified, Systems Manager
         Automation uses your IAM permissions to operate this runbook.'
       default: ''
     InstanceId:
       type: String
       description: >-
         '(Required) The instance you want to patch.'
     SnapshotId:
       type: String
       description: '(Optional) The snapshot ID to use to retrieve a patch baseline snapshot.'
       default: ''
     RebootOption:
       type: String
       description: '(Optional) Reboot behavior after a patch Install operation. If you choose NoReboot and patches are installed, the instance is marked as non-compliant until a subsequent reboot and scan.'
       allowedValues:
         - NoReboot
         - RebootIfNeeded
       default: RebootIfNeeded
     Operation:
       type: String
       description: '(Optional) The update or configuration to perform on the instance. The system checks if patches specified in the patch baseline are installed on the instance. The install operation installs patches missing from the baseline.'
       allowedValues:
         - Install
         - Scan
       default: Install
   ```

------
#### [ JSON ]

   ```
   {
      "schemaVersion":"0.3",
      "description":"An example of an Automation runbook that patches groups of Amazon EC2 instances in stages.",
      "assumeRole":"{{AutomationAssumeRole}}",
      "parameters":{
         "AutomationAssumeRole":{
            "type":"String",
            "description":"(Optional) The Amazon Resource Name (ARN) of the IAM role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses your IAM permissions to operate this runbook.",
            "default":""
         },
         "InstanceId":{
            "type":"String",
            "description":"(Required) The instance you want to patch."
         },
         "SnapshotId":{
            "type":"String",
            "description":"(Optional) The snapshot ID to use to retrieve a patch baseline snapshot.",
            "default":""
         },
         "RebootOption":{
            "type":"String",
            "description":"(Optional) Reboot behavior after a patch Install operation. If you choose NoReboot and patches are installed, the instance is marked as non-compliant until a subsequent reboot and scan.",
            "allowedValues":[
               "NoReboot",
               "RebootIfNeeded"
            ],
            "default":"RebootIfNeeded"
         },
         "Operation":{
            "type":"String",
            "description":"(Optional) The update or configuration to perform on the instance. The system checks if patches specified in the patch baseline are installed on the instance. The install operation installs patches missing from the baseline.",
            "allowedValues":[
               "Install",
               "Scan"
            ],
            "default":"Install"
         }
      }
   },
   ```

------

1. With the top\-level elements defined, Emily proceeds with authoring the actions that make up the `mainSteps` of the runbook\. The first step outputs the current state of the target instance specified in the `InstanceId` input parameter using the `aws:executeAwsApi` action\. The output of this action is used in later actions\.

------
#### [ YAML ]

   ```
   mainSteps:
     - name: getInstanceState
       action: 'aws:executeAwsApi'
       onFailure: Abort
       inputs:
         Service: ec2
         Api: DescribeInstances
         InstanceIds:
           - '{{InstanceId}}'
       outputs:
         - Name: instanceState
           Selector: '$.Reservations[0].Instances[0].State.Name'
           Type: String
       nextStep: branchOnInstanceState
   ```

------
#### [ JSON ]

   ```
   "mainSteps":[
         {
            "name":"getInstanceState",
            "action":"aws:executeAwsApi",
            "onFailure":"Abort",
            "inputs":{
               "inputs":null,
               "Service":"ec2",
               "Api":"DescribeInstances",
               "InstanceIds":[
                  "{{InstanceId}}"
               ]
            },
            "outputs":[
               {
                  "Name":"instanceState",
                  "Selector":"$.Reservations[0].Instances[0].State.Name",
                  "Type":"String"
               }
            ],
            "nextStep":"branchOnInstanceState"
         },
   ```

------

1. Rather than manually starting and keeping track of the original state of every instance that needs to be patched, Emily uses the output from the previous action to branch the automation based on the state of the target instance\. This allows the automation to run different steps depending on the conditions defined in the `aws:branch` action and improves the overall efficiency of the automation without manual intervention\.

   If the instance state is already `running`, the automation proceeds with patching the instance with the `AWS-RunPatchBaseline` document using the `aws:runCommand` action\.

   If the instance state is `stopping`, the automation polls for the instance to reach the `stopped` state using the `aws:waitForAwsResourceProperty` action, starts the instance using the `executeAwsApi` action, and polls for the instance to reach a `running` state before patching the instance\.

   If the instance state is `stopped`, the automation starts the instance and polls for the instance to reach a `running` state before patching the instance using the same actions\.

------
#### [ YAML ]

   ```
   - name: branchOnInstanceState
       action: 'aws:branch'
       onFailure: Abort
       inputs:
         Choices:
           - NextStep: startInstance
              Variable: '{{getInstanceState.instanceState}}'
              StringEquals: stopped
            - NextStep: verifyInstanceStopped
              Variable: '{{getInstanceState.instanceState}}'
              StringEquals: stopping
            - NextStep: patchInstance
              Variable: '{{getInstanceState.instanceState}}'
              StringEquals: running
       isEnd: true
     - name: startInstance
       action: 'aws:executeAwsApi'
       onFailure: Abort
       inputs:
         Service: ec2
         Api: StartInstances
         InstanceIds:
           - '{{InstanceId}}'
       nextStep: verifyInstanceRunning
     - name: verifyInstanceRunning
       action: 'aws:waitForAwsResourceProperty'
       timeoutSeconds: 120
       inputs:
         Service: ec2
         Api: DescribeInstances
         InstanceIds:
           - '{{InstanceId}}'
         PropertySelector: '$.Reservations[0].Instances[0].State.Name'
         DesiredValues:
           - running
       nextStep: patchInstance
     - name: verifyInstanceStopped
       action: 'aws:waitForAwsResourceProperty'
       timeoutSeconds: 120
       inputs:
         Service: ec2
         Api: DescribeInstances
         InstanceIds:
           - '{{InstanceId}}'
         PropertySelector: '$.Reservations[0].Instances[0].State.Name'
         DesiredValues:
           - stopped
         nextStep: startInstance
     - name: patchInstance
       action: 'aws:runCommand'
       onFailure: Abort
       timeoutSeconds: 5400
       inputs:
         DocumentName: 'AWS-RunPatchBaseline'
         InstanceIds: 
         - '{{InstanceId}}'
         Parameters:
           SnapshotId: '{{SnapshotId}}'
           RebootOption: '{{RebootOption}}'
           Operation: '{{Operation}}'
   ```

------
#### [ JSON ]

   ```
   {
            "name":"branchOnInstanceState",
            "action":"aws:branch",
            "onFailure":"Abort",
            "inputs":{
               "Choices":[
                  {
                     "NextStep":"startInstance",
                     "Variable":"{{getInstanceState.instanceState}}",
                     "StringEquals":"stopped"
                  },
                  {
                     "Or":[
                        {
                           "Variable":"{{getInstanceState.instanceState}}",
                           "StringEquals":"stopping"
                        }
                     ],
                     "NextStep":"verifyInstanceStopped"
                  },
                  {
                     "NextStep":"patchInstance",
                     "Variable":"{{getInstanceState.instanceState}}",
                     "StringEquals":"running"
                  }
               ]
            },
            "isEnd":true
         },
         {
            "name":"startInstance",
            "action":"aws:executeAwsApi",
            "onFailure":"Abort",
            "inputs":{
               "Service":"ec2",
               "Api":"StartInstances",
               "InstanceIds":[
                  "{{InstanceId}}"
               ]
            },
            "nextStep":"verifyInstanceRunning"
         },
         {
            "name":"verifyInstanceRunning",
            "action":"aws:waitForAwsResourceProperty",
            "timeoutSeconds":120,
            "inputs":{
               "Service":"ec2",
               "Api":"DescribeInstances",
               "InstanceIds":[
                  "{{InstanceId}}"
               ],
               "PropertySelector":"$.Reservations[0].Instances[0].State.Name",
               "DesiredValues":[
                  "running"
               ]
            },
            "nextStep":"patchInstance"
         },
         {
            "name":"verifyInstanceStopped",
            "action":"aws:waitForAwsResourceProperty",
            "timeoutSeconds":120,
            "inputs":{
               "Service":"ec2",
               "Api":"DescribeInstances",
               "InstanceIds":[
                  "{{InstanceId}}"
               ],
               "PropertySelector":"$.Reservations[0].Instances[0].State.Name",
               "DesiredValues":[
                  "stopped"
               ],
               "nextStep":"startInstance"
            }
         },
         {
            "name":"patchInstance",
            "action":"aws:runCommand",
            "onFailure":"Abort",
            "timeoutSeconds":5400,
            "inputs":{
               "DocumentName":"AWS-RunPatchBaseline",
               "InstanceIds":[
                  "{{InstanceId}}"
               ],
               "Parameters":{
                  "SnapshotId":"{{SnapshotId}}",
                  "RebootOption":"{{RebootOption}}",
                  "Operation":"{{Operation}}"
               }
            }
         },
   ```

------

1. After the patching operation completes, Emily wants the automation to return the target instance to the same state it was in before the automation started\. She does this by again using the output from the first action\. The automation branches based on the original state of the target instance using the `aws:branch` action\. If the instance was previously in any state other than `running`, the instance is stopped\. Otherwise, if the instance state is `running`, the automation ends\.

------
#### [ YAML ]

   ```
   - name: branchOnOriginalInstanceState
       action: 'aws:branch'
       onFailure: Abort
       inputs:
         Choices:
           - NextStep: stopInstance
             Not: 
               Variable: '{{getInstanceState.instanceState}}'
               StringEquals: running
       isEnd: true
     - name: stopInstance
       action: 'aws:executeAwsApi'
       onFailure: Abort
       inputs:
         Service: ec2
         Api: StopInstances
         InstanceIds:
           - '{{InstanceId}}'
   ```

------
#### [ JSON ]

   ```
   {
            "name":"branchOnOriginalInstanceState",
            "action":"aws:branch",
            "onFailure":"Abort",
            "inputs":{
               "Choices":[
                  {
                     "NextStep":"stopInstance",
                     "Not":{
                        "Variable":"{{getInstanceState.instanceState}}",
                        "StringEquals":"running"
                     }
                  }
               ]
            },
            "isEnd":true
         },
         {
            "name":"stopInstance",
            "action":"aws:executeAwsApi",
            "onFailure":"Abort",
            "inputs":{
               "Service":"ec2",
               "Api":"StopInstances",
               "InstanceIds":[
                  "{{InstanceId}}"
               ]
            }
         }
      ]
   }
   ```

------

1. Emily reviews the completed child runbook content and creates the runbook in the same AWS account and AWS Region as the target instances\. Now she's ready to continue with the creation of the parent runbook's content\. The following is the completed child runbook content\.

------
#### [ YAML ]

   ```
   schemaVersion: '0.3'
   description: 'An example of an Automation runbook that patches groups of Amazon EC2 instances in stages.'
   assumeRole: '{{AutomationAssumeRole}}'
   parameters:
     AutomationAssumeRole:
       type: String
       description: >-
         '(Optional) The Amazon Resource Name (ARN) of the IAM role that allows Automation to perform the
         actions on your behalf. If no role is specified, Systems Manager
         Automation uses your IAM permissions to operate this runbook.'
       default: ''
     InstanceId:
       type: String
       description: >-
         '(Required) The instance you want to patch.'
     SnapshotId:
       type: String
       description: '(Optional) The snapshot ID to use to retrieve a patch baseline snapshot.'
       default: ''
     RebootOption:
       type: String
       description: '(Optional) Reboot behavior after a patch Install operation. If you choose NoReboot and patches are installed, the instance is marked as non-compliant until a subsequent reboot and scan.'
       allowedValues:
         - NoReboot
         - RebootIfNeeded
       default: RebootIfNeeded
     Operation:
       type: String
       description: '(Optional) The update or configuration to perform on the instance. The system checks if patches specified in the patch baseline are installed on the instance. The install operation installs patches missing from the baseline.'
       allowedValues:
         - Install
         - Scan
       default: Install
   mainSteps:
     - name: getInstanceState
       action: 'aws:executeAwsApi'
       onFailure: Abort
       inputs:
         inputs:
         Service: ec2
         Api: DescribeInstances
         InstanceIds:
           - '{{InstanceId}}'
       outputs:
         - Name: instanceState
           Selector: '$.Reservations[0].Instances[0].State.Name'
           Type: String
       nextStep: branchOnInstanceState
     - name: branchOnInstanceState
       action: 'aws:branch'
       onFailure: Abort
       inputs:
         Choices:
           - NextStep: startInstance
             Variable: '{{getInstanceState.instanceState}}'
             StringEquals: stopped
           - Or:
               - Variable: '{{getInstanceState.instanceState}}'
                 StringEquals: stopping
             NextStep: verifyInstanceStopped
           - NextStep: patchInstance
             Variable: '{{getInstanceState.instanceState}}'
             StringEquals: running
       isEnd: true
     - name: startInstance
       action: 'aws:executeAwsApi'
       onFailure: Abort
       inputs:
         Service: ec2
         Api: StartInstances
         InstanceIds:
           - '{{InstanceId}}'
       nextStep: verifyInstanceRunning
     - name: verifyInstanceRunning
       action: 'aws:waitForAwsResourceProperty'
       timeoutSeconds: 120
       inputs:
         Service: ec2
         Api: DescribeInstances
         InstanceIds:
           - '{{InstanceId}}'
         PropertySelector: '$.Reservations[0].Instances[0].State.Name'
         DesiredValues:
           - running
       nextStep: patchInstance
     - name: verifyInstanceStopped
       action: 'aws:waitForAwsResourceProperty'
       timeoutSeconds: 120
       inputs:
         Service: ec2
         Api: DescribeInstances
         InstanceIds:
           - '{{InstanceId}}'
         PropertySelector: '$.Reservations[0].Instances[0].State.Name'
         DesiredValues:
           - stopped
         nextStep: startInstance
     - name: patchInstance
       action: 'aws:runCommand'
       onFailure: Abort
       timeoutSeconds: 5400
       inputs:
         DocumentName: 'AWS-RunPatchBaseline'
         InstanceIds: 
         - '{{InstanceId}}'
         Parameters:
           SnapshotId: '{{SnapshotId}}'
           RebootOption: '{{RebootOption}}'
           Operation: '{{Operation}}'
     - name: branchOnOriginalInstanceState
       action: 'aws:branch'
       onFailure: Abort
       inputs:
         Choices:
           - NextStep: stopInstance
             Not: 
               Variable: '{{getInstanceState.instanceState}}'
               StringEquals: running
       isEnd: true
     - name: stopInstance
       action: 'aws:executeAwsApi'
       onFailure: Abort
       inputs:
         Service: ec2
         Api: StopInstances
         InstanceIds:
           - '{{InstanceId}}'
   ```

------
#### [ JSON ]

   ```
   {
      "schemaVersion":"0.3",
      "description":"An example of an Automation runbook that patches groups of Amazon EC2 instances in stages.",
      "assumeRole":"{{AutomationAssumeRole}}",
      "parameters":{
         "AutomationAssumeRole":{
            "type":"String",
            "description":"'(Optional) The Amazon Resource Name (ARN) of the IAM role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses your IAM permissions to operate this runbook.'",
            "default":""
         },
         "InstanceId":{
            "type":"String",
            "description":"'(Required) The instance you want to patch.'"
         },
         "SnapshotId":{
            "type":"String",
            "description":"(Optional) The snapshot ID to use to retrieve a patch baseline snapshot.",
            "default":""
         },
         "RebootOption":{
            "type":"String",
            "description":"(Optional) Reboot behavior after a patch Install operation. If you choose NoReboot and patches are installed, the instance is marked as non-compliant until a subsequent reboot and scan.",
            "allowedValues":[
               "NoReboot",
               "RebootIfNeeded"
            ],
            "default":"RebootIfNeeded"
         },
         "Operation":{
            "type":"String",
            "description":"(Optional) The update or configuration to perform on the instance. The system checks if patches specified in the patch baseline are installed on the instance. The install operation installs patches missing from the baseline.",
            "allowedValues":[
               "Install",
               "Scan"
            ],
            "default":"Install"
         }
      },
      "mainSteps":[
         {
            "name":"getInstanceState",
            "action":"aws:executeAwsApi",
            "onFailure":"Abort",
            "inputs":{
               "inputs":null,
               "Service":"ec2",
               "Api":"DescribeInstances",
               "InstanceIds":[
                  "{{InstanceId}}"
               ]
            },
            "outputs":[
               {
                  "Name":"instanceState",
                  "Selector":"$.Reservations[0].Instances[0].State.Name",
                  "Type":"String"
               }
            ],
            "nextStep":"branchOnInstanceState"
         },
         {
            "name":"branchOnInstanceState",
            "action":"aws:branch",
            "onFailure":"Abort",
            "inputs":{
               "Choices":[
                  {
                     "NextStep":"startInstance",
                     "Variable":"{{getInstanceState.instanceState}}",
                     "StringEquals":"stopped"
                  },
                  {
                     "Or":[
                        {
                           "Variable":"{{getInstanceState.instanceState}}",
                           "StringEquals":"stopping"
                        }
                     ],
                     "NextStep":"verifyInstanceStopped"
                  },
                  {
                     "NextStep":"patchInstance",
                     "Variable":"{{getInstanceState.instanceState}}",
                     "StringEquals":"running"
                  }
               ]
            },
            "isEnd":true
         },
         {
            "name":"startInstance",
            "action":"aws:executeAwsApi",
            "onFailure":"Abort",
            "inputs":{
               "Service":"ec2",
               "Api":"StartInstances",
               "InstanceIds":[
                  "{{InstanceId}}"
               ]
            },
            "nextStep":"verifyInstanceRunning"
         },
         {
            "name":"verifyInstanceRunning",
            "action":"aws:waitForAwsResourceProperty",
            "timeoutSeconds":120,
            "inputs":{
               "Service":"ec2",
               "Api":"DescribeInstances",
               "InstanceIds":[
                  "{{InstanceId}}"
               ],
               "PropertySelector":"$.Reservations[0].Instances[0].State.Name",
               "DesiredValues":[
                  "running"
               ]
            },
            "nextStep":"patchInstance"
         },
         {
            "name":"verifyInstanceStopped",
            "action":"aws:waitForAwsResourceProperty",
            "timeoutSeconds":120,
            "inputs":{
               "Service":"ec2",
               "Api":"DescribeInstances",
               "InstanceIds":[
                  "{{InstanceId}}"
               ],
               "PropertySelector":"$.Reservations[0].Instances[0].State.Name",
               "DesiredValues":[
                  "stopped"
               ],
               "nextStep":"startInstance"
            }
         },
         {
            "name":"patchInstance",
            "action":"aws:runCommand",
            "onFailure":"Abort",
            "timeoutSeconds":5400,
            "inputs":{
               "DocumentName":"AWS-RunPatchBaseline",
               "InstanceIds":[
                  "{{InstanceId}}"
               ],
               "Parameters":{
                  "SnapshotId":"{{SnapshotId}}",
                  "RebootOption":"{{RebootOption}}",
                  "Operation":"{{Operation}}"
               }
            }
         },
         {
            "name":"branchOnOriginalInstanceState",
            "action":"aws:branch",
            "onFailure":"Abort",
            "inputs":{
               "Choices":[
                  {
                     "NextStep":"stopInstance",
                     "Not":{
                        "Variable":"{{getInstanceState.instanceState}}",
                        "StringEquals":"running"
                     }
                  }
               ]
            },
            "isEnd":true
         },
         {
            "name":"stopInstance",
            "action":"aws:executeAwsApi",
            "onFailure":"Abort",
            "inputs":{
               "Service":"ec2",
               "Api":"StopInstances",
               "InstanceIds":[
                  "{{InstanceId}}"
               ]
            }
         }
      ]
   }
   ```

------

For more information about the automation actions used in this example, see the [Systems Manager Automation actions reference](automation-actions.md)\.

## Create the parent runbook<a name="automation-authoring-runbooks-parent-runbook"></a>

This example runbook continues the scenario described in the previous section\. Now that Emily has created the child runbook, she begins authoring the content for the parent runbook as follows:

1. First, she provides values for the schema and description of the runbook, and defines the input parameters for the parent runbook\.

   By using the `AutomationAssumeRole` parameter, Emily and her colleagues can use an existing IAM role that allows Automation to perform the actions in the runbook on their behalf\. Emily uses the `PatchGroupPrimaryKey` and `PatchGroupPrimaryValue` parameters to specify the tag associated with the primary group of database instances that will be patched\. She uses the `PatchGroupSecondaryKey` and `PatchGroupSecondaryValue` parameters to specify the tag associated with the secondary group of database instances that will be patched\.

------
#### [ YAML ]

   ```
   description: 'An example of an Automation runbook that patches groups of Amazon EC2 instances in stages.'
   schemaVersion: '0.3'
   assumeRole: '{{AutomationAssumeRole}}'
   parameters:
     AutomationAssumeRole:
       type: String
       description: '(Optional) The Amazon Resource Name (ARN) of the IAM role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses your IAM permissions to operate this runbook.'
       default: ''
     PatchGroupPrimaryKey:
       type: String
       description: '(Required) The key of the tag for the primary group of instances you want to patch.''
     PatchGroupPrimaryValue:
       type: String
       description: '(Required) The value of the tag for the primary group of instances you want to patch.'
     PatchGroupSecondaryKey:
       type: String
       description: '(Required) The key of the tag for the secondary group of instances you want to patch.'
     PatchGroupSecondaryValue:
       type: String
       description: '(Required) The value of the tag for the secondary group of instances you want to patch.'
   ```

------
#### [ JSON ]

   ```
   {
      "schemaVersion": "0.3",
      "description": "An example of an Automation runbook that patches groups of Amazon EC2 instances in stages.",
      "assumeRole": "{{AutomationAssumeRole}}",
      "parameters": {
         "AutomationAssumeRole": {
            "type": "String",
            "description": "(Optional) The Amazon Resource Name (ARN) of the IAM role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses your IAM permissions to operate this runbook.",
            "default": ""
         },
         "PatchGroupPrimaryKey": {
            "type": "String",
            "description": "(Required) The key of the tag for the primary group of instances you want to patch."
         },
         "PatchGroupPrimaryValue": {
            "type": "String",
            "description": "(Required) The value of the tag for the primary group of instances you want to patch."
         },
         "PatchGroupSecondaryKey": {
            "type": "String",
            "description": "(Required) The key of the tag for the secondary group of instances you want to patch."
         },
         "PatchGroupSecondaryValue": {
            "type": "String",
            "description": "(Required) The value of the tag for the secondary group of instances you want to patch."
         }
      }
   },
   ```

------

1. With the top\-level elements defined, Emily proceeds with authoring the actions that make up the `mainSteps` of the runbook\. 

   The first action starts a rate control automation using the child runbook she just created that targets instances associated with the tag specified in the `PatchGroupPrimaryKey` and `PatchGroupPrimaryValue` input parameters\. She uses the values provided to the input parameters to specify the key and value of the tag associated with the primary group of database instances she wants to patch\.

   After the first automation completes, the second action starts another rate control automation using the child runbook that targets instances associated with the tag specified in the `PatchGroupSecondaryKey` and `PatchGroupSecondaryValue` input parameters\. She uses the values provided to the input parameters to specify the key and value of the tag associated with the secondary group of database instances she wants to patch\.

------
#### [ YAML ]

   ```
   mainSteps:
     - name: patchPrimaryTargets
       action: 'aws:executeAutomation'
       onFailure: Abort
       timeoutSeconds: 7200
       inputs:
         DocumentName: RunbookTutorialChildAutomation
         Targets:
           - Key: 'tag:{{PatchGroupPrimaryKey}}'
             Values:
               - '{{PatchGroupPrimaryValue}}'
         TargetParameterName: 'InstanceId'
     - name: patchSecondaryTargets
       action: 'aws:executeAutomation'
       onFailure: Abort
       timeoutSeconds: 7200
       inputs:
         DocumentName: RunbookTutorialChildAutomation
         Targets:
           - Key: 'tag:{{PatchGroupSecondaryKey}}'
             Values:
               - '{{PatchGroupSecondaryValue}}'
         TargetParameterName: 'InstanceId'
   ```

------
#### [ JSON ]

   ```
   "mainSteps":[
         {
            "name":"patchPrimaryTargets",
            "action":"aws:executeAutomation",
            "onFailure":"Abort",
            "timeoutSeconds":7200,
            "inputs":{
               "DocumentName":"RunbookTutorialChildAutomation",
               "Targets":[
                  {
                     "Key":"tag:{{PatchGroupPrimaryKey}}",
                     "Values":[
                        "{{PatchGroupPrimaryValue}}"
                     ]
                  }
               ],
               "TargetParameterName":"InstanceId"
            }
         },
         {
            "name":"patchSecondaryTargets",
            "action":"aws:executeAutomation",
            "onFailure":"Abort",
            "timeoutSeconds":7200,
            "inputs":{
               "DocumentName":"RunbookTutorialChildAutomation",
               "Targets":[
                  {
                     "Key":"tag:{{PatchGroupSecondaryKey}}",
                     "Values":[
                        "{{PatchGroupSecondaryValue}}"
                     ]
                  }
               ],
               "TargetParameterName":"InstanceId"
            }
         }
      ]
   }
   ```

------

1. Emily reviews the completed parent runbook content and creates the runbook in the same AWS account and AWS Region as the target instances\. Now, she is ready to test her runbooks to make sure the automation operates as desired before implementing them into her production environment\. The following is the completed parent runbook content\.

------
#### [ YAML ]

   ```
   description: An example of an Automation runbook that patches groups of Amazon EC2 instances in stages.
   schemaVersion: '0.3'
   assumeRole: '{{AutomationAssumeRole}}'
   parameters:
     AutomationAssumeRole:
       type: String
       description: '(Optional) The Amazon Resource Name (ARN) of the IAM role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses your IAM permissions to operate this runbook.'
       default: ''
     PatchGroupPrimaryKey:
       type: String
       description: (Required) The key of the tag for the primary group of instances you want to patch.
     PatchGroupPrimaryValue:
       type: String
       description: '(Required) The value of the tag for the primary group of instances you want to patch. '
     PatchGroupSecondaryKey:
       type: String
       description: (Required) The key of the tag for the secondary group of instances you want to patch.
     PatchGroupSecondaryValue:
       type: String
       description: '(Required) The value of the tag for the secondary group of instances you want to patch.  '
   mainSteps:
     - name: patchPrimaryTargets
       action: 'aws:executeAutomation'
       onFailure: Abort
       timeoutSeconds: 7200
       inputs:
         DocumentName: RunbookTutorialChildAutomation
         Targets:
           - Key: 'tag:{{PatchGroupPrimaryKey}}'
             Values:
               - '{{PatchGroupPrimaryValue}}'
         TargetParameterName: 'InstanceId'
     - name: patchSecondaryTargets
       action: 'aws:executeAutomation'
       onFailure: Abort
       timeoutSeconds: 7200
       inputs:
         DocumentName: RunbookTutorialChildAutomation
         Targets:
           - Key: 'tag:{{PatchGroupSecondaryKey}}'
             Values:
               - '{{PatchGroupSecondaryValue}}'
         TargetParameterName: 'InstanceId'
   ```

------
#### [ JSON ]

   ```
   {
      "description":"An example of an Automation runbook that patches groups of Amazon EC2 instances in stages.",
      "schemaVersion":"0.3",
      "assumeRole":"{{AutomationAssumeRole}}",
      "parameters":{
         "AutomationAssumeRole":{
            "type":"String",
            "description":"(Optional) The Amazon Resource Name (ARN) of the IAM role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses your IAM permissions to operate this runbook.",
            "default":""
         },
         "PatchGroupPrimaryKey":{
            "type":"String",
            "description":"(Required) The key of the tag for the primary group of instances you want to patch."
         },
         "PatchGroupPrimaryValue":{
            "type":"String",
            "description":"(Required) The value of the tag for the primary group of instances you want to patch. "
         },
         "PatchGroupSecondaryKey":{
            "type":"String",
            "description":"(Required) The key of the tag for the secondary group of instances you want to patch."
         },
         "PatchGroupSecondaryValue":{
            "type":"String",
            "description":"(Required) The value of the tag for the secondary group of instances you want to patch.  "
         }
      },
      "mainSteps":[
         {
            "name":"patchPrimaryTargets",
            "action":"aws:executeAutomation",
            "onFailure":"Abort",
            "timeoutSeconds":7200,
            "inputs":{
               "DocumentName":"RunbookTutorialChildAutomation",
               "Targets":[
                  {
                     "Key":"tag:{{PatchGroupPrimaryKey}}",
                     "Values":[
                        "{{PatchGroupPrimaryValue}}"
                     ]
                  }
               ],
               "TargetParameterName":"InstanceId"
            }
         },
         {
            "name":"patchSecondaryTargets",
            "action":"aws:executeAutomation",
            "onFailure":"Abort",
            "timeoutSeconds":7200,
            "inputs":{
               "DocumentName":"RunbookTutorialChildAutomation",
               "Targets":[
                  {
                     "Key":"tag:{{PatchGroupSecondaryKey}}",
                     "Values":[
                        "{{PatchGroupSecondaryValue}}"
                     ]
                  }
               ],
               "TargetParameterName":"InstanceId"
            }
         }
      ]
   }
   ```

------

For more information about the automation actions used in this example, see the [Systems Manager Automation actions reference](automation-actions.md)\.