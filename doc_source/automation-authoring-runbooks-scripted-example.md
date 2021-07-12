# Example 2: Scripted runbook<a name="automation-authoring-runbooks-scripted-example"></a>

This example runbook addresses the following scenario\. Emily is a Systems Engineer at AnyCompany Consultants, LLC\. She previously created two runbooks that are used in a parent\-child relationship to patch groups of Amazon Elastic Compute Cloud \(Amazon EC2\) instances that host primary and secondary databases\. Applications access these databases 24 hours a day, so one of the database instances must always be available\. 

Based on this requirement, she built a solution that patches the instances in stages using the `AWS-RunPatchBaseline` Systems Manager \(SSM\) document\. By using this SSM document, her colleagues can review the associated patch compliance information after the patching operation completes\. 

The primary group of database instances are patched first, followed by the secondary group of database instances\. Also, to avoid incurring additional costs by leaving instances running that were previously stopped, Emily made sure that the automation returned the patched instances to their original state before the patching occurred\. Emily used tags that are associated with the primary and secondary groups of database instances to identify which instances should be patched in her desired order\.

Her existing automated solution works, but she wants to improve her solution if possible\. To help with the maintenance of the runbook content and to ease troubleshooting efforts, she would like to condense the automation into a single runbook and simplify the number of input parameters\. Also, she would like to avoid creating multiple child automations\. 

After Emily reviews the available automation actions, she determines that she can improve her solution by using the `aws:executeScript` action to run her custom Python scripts\. She now begins authoring the content for the runbook as follows:

1. First, she provides values for the schema and description of the runbook, and defines the input parameters for the parent runbook\.

   By using the `AutomationAssumeRole` parameter, Emily and her colleagues can use an existing IAM role that allows Automation to perform the actions in the runbook on their behalf\. Unlike [Example 1](automation-authoring-runbooks-parent-child-example.md#automation-authoring-runbooks-parent-child-example.title), the `AutomationAssumeRole` parameter is now required rather than optional\. Because this runbook includes `aws:executeScript` actions, an AWS Identity and Access Management \(IAM\) service role \(or assume role\) is always required\. This requirement is necessary because some of the Python scripts specified for the actions call AWS API operations\.

   Emily uses the `PrimaryPatchGroupTag` and `SecondaryPatchGroupTag` parameters to specify the tags associated with the primary and secondary group of database instances that will be patched\. To simplify the required input parameters, she decides to use `StringMap` parameters rather than using multiple `String` parameters as she used in the Example 1 runbook\. Optionally, the `Operation`, `RebootOption`, and `SnapshotId` parameters can be used to provide values to document parameters for `AWS-RunPatchBaseline`\. To prevent invalid values from being provided to those document parameters, she defines the `allowedValues` as needed\.

------
#### [ YAML ]

   ```
   description: 'An example of an Automation runbook that patches groups of Amazon EC2 instances in stages.'
   schemaVersion: '0.3'
   assumeRole: '{{AutomationAssumeRole}}'
   parameters:
     AutomationAssumeRole:
       type: String
       description: '(Required) The Amazon Resource Name (ARN) of the IAM role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses your IAM permissions to operate this runbook.'
     PrimaryPatchGroupTag:
       type: StringMap
       description: '(Required) The tag for the primary group of instances you want to patch. Specify a key-value pair. Example: {"key" : "value"}'
     SecondaryPatchGroupTag:
       type: StringMap
       description: '(Required) The tag for the secondary group of instances you want to patch. Specify a key-value pair. Example: {"key" : "value"}'
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
      "description":"An example of an Automation runbook that patches groups of Amazon EC2 instances in stages.",
      "schemaVersion":"0.3",
      "assumeRole":"{{AutomationAssumeRole}}",
      "parameters":{
         "AutomationAssumeRole":{
            "type":"String",
            "description":"(Required) The Amazon Resource Name (ARN) of the IAM role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses your IAM permissions to operate this runbook."
         },
         "PrimaryPatchGroupTag":{
            "type":"StringMap",
            "description":"(Required) The tag for the primary group of instances you want to patch. Specify a key-value pair. Example: {\"key\" : \"value\"}"
         },
         "SecondaryPatchGroupTag":{
            "type":"StringMap",
            "description":"(Required) The tag for the secondary group of instances you want to patch. Specify a key-value pair. Example: {\"key\" : \"value\"}"
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

1. With the top\-level elements defined, Emily proceeds with authoring the actions that make up the `mainSteps` of the runbook\. The first step gathers the IDs of all instances associated with the tag specified in the `PrimaryPatchGroupTag` parameter and outputs a `StringMap` parameter containing the instance ID and the current state of the instance\. The output of this action is used in later actions\. 

   Note that the `script` input parameter isn't supported for JSON runbooks\. JSON runbooks must provide script content using the `attachment` input parameter\.

------
#### [ YAML ]

   ```
   mainSteps:
     - name: getPrimaryInstanceState
       action: 'aws:executeScript'
       timeoutSeconds: 120
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: getInstanceStates
         InputPayload:
           primaryTag: '{{PrimaryPatchGroupTag}}'
         Script: |-
           def getInstanceStates(events,context):
             import boto3
   
             #Initialize client
             ec2 = boto3.client('ec2')
             tag = events['primaryTag']
             tagKey, tagValue = list(tag.items())[0]
             instanceQuery = ec2.describe_instances(
             Filters=[
                 {
                     "Name": "tag:" + tagKey,
                     "Values": [tagValue]
                 }]
             )
             if not instanceQuery['Reservations']:
                 noInstancesForTagString = "No instances found for specified tag."
                 return({ 'noInstancesFound' : noInstancesForTagString })
             else:
                 queryResponse = instanceQuery['Reservations']
                 originalInstanceStates = {}
                 for results in queryResponse:
                     instanceSet = results['Instances']
                     for instance in instanceSet:
                         instanceId = instance['InstanceId']
                         originalInstanceStates[instanceId] = instance['State']['Name']
                 return originalInstanceStates
       outputs:
         - Name: originalInstanceStates
           Selector: $.Payload
           Type: StringMap
       nextStep: verifyPrimaryInstancesRunning
   ```

------
#### [ JSON ]

   ```
   "mainSteps":[
         {
            "name":"getPrimaryInstanceState",
            "action":"aws:executeScript",
            "timeoutSeconds":120,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"getInstanceStates",
               "InputPayload":{
                  "primaryTag":"{{PrimaryPatchGroupTag}}"
               },
               "Script":"..."
            },
            "outputs":[
               {
                  "Name":"originalInstanceStates",
                  "Selector":"$.Payload",
                  "Type":"StringMap"
               }
            ],
            "nextStep":"verifyPrimaryInstancesRunning"
         },
   ```

------

1. Emily uses the output from the previous action in another `aws:executeScript` action to verify all instances associated with the tag specified in the `PrimaryPatchGroupTag` parameter are in a `running` state\.

   If the instance state is already `running` or `shutting-down`, the script continues to loop through the remaining instances\.

   If the instance state is `stopping`, the script polls for the instance to reach the `stopped` state and starts the instance\.

   If the instance state is `stopped`, the script starts the instance\.

------
#### [ YAML ]

   ```
   - name: verifyPrimaryInstancesRunning
       action: 'aws:executeScript'
       timeoutSeconds: 600
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: verifyInstancesRunning
         InputPayload:
           targetInstances: '{{getPrimaryInstanceState.originalInstanceStates}}'
         Script: |-
           def verifyInstancesRunning(events,context):
             import boto3
   
             #Initialize client
             ec2 = boto3.client('ec2')
             instanceDict = events['targetInstances']
             for instance in instanceDict:
               if instanceDict[instance] == 'stopped':
                   print("The target instance " + instance + " is stopped. The instance will now be started.")
                   ec2.start_instances(
                       InstanceIds=[instance]
                       )
               elif instanceDict[instance] == 'stopping':
                   print("The target instance " + instance + " is stopping. Polling for instance to reach stopped state.")
                   while instanceDict[instance] != 'stopped':
                       poll = ec2.get_waiter('instance_stopped')
                       poll.wait(
                           InstanceIds=[instance]
                       )
                   ec2.start_instances(
                       InstanceIds=[instance]
                   )
               else:
                 pass
       nextStep: waitForPrimaryRunningInstances
   ```

------
#### [ JSON ]

   ```
   {
            "name":"verifyPrimaryInstancesRunning",
            "action":"aws:executeScript",
            "timeoutSeconds":600,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"verifyInstancesRunning",
               "InputPayload":{
                  "targetInstances":"{{getPrimaryInstanceState.originalInstanceStates}}"
               },
               "Script":"..."
            },
            "nextStep":"waitForPrimaryRunningInstances"
         },
   ```

------

1. Emily verifies that all instances associated with the tag specified in the `PrimaryPatchGroupTag` parameter were started or already in a `running` state\. Then she uses another script to verify that all instances, including those that were started in the previous action, have reached the `running` state\.

------
#### [ YAML ]

   ```
   - name: waitForPrimaryRunningInstances
       action: 'aws:executeScript'
       timeoutSeconds: 300
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: waitForRunningInstances
         InputPayload:
           targetInstances: '{{getPrimaryInstanceState.originalInstanceStates}}'
         Script: |-
           def waitForRunningInstances(events,context):
             import boto3
   
             #Initialize client
             ec2 = boto3.client('ec2')
             instanceDict = events['targetInstances']
             for instance in instanceDict:
                 poll = ec2.get_waiter('instance_running')
                 poll.wait(
                     InstanceIds=[instance]
                 )
       nextStep: returnPrimaryTagKey
   ```

------
#### [ JSON ]

   ```
   {
            "name":"waitForPrimaryRunningInstances",
            "action":"aws:executeScript",
            "timeoutSeconds":300,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"waitForRunningInstances",
               "InputPayload":{
                  "targetInstances":"{{getPrimaryInstanceState.originalInstanceStates}}"
               },
               "Script":"..."
            },
            "nextStep":"returnPrimaryTagKey"
         },
   ```

------

1. Emily uses two more scripts to return individual `String` values of the key and value of the tag specified in the `PrimaryPatchGroupTag` parameter\. The values returned by these actions allows her to provide values directly to the `Targets` parameter for the `AWS-RunPatchBaseline` document\. The automation then proceeds with patching the instance with the `AWS-RunPatchBaseline` document using the `aws:runCommand` action\.

------
#### [ YAML ]

   ```
   - name: returnPrimaryTagKey
       action: 'aws:executeScript'
       timeoutSeconds: 120
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: returnTagValues
         InputPayload:
           primaryTag: '{{PrimaryPatchGroupTag}}'
         Script: |-
           def returnTagValues(events,context):
             tag = events['primaryTag']
             tagKey = list(tag)[0]
             stringKey = "tag:" + tagKey
             return {'tagKey' : stringKey}
       outputs:
         - Name: Payload
           Selector: $.Payload
           Type: StringMap
         - Name: primaryPatchGroupKey
           Selector: $.Payload.tagKey
           Type: String
       nextStep: returnPrimaryTagValue
     - name: returnPrimaryTagValue
       action: 'aws:executeScript'
       timeoutSeconds: 120
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: returnTagValues
         InputPayload:
           primaryTag: '{{PrimaryPatchGroupTag}}'
         Script: |-
           def returnTagValues(events,context):
             tag = events['primaryTag']
             tagKey = list(tag)[0]
             tagValue = tag[tagKey]
             return {'tagValue' : tagValue}
       outputs:
         - Name: Payload
           Selector: $.Payload
           Type: StringMap
         - Name: primaryPatchGroupValue
           Selector: $.Payload.tagValue
           Type: String
       nextStep: patchPrimaryInstances
     - name: patchPrimaryInstances
       action: 'aws:runCommand'
       onFailure: Abort
       timeoutSeconds: 7200
       inputs:
         DocumentName: AWS-RunPatchBaseline
         Parameters:
           SnapshotId: '{{SnapshotId}}'
           RebootOption: '{{RebootOption}}'
           Operation: '{{Operation}}'
         Targets:
           - Key: '{{returnPrimaryTagKey.primaryPatchGroupKey}}'
             Values:
               - '{{returnPrimaryTagValue.primaryPatchGroupValue}}'
         MaxConcurrency: 10%
         MaxErrors: 10%
       nextStep: returnPrimaryToOriginalState
   ```

------
#### [ JSON ]

   ```
   {
            "name":"returnPrimaryTagKey",
            "action":"aws:executeScript",
            "timeoutSeconds":120,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"returnTagValues",
               "InputPayload":{
                  "primaryTag":"{{PrimaryPatchGroupTag}}"
               },
               "Script":"..."
            },
            "outputs":[
               {
                  "Name":"Payload",
                  "Selector":"$.Payload",
                  "Type":"StringMap"
               },
               {
                  "Name":"primaryPatchGroupKey",
                  "Selector":"$.Payload.tagKey",
                  "Type":"String"
               }
            ],
            "nextStep":"returnPrimaryTagValue"
         },
         {
            "name":"returnPrimaryTagValue",
            "action":"aws:executeScript",
            "timeoutSeconds":120,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"returnTagValues",
               "InputPayload":{
                  "primaryTag":"{{PrimaryPatchGroupTag}}"
               },
               "Script":"..."
            },
            "outputs":[
               {
                  "Name":"Payload",
                  "Selector":"$.Payload",
                  "Type":"StringMap"
               },
               {
                  "Name":"primaryPatchGroupValue",
                  "Selector":"$.Payload.tagValue",
                  "Type":"String"
               }
            ],
            "nextStep":"patchPrimaryInstances"
         },
         {
            "name":"patchPrimaryInstances",
            "action":"aws:runCommand",
            "onFailure":"Abort",
            "timeoutSeconds":7200,
            "inputs":{
               "DocumentName":"AWS-RunPatchBaseline",
               "Parameters":{
                  "SnapshotId":"{{SnapshotId}}",
                  "RebootOption":"{{RebootOption}}",
                  "Operation":"{{Operation}}"
               },
               "Targets":[
                  {
                     "Key":"{{returnPrimaryTagKey.primaryPatchGroupKey}}",
                     "Values":[
                        "{{returnPrimaryTagValue.primaryPatchGroupValue}}"
                     ]
                  }
               ],
               "MaxConcurrency":"10%",
               "MaxErrors":"10%"
            },
            "nextStep":"returnPrimaryToOriginalState"
         },
   ```

------

1. After the patching operation completes, Emily wants the automation to return the target instances associated with the tag specified in the `PrimaryPatchGroupTag` parameter to the same state they were before the automation started\. She does this by again using the output from the first action in a script\. Based on the original state of the target instance, if the instance was previously in any state other than `running`, the instance is stopped\. Otherwise, if the instance state is `running`, the script continues to loop through the remaining instances\.

------
#### [ YAML ]

   ```
   - name: returnPrimaryToOriginalState
       action: 'aws:executeScript'
       timeoutSeconds: 600
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: returnToOriginalState
         InputPayload:
           targetInstances: '{{getPrimaryInstanceState.originalInstanceStates}}'
         Script: |-
           def returnToOriginalState(events,context):
             import boto3
   
             #Initialize client
             ec2 = boto3.client('ec2')
             instanceDict = events['targetInstances']
             for instance in instanceDict:
               if instanceDict[instance] == 'stopped' or instanceDict[instance] == 'stopping':
                   ec2.stop_instances(
                       InstanceIds=[instance]
                       )
               else:
                 pass
       nextStep: getSecondaryInstanceState
   ```

------
#### [ JSON ]

   ```
   {
            "name":"returnPrimaryToOriginalState",
            "action":"aws:executeScript",
            "timeoutSeconds":600,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"returnToOriginalState",
               "InputPayload":{
                  "targetInstances":"{{getPrimaryInstanceState.originalInstanceStates}}"
               },
               "Script":"..."
            },
            "nextStep":"getSecondaryInstanceState"
         },
   ```

------

1. The patching operation is completed for the instances associated with the tag specified in the `PrimaryPatchGroupTag` parameter\. Now Emily duplicates all of the previous actions in her runbook content to target the instances associated with the tag specified in the `SecondaryPatchGroupTag` parameter\.

------
#### [ YAML ]

   ```
   - name: getSecondaryInstanceState
       action: 'aws:executeScript'
       timeoutSeconds: 120
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: getInstanceStates
         InputPayload:
           secondaryTag: '{{SecondaryPatchGroupTag}}'
         Script: |-
           def getInstanceStates(events,context):
             import boto3
   
             #Initialize client
             ec2 = boto3.client('ec2')
             tag = events['secondaryTag']
             tagKey, tagValue = list(tag.items())[0]
             instanceQuery = ec2.describe_instances(
             Filters=[
                 {
                     "Name": "tag:" + tagKey,
                     "Values": [tagValue]
                 }]
             )
             if not instanceQuery['Reservations']:
                 noInstancesForTagString = "No instances found for specified tag."
                 return({ 'noInstancesFound' : noInstancesForTagString })
             else:
                 queryResponse = instanceQuery['Reservations']
                 originalInstanceStates = {}
                 for results in queryResponse:
                     instanceSet = results['Instances']
                     for instance in instanceSet:
                         instanceId = instance['InstanceId']
                         originalInstanceStates[instanceId] = instance['State']['Name']
                 return originalInstanceStates
       outputs:
         - Name: originalInstanceStates
           Selector: $.Payload
           Type: StringMap
       nextStep: verifySecondaryInstancesRunning
     - name: verifySecondaryInstancesRunning
       action: 'aws:executeScript'
       timeoutSeconds: 600
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: verifyInstancesRunning
         InputPayload:
           targetInstances: '{{getSecondaryInstanceState.originalInstanceStates}}'
         Script: |-
           def verifyInstancesRunning(events,context):
             import boto3
   
             #Initialize client
             ec2 = boto3.client('ec2')
             instanceDict = events['targetInstances']
             for instance in instanceDict:
               if instanceDict[instance] == 'stopped':
                   print("The target instance " + instance + " is stopped. The instance will now be started.")
                   ec2.start_instances(
                       InstanceIds=[instance]
                       )
               elif instanceDict[instance] == 'stopping':
                   print("The target instance " + instance + " is stopping. Polling for instance to reach stopped state.")
                   while instanceDict[instance] != 'stopped':
                       poll = ec2.get_waiter('instance_stopped')
                       poll.wait(
                           InstanceIds=[instance]
                       )
                   ec2.start_instances(
                       InstanceIds=[instance]
                   )
               else:
                 pass
       nextStep: waitForSecondaryRunningInstances
     - name: waitForSecondaryRunningInstances
       action: 'aws:executeScript'
       timeoutSeconds: 300
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: waitForRunningInstances
         InputPayload:
           targetInstances: '{{getSecondaryInstanceState.originalInstanceStates}}'
         Script: |-
           def waitForRunningInstances(events,context):
             import boto3
   
             #Initialize client
             ec2 = boto3.client('ec2')
             instanceDict = events['targetInstances']
             for instance in instanceDict:
                 poll = ec2.get_waiter('instance_running')
                 poll.wait(
                     InstanceIds=[instance]
                 )
       nextStep: returnSecondaryTagKey
     - name: returnSecondaryTagKey
       action: 'aws:executeScript'
       timeoutSeconds: 120
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: returnTagValues
         InputPayload:
           secondaryTag: '{{SecondaryPatchGroupTag}}'
         Script: |-
           def returnTagValues(events,context):
             tag = events['secondaryTag']
             tagKey = list(tag)[0]
             stringKey = "tag:" + tagKey
             return {'tagKey' : stringKey}
       outputs:
         - Name: Payload
           Selector: $.Payload
           Type: StringMap
         - Name: secondaryPatchGroupKey
           Selector: $.Payload.tagKey
           Type: String
       nextStep: returnSecondaryTagValue
     - name: returnSecondaryTagValue
       action: 'aws:executeScript'
       timeoutSeconds: 120
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: returnTagValues
         InputPayload:
           secondaryTag: '{{SecondaryPatchGroupTag}}'
         Script: |-
           def returnTagValues(events,context):
             tag = events['secondaryTag']
             tagKey = list(tag)[0]
             tagValue = tag[tagKey]
             return {'tagValue' : tagValue}
       outputs:
         - Name: Payload
           Selector: $.Payload
           Type: StringMap
         - Name: secondaryPatchGroupValue
           Selector: $.Payload.tagValue
           Type: String
       nextStep: patchSecondaryInstances
     - name: patchSecondaryInstances
       action: 'aws:runCommand'
       onFailure: Abort
       timeoutSeconds: 7200
       inputs:
         DocumentName: AWS-RunPatchBaseline
         Parameters:
           SnapshotId: '{{SnapshotId}}'
           RebootOption: '{{RebootOption}}'
           Operation: '{{Operation}}'
         Targets:
           - Key: '{{returnSecondaryTagKey.secondaryPatchGroupKey}}'
             Values:
             - '{{returnSecondaryTagValue.secondaryPatchGroupValue}}'
         MaxConcurrency: 10%
         MaxErrors: 10%
       nextStep: returnSecondaryToOriginalState
     - name: returnSecondaryToOriginalState
       action: 'aws:executeScript'
       timeoutSeconds: 600
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: returnToOriginalState
         InputPayload:
           targetInstances: '{{getSecondaryInstanceState.originalInstanceStates}}'
         Script: |-
           def returnToOriginalState(events,context):
             import boto3
   
             #Initialize client
             ec2 = boto3.client('ec2')
             instanceDict = events['targetInstances']
             for instance in instanceDict:
               if instanceDict[instance] == 'stopped' or instanceDict[instance] == 'stopping':
                   ec2.stop_instances(
                       InstanceIds=[instance]
                       )
               else:
                 pass
   ```

------
#### [ JSON ]

   ```
   {
            "name":"getSecondaryInstanceState",
            "action":"aws:executeScript",
            "timeoutSeconds":120,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"getInstanceStates",
               "InputPayload":{
                  "secondaryTag":"{{SecondaryPatchGroupTag}}"
               },
               "Script":"..."
            },
            "outputs":[
               {
                  "Name":"originalInstanceStates",
                  "Selector":"$.Payload",
                  "Type":"StringMap"
               }
            ],
            "nextStep":"verifySecondaryInstancesRunning"
         },
         {
            "name":"verifySecondaryInstancesRunning",
            "action":"aws:executeScript",
            "timeoutSeconds":600,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"verifyInstancesRunning",
               "InputPayload":{
                  "targetInstances":"{{getSecondaryInstanceState.originalInstanceStates}}"
               },
               "Script":"..."
            },
            "nextStep":"waitForSecondaryRunningInstances"
         },
         {
            "name":"waitForSecondaryRunningInstances",
            "action":"aws:executeScript",
            "timeoutSeconds":300,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"waitForRunningInstances",
               "InputPayload":{
                  "targetInstances":"{{getSecondaryInstanceState.originalInstanceStates}}"
               },
               "Script":"..."
            },
            "nextStep":"returnSecondaryTagKey"
         },
         {
            "name":"returnSecondaryTagKey",
            "action":"aws:executeScript",
            "timeoutSeconds":120,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"returnTagValues",
               "InputPayload":{
                  "secondaryTag":"{{SecondaryPatchGroupTag}}"
               },
               "Script":"..."
            },
            "outputs":[
               {
                  "Name":"Payload",
                  "Selector":"$.Payload",
                  "Type":"StringMap"
               },
               {
                  "Name":"secondaryPatchGroupKey",
                  "Selector":"$.Payload.tagKey",
                  "Type":"String"
               }
            ],
            "nextStep":"returnSecondaryTagValue"
         },
         {
            "name":"returnSecondaryTagValue",
            "action":"aws:executeScript",
            "timeoutSeconds":120,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"returnTagValues",
               "InputPayload":{
                  "secondaryTag":"{{SecondaryPatchGroupTag}}"
               },
               "Script":"..."
            },
            "outputs":[
               {
                  "Name":"Payload",
                  "Selector":"$.Payload",
                  "Type":"StringMap"
               },
               {
                  "Name":"secondaryPatchGroupValue",
                  "Selector":"$.Payload.tagValue",
                  "Type":"String"
               }
            ],
            "nextStep":"patchSecondaryInstances"
         },
         {
            "name":"patchSecondaryInstances",
            "action":"aws:runCommand",
            "onFailure":"Abort",
            "timeoutSeconds":7200,
            "inputs":{
               "DocumentName":"AWS-RunPatchBaseline",
               "Parameters":{
                  "SnapshotId":"{{SnapshotId}}",
                  "RebootOption":"{{RebootOption}}",
                  "Operation":"{{Operation}}"
               },
               "Targets":[
                  {
                     "Key":"{{returnSecondaryTagKey.secondaryPatchGroupKey}}",
                     "Values":[
                        "{{returnSecondaryTagValue.secondaryPatchGroupValue}}"
                     ]
                  }
               ],
               "MaxConcurrency":"10%",
               "MaxErrors":"10%"
            },
            "nextStep":"returnSecondaryToOriginalState"
         },
         {
            "name":"returnSecondaryToOriginalState",
            "action":"aws:executeScript",
            "timeoutSeconds":600,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"returnToOriginalState",
               "InputPayload":{
                  "targetInstances":"{{getSecondaryInstanceState.originalInstanceStates}}"
               },
               "Script":"..."
            }
         }
      ]
   }
   ```

------

1. Emily reviews the completed scripted runbook content and creates the runbook in the same AWS account and AWS Region as the target instances\. Now she's ready to test her runbook to make sure the automation operates as desired before implementing it into her production environment\. The following is the completed scripted runbook content\.

------
#### [ YAML ]

   ```
   description: An example of an Automation runbook that patches groups of Amazon EC2 instances in stages.
   schemaVersion: '0.3'
   assumeRole: '{{AutomationAssumeRole}}'
   parameters:
     AutomationAssumeRole:
       type: String
       description: '(Required) The Amazon Resource Name (ARN) of the IAM role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses your IAM permissions to operate this runbook.'
     PrimaryPatchGroupTag:
       type: StringMap
       description: '(Required) The tag for the primary group of instances you want to patch. Specify a key-value pair. Example: {"key" : "value"}'
     SecondaryPatchGroupTag:
       type: StringMap
       description: '(Required) The tag for the secondary group of instances you want to patch. Specify a key-value pair. Example: {"key" : "value"}'
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
     - name: getPrimaryInstanceState
       action: 'aws:executeScript'
       timeoutSeconds: 120
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: getInstanceStates
         InputPayload:
           primaryTag: '{{PrimaryPatchGroupTag}}'
         Script: |-
           def getInstanceStates(events,context):
             import boto3
   
             #Initialize client
             ec2 = boto3.client('ec2')
             tag = events['primaryTag']
             tagKey, tagValue = list(tag.items())[0]
             instanceQuery = ec2.describe_instances(
             Filters=[
                 {
                     "Name": "tag:" + tagKey,
                     "Values": [tagValue]
                 }]
             )
             if not instanceQuery['Reservations']:
                 noInstancesForTagString = "No instances found for specified tag."
                 return({ 'noInstancesFound' : noInstancesForTagString })
             else:
                 queryResponse = instanceQuery['Reservations']
                 originalInstanceStates = {}
                 for results in queryResponse:
                     instanceSet = results['Instances']
                     for instance in instanceSet:
                         instanceId = instance['InstanceId']
                         originalInstanceStates[instanceId] = instance['State']['Name']
                 return originalInstanceStates
       outputs:
         - Name: originalInstanceStates
           Selector: $.Payload
           Type: StringMap
       nextStep: verifyPrimaryInstancesRunning
     - name: verifyPrimaryInstancesRunning
       action: 'aws:executeScript'
       timeoutSeconds: 600
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: verifyInstancesRunning
         InputPayload:
           targetInstances: '{{getPrimaryInstanceState.originalInstanceStates}}'
         Script: |-
           def verifyInstancesRunning(events,context):
             import boto3
   
             #Initialize client
             ec2 = boto3.client('ec2')
             instanceDict = events['targetInstances']
             for instance in instanceDict:
               if instanceDict[instance] == 'stopped':
                   print("The target instance " + instance + " is stopped. The instance will now be started.")
                   ec2.start_instances(
                       InstanceIds=[instance]
                       )
               elif instanceDict[instance] == 'stopping':
                   print("The target instance " + instance + " is stopping. Polling for instance to reach stopped state.")
                   while instanceDict[instance] != 'stopped':
                       poll = ec2.get_waiter('instance_stopped')
                       poll.wait(
                           InstanceIds=[instance]
                       )
                   ec2.start_instances(
                       InstanceIds=[instance]
                   )
               else:
                 pass
       nextStep: waitForPrimaryRunningInstances
     - name: waitForPrimaryRunningInstances
       action: 'aws:executeScript'
       timeoutSeconds: 300
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: waitForRunningInstances
         InputPayload:
           targetInstances: '{{getPrimaryInstanceState.originalInstanceStates}}'
         Script: |-
           def waitForRunningInstances(events,context):
             import boto3
   
             #Initialize client
             ec2 = boto3.client('ec2')
             instanceDict = events['targetInstances']
             for instance in instanceDict:
                 poll = ec2.get_waiter('instance_running')
                 poll.wait(
                     InstanceIds=[instance]
                 )
       nextStep: returnPrimaryTagKey
     - name: returnPrimaryTagKey
       action: 'aws:executeScript'
       timeoutSeconds: 120
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: returnTagValues
         InputPayload:
           primaryTag: '{{PrimaryPatchGroupTag}}'
         Script: |-
           def returnTagValues(events,context):
             tag = events['primaryTag']
             tagKey = list(tag)[0]
             stringKey = "tag:" + tagKey
             return {'tagKey' : stringKey}
       outputs:
         - Name: Payload
           Selector: $.Payload
           Type: StringMap
         - Name: primaryPatchGroupKey
           Selector: $.Payload.tagKey
           Type: String
       nextStep: returnPrimaryTagValue
     - name: returnPrimaryTagValue
       action: 'aws:executeScript'
       timeoutSeconds: 120
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: returnTagValues
         InputPayload:
           primaryTag: '{{PrimaryPatchGroupTag}}'
         Script: |-
           def returnTagValues(events,context):
             tag = events['primaryTag']
             tagKey = list(tag)[0]
             tagValue = tag[tagKey]
             return {'tagValue' : tagValue}
       outputs:
         - Name: Payload
           Selector: $.Payload
           Type: StringMap
         - Name: primaryPatchGroupValue
           Selector: $.Payload.tagValue
           Type: String
       nextStep: patchPrimaryInstances
     - name: patchPrimaryInstances
       action: 'aws:runCommand'
       onFailure: Abort
       timeoutSeconds: 7200
       inputs:
         DocumentName: AWS-RunPatchBaseline
         Parameters:
           SnapshotId: '{{SnapshotId}}'
           RebootOption: '{{RebootOption}}'
           Operation: '{{Operation}}'
         Targets:
           - Key: '{{returnPrimaryTagKey.primaryPatchGroupKey}}'
             Values:
               - '{{returnPrimaryTagValue.primaryPatchGroupValue}}'
         MaxConcurrency: 10%
         MaxErrors: 10%
       nextStep: returnPrimaryToOriginalState
     - name: returnPrimaryToOriginalState
       action: 'aws:executeScript'
       timeoutSeconds: 600
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: returnToOriginalState
         InputPayload:
           targetInstances: '{{getPrimaryInstanceState.originalInstanceStates}}'
         Script: |-
           def returnToOriginalState(events,context):
             import boto3
   
             #Initialize client
             ec2 = boto3.client('ec2')
             instanceDict = events['targetInstances']
             for instance in instanceDict:
               if instanceDict[instance] == 'stopped' or instanceDict[instance] == 'stopping':
                   ec2.stop_instances(
                       InstanceIds=[instance]
                       )
               else:
                 pass
       nextStep: getSecondaryInstanceState
     - name: getSecondaryInstanceState
       action: 'aws:executeScript'
       timeoutSeconds: 120
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: getInstanceStates
         InputPayload:
           secondaryTag: '{{SecondaryPatchGroupTag}}'
         Script: |-
           def getInstanceStates(events,context):
             import boto3
   
             #Initialize client
             ec2 = boto3.client('ec2')
             tag = events['secondaryTag']
             tagKey, tagValue = list(tag.items())[0]
             instanceQuery = ec2.describe_instances(
             Filters=[
                 {
                     "Name": "tag:" + tagKey,
                     "Values": [tagValue]
                 }]
             )
             if not instanceQuery['Reservations']:
                 noInstancesForTagString = "No instances found for specified tag."
                 return({ 'noInstancesFound' : noInstancesForTagString })
             else:
                 queryResponse = instanceQuery['Reservations']
                 originalInstanceStates = {}
                 for results in queryResponse:
                     instanceSet = results['Instances']
                     for instance in instanceSet:
                         instanceId = instance['InstanceId']
                         originalInstanceStates[instanceId] = instance['State']['Name']
                 return originalInstanceStates
       outputs:
         - Name: originalInstanceStates
           Selector: $.Payload
           Type: StringMap
       nextStep: verifySecondaryInstancesRunning
     - name: verifySecondaryInstancesRunning
       action: 'aws:executeScript'
       timeoutSeconds: 600
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: verifyInstancesRunning
         InputPayload:
           targetInstances: '{{getSecondaryInstanceState.originalInstanceStates}}'
         Script: |-
           def verifyInstancesRunning(events,context):
             import boto3
   
             #Initialize client
             ec2 = boto3.client('ec2')
             instanceDict = events['targetInstances']
             for instance in instanceDict:
               if instanceDict[instance] == 'stopped':
                   print("The target instance " + instance + " is stopped. The instance will now be started.")
                   ec2.start_instances(
                       InstanceIds=[instance]
                       )
               elif instanceDict[instance] == 'stopping':
                   print("The target instance " + instance + " is stopping. Polling for instance to reach stopped state.")
                   while instanceDict[instance] != 'stopped':
                       poll = ec2.get_waiter('instance_stopped')
                       poll.wait(
                           InstanceIds=[instance]
                       )
                   ec2.start_instances(
                       InstanceIds=[instance]
                   )
               else:
                 pass
       nextStep: waitForSecondaryRunningInstances
     - name: waitForSecondaryRunningInstances
       action: 'aws:executeScript'
       timeoutSeconds: 300
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: waitForRunningInstances
         InputPayload:
           targetInstances: '{{getSecondaryInstanceState.originalInstanceStates}}'
         Script: |-
           def waitForRunningInstances(events,context):
             import boto3
   
             #Initialize client
             ec2 = boto3.client('ec2')
             instanceDict = events['targetInstances']
             for instance in instanceDict:
                 poll = ec2.get_waiter('instance_running')
                 poll.wait(
                     InstanceIds=[instance]
                 )
       nextStep: returnSecondaryTagKey
     - name: returnSecondaryTagKey
       action: 'aws:executeScript'
       timeoutSeconds: 120
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: returnTagValues
         InputPayload:
           secondaryTag: '{{SecondaryPatchGroupTag}}'
         Script: |-
           def returnTagValues(events,context):
             tag = events['secondaryTag']
             tagKey = list(tag)[0]
             stringKey = "tag:" + tagKey
             return {'tagKey' : stringKey}
       outputs:
         - Name: Payload
           Selector: $.Payload
           Type: StringMap
         - Name: secondaryPatchGroupKey
           Selector: $.Payload.tagKey
           Type: String
       nextStep: returnSecondaryTagValue
     - name: returnSecondaryTagValue
       action: 'aws:executeScript'
       timeoutSeconds: 120
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: returnTagValues
         InputPayload:
           secondaryTag: '{{SecondaryPatchGroupTag}}'
         Script: |-
           def returnTagValues(events,context):
             tag = events['secondaryTag']
             tagKey = list(tag)[0]
             tagValue = tag[tagKey]
             return {'tagValue' : tagValue}
       outputs:
         - Name: Payload
           Selector: $.Payload
           Type: StringMap
         - Name: secondaryPatchGroupValue
           Selector: $.Payload.tagValue
           Type: String
       nextStep: patchSecondaryInstances
     - name: patchSecondaryInstances
       action: 'aws:runCommand'
       onFailure: Abort
       timeoutSeconds: 7200
       inputs:
         DocumentName: AWS-RunPatchBaseline
         Parameters:
           SnapshotId: '{{SnapshotId}}'
           RebootOption: '{{RebootOption}}'
           Operation: '{{Operation}}'
         Targets:
           - Key: '{{returnSecondaryTagKey.secondaryPatchGroupKey}}'
             Values:
             - '{{returnSecondaryTagValue.secondaryPatchGroupValue}}'
         MaxConcurrency: 10%
         MaxErrors: 10%
       nextStep: returnSecondaryToOriginalState
     - name: returnSecondaryToOriginalState
       action: 'aws:executeScript'
       timeoutSeconds: 600
       onFailure: Abort
       inputs:
         Runtime: python3.7
         Handler: returnToOriginalState
         InputPayload:
           targetInstances: '{{getSecondaryInstanceState.originalInstanceStates}}'
         Script: |-
           def returnToOriginalState(events,context):
             import boto3
   
             #Initialize client
             ec2 = boto3.client('ec2')
             instanceDict = events['targetInstances']
             for instance in instanceDict:
               if instanceDict[instance] == 'stopped' or instanceDict[instance] == 'stopping':
                   ec2.stop_instances(
                       InstanceIds=[instance]
                       )
               else:
                 pass
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
            "description":"(Required) The Amazon Resource Name (ARN) of the IAM role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses your IAM permissions to operate this runbook."
         },
         "PrimaryPatchGroupTag":{
            "type":"StringMap",
            "description":"(Required) The tag for the primary group of instances you want to patch. Specify a key-value pair. Example: {\"key\" : \"value\"}"
         },
         "SecondaryPatchGroupTag":{
            "type":"StringMap",
            "description":"(Required) The tag for the secondary group of instances you want to patch. Specify a key-value pair. Example: {\"key\" : \"value\"}"
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
            "name":"getPrimaryInstanceState",
            "action":"aws:executeScript",
            "timeoutSeconds":120,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"getInstanceStates",
               "InputPayload":{
                  "primaryTag":"{{PrimaryPatchGroupTag}}"
               },
               "Script":"..."
            },
            "outputs":[
               {
                  "Name":"originalInstanceStates",
                  "Selector":"$.Payload",
                  "Type":"StringMap"
               }
            ],
            "nextStep":"verifyPrimaryInstancesRunning"
         },
         {
            "name":"verifyPrimaryInstancesRunning",
            "action":"aws:executeScript",
            "timeoutSeconds":600,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"verifyInstancesRunning",
               "InputPayload":{
                  "targetInstances":"{{getPrimaryInstanceState.originalInstanceStates}}"
               },
               "Script":"..."
            },
            "nextStep":"waitForPrimaryRunningInstances"
         },
         {
            "name":"waitForPrimaryRunningInstances",
            "action":"aws:executeScript",
            "timeoutSeconds":300,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"waitForRunningInstances",
               "InputPayload":{
                  "targetInstances":"{{getPrimaryInstanceState.originalInstanceStates}}"
               },
               "Script":"..."
            },
            "nextStep":"returnPrimaryTagKey"
         },
         {
            "name":"returnPrimaryTagKey",
            "action":"aws:executeScript",
            "timeoutSeconds":120,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"returnTagValues",
               "InputPayload":{
                  "primaryTag":"{{PrimaryPatchGroupTag}}"
               },
               "Script":"..."
            },
            "outputs":[
               {
                  "Name":"Payload",
                  "Selector":"$.Payload",
                  "Type":"StringMap"
               },
               {
                  "Name":"primaryPatchGroupKey",
                  "Selector":"$.Payload.tagKey",
                  "Type":"String"
               }
            ],
            "nextStep":"returnPrimaryTagValue"
         },
         {
            "name":"returnPrimaryTagValue",
            "action":"aws:executeScript",
            "timeoutSeconds":120,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"returnTagValues",
               "InputPayload":{
                  "primaryTag":"{{PrimaryPatchGroupTag}}"
               },
               "Script":"..."
            },
            "outputs":[
               {
                  "Name":"Payload",
                  "Selector":"$.Payload",
                  "Type":"StringMap"
               },
               {
                  "Name":"primaryPatchGroupValue",
                  "Selector":"$.Payload.tagValue",
                  "Type":"String"
               }
            ],
            "nextStep":"patchPrimaryInstances"
         },
         {
            "name":"patchPrimaryInstances",
            "action":"aws:runCommand",
            "onFailure":"Abort",
            "timeoutSeconds":7200,
            "inputs":{
               "DocumentName":"AWS-RunPatchBaseline",
               "Parameters":{
                  "SnapshotId":"{{SnapshotId}}",
                  "RebootOption":"{{RebootOption}}",
                  "Operation":"{{Operation}}"
               },
               "Targets":[
                  {
                     "Key":"{{returnPrimaryTagKey.primaryPatchGroupKey}}",
                     "Values":[
                        "{{returnPrimaryTagValue.primaryPatchGroupValue}}"
                     ]
                  }
               ],
               "MaxConcurrency":"10%",
               "MaxErrors":"10%"
            },
            "nextStep":"returnPrimaryToOriginalState"
         },
         {
            "name":"returnPrimaryToOriginalState",
            "action":"aws:executeScript",
            "timeoutSeconds":600,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"returnToOriginalState",
               "InputPayload":{
                  "targetInstances":"{{getPrimaryInstanceState.originalInstanceStates}}"
               },
               "Script":"..."
            },
            "nextStep":"getSecondaryInstanceState"
         },
         {
            "name":"getSecondaryInstanceState",
            "action":"aws:executeScript",
            "timeoutSeconds":120,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"getInstanceStates",
               "InputPayload":{
                  "secondaryTag":"{{SecondaryPatchGroupTag}}"
               },
               "Script":"..."
            },
            "outputs":[
               {
                  "Name":"originalInstanceStates",
                  "Selector":"$.Payload",
                  "Type":"StringMap"
               }
            ],
            "nextStep":"verifySecondaryInstancesRunning"
         },
         {
            "name":"verifySecondaryInstancesRunning",
            "action":"aws:executeScript",
            "timeoutSeconds":600,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"verifyInstancesRunning",
               "InputPayload":{
                  "targetInstances":"{{getSecondaryInstanceState.originalInstanceStates}}"
               },
               "Script":"..."
            },
            "nextStep":"waitForSecondaryRunningInstances"
         },
         {
            "name":"waitForSecondaryRunningInstances",
            "action":"aws:executeScript",
            "timeoutSeconds":300,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"waitForRunningInstances",
               "InputPayload":{
                  "targetInstances":"{{getSecondaryInstanceState.originalInstanceStates}}"
               },
               "Script":"..."
            },
            "nextStep":"returnSecondaryTagKey"
         },
         {
            "name":"returnSecondaryTagKey",
            "action":"aws:executeScript",
            "timeoutSeconds":120,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"returnTagValues",
               "InputPayload":{
                  "secondaryTag":"{{SecondaryPatchGroupTag}}"
               },
               "Script":"..."
            },
            "outputs":[
               {
                  "Name":"Payload",
                  "Selector":"$.Payload",
                  "Type":"StringMap"
               },
               {
                  "Name":"secondaryPatchGroupKey",
                  "Selector":"$.Payload.tagKey",
                  "Type":"String"
               }
            ],
            "nextStep":"returnSecondaryTagValue"
         },
         {
            "name":"returnSecondaryTagValue",
            "action":"aws:executeScript",
            "timeoutSeconds":120,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"returnTagValues",
               "InputPayload":{
                  "secondaryTag":"{{SecondaryPatchGroupTag}}"
               },
               "Script":"..."
            },
            "outputs":[
               {
                  "Name":"Payload",
                  "Selector":"$.Payload",
                  "Type":"StringMap"
               },
               {
                  "Name":"secondaryPatchGroupValue",
                  "Selector":"$.Payload.tagValue",
                  "Type":"String"
               }
            ],
            "nextStep":"patchSecondaryInstances"
         },
         {
            "name":"patchSecondaryInstances",
            "action":"aws:runCommand",
            "onFailure":"Abort",
            "timeoutSeconds":7200,
            "inputs":{
               "DocumentName":"AWS-RunPatchBaseline",
               "Parameters":{
                  "SnapshotId":"{{SnapshotId}}",
                  "RebootOption":"{{RebootOption}}",
                  "Operation":"{{Operation}}"
               },
               "Targets":[
                  {
                     "Key":"{{returnSecondaryTagKey.secondaryPatchGroupKey}}",
                     "Values":[
                        "{{returnSecondaryTagValue.secondaryPatchGroupValue}}"
                     ]
                  }
               ],
               "MaxConcurrency":"10%",
               "MaxErrors":"10%"
            },
            "nextStep":"returnSecondaryToOriginalState"
         },
         {
            "name":"returnSecondaryToOriginalState",
            "action":"aws:executeScript",
            "timeoutSeconds":600,
            "onFailure":"Abort",
            "inputs":{
               "Runtime":"python3.7",
               "Handler":"returnToOriginalState",
               "InputPayload":{
                  "targetInstances":"{{getSecondaryInstanceState.originalInstanceStates}}"
               },
               "Script":"..."
            }
         }
      ]
   }
   ```

------

For more information about the automation actions used in this example, see the [Systems Manager Automation actions reference](automation-actions.md)\.