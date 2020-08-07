# Systems Manager Automation actions reference<a name="automation-actions"></a>

This reference describes the Automation actions that you can specify in an AWS Systems Manager Automation document\. These actions cannot be used in other types of SSM documents\. For information about plugins for other types of SSM documents, see [Systems Manager Command document plugin reference](ssm-plugins.md)\.

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
+ [Automation system variables](automation-variables.md)

## Properties shared by all actions<a name="automation-common"></a>

Common properties are parameters or options that are found in all actions\. Some options define execution behavior for a step, such as how long to wait for a step to complete and what to do if the step fails\. The following properties are common to all actions\.

[name](#nameProp)  
An identifier that must be unique across all step names in the document\.  
Type: String  
Required: Yes

[action](#actProp)  
The name of the action the step is to run\. [aws:runCommand – Run a command on a managed instance](automation-action-runcommand.md) is an example of an action you can specify here\. This document provides detailed information about all available actions\.  
Type: String  
Required: Yes

[maxAttempts](#maxProp)  
The number of times the step should be retried in case of failure\. If the value is greater than 1, the step is not considered to have failed until all retry attempts have failed\. The default value is 1\.  
Type: Integer  
Required: No

[timeoutSeconds](#timeProp)  
The execution timeout value for the step\. If the timeout is reached and the value of `maxAttempts` is greater than 1, then the step is not considered to have timed out until all retries have been attempted\.  
Type: Integer  
Required: No

[onFailure](#failProp)  
Indicates whether the workflow should abort, continue, or go to a different step on failure\. The default value for this option is abort\.  
Type: String  
Valid values: Abort \| Continue \| step:*step\_name*  
Required: No

[isEnd](#endProp)  
This option stops an Automation execution at the end of a specific step\. The Automation execution stops if the step execution failed or succeeded\. The default value is false\.  
Type: Boolean  
Valid values: true \| false  
Required: No

[nextStep](#nextProp)  
Specifies which step in an Automation workflow to process next after successfully completing a step\.  
Type: String  
Required: No

[isCritical](#critProp)  
Designates a step as critical for the successful completion of the Automation\. If a step with this designation fails, then Automation reports the final status of the Automation as Failed\. This property is only evaluated if you explicitly define it in your step\. The default value for this option is true\.  
Type: Boolean  
Valid values: true \| false  
Required: No

[inputs](#inProp)  
The properties specific to the action\.  
Type: Map  
Required: Yes

### Example<a name="automation-demo"></a>

```
---
description: "Custom Automation Example"
schemaVersion: '0.3'
assumeRole: "{{ AutomationAssumeRole }}"
parameters:
  AutomationAssumeRole:
    type: String
    description: "(Required) The ARN of the role that allows Automation to perform
      the actions on your behalf. If no role is specified, Systems Manager Automation
      uses your IAM permissions to run this document."
    default: ''
  InstanceId:
      type: String
      description: "(Required) The Instance Id whose root EBS volume you want to restore the latest Snapshot."
      default: ''
mainSteps:
- name: getInstanceDetails
  action: aws:executeAwsApi
  onFailure: Abort
  inputs:
    Service: ec2
    Api: DescribeInstances
    InstanceIds:
    - "{{ InstanceId }}"
  outputs:
    - Name: availabilityZone
      Selector: "$.Reservations[0].Instances[0].Placement.AvailabilityZone"
      Type: String
    - Name: rootDeviceName
      Selector: "$.Reservations[0].Instances[0].RootDeviceName"
      Type: String
  nextStep: getRootVolumeId
- name: getRootVolumeId
  action: aws:executeAwsApi
  maxAttempts: 3
  onFailure: Abort
  inputs:
    Service: ec2
    Api: DescribeVolumes
    Filters:
    -  Name: attachment.device
       Values: ["{{ getInstanceDetails.rootDeviceName }}"]
    -  Name: attachment.instance-id
       Values: ["{{ InstanceId }}"]
  outputs:
    - Name: rootVolumeId
      Selector: "$.Volumes[0].VolumeId"
      Type: String
  nextStep: getSnapshotsByStartTime
- name: getSnapshotsByStartTime
  action: aws:executeScript
  timeoutSeconds: 45
  onFailure: Abort
  inputs:
    Runtime: python3.6
    Handler: getSnapshotsByStartTime
    InputPayload:
      rootVolumeId : "{{ getRootVolumeId.rootVolumeId }}"
    Script: |-
      def getSnapshotsByStartTime(events,context):
        import boto3

        #Initialize client
        ec2 = boto3.client('ec2')
        rootVolumeId = events['rootVolumeId']
        snapshotsQuery = ec2.describe_snapshots(
          Filters=[
            {
              "Name": "volume-id",
              "Values": [rootVolumeId]
            }
          ]
        )
        if not snapshotsQuery['Snapshots']:
          noSnapshotFoundString = "NoSnapshotFound"
          return { 'noSnapshotFound' : noSnapshotFoundString }
        else:
          jsonSnapshots = snapshotsQuery['Snapshots']
          sortedSnapshots = sorted(jsonSnapshots, key=lambda k: k['StartTime'], reverse=True)
          latestSortedSnapshotId = sortedSnapshots[0]['SnapshotId']
          return { 'latestSnapshotId' : latestSortedSnapshotId }
  outputs:
  - Name: Payload
    Selector: $.Payload
    Type: StringMap
  - Name: latestSnapshotId
    Selector: $.Payload.latestSnapshotId
    Type: String
  - Name: noSnapshotFound
    Selector: $.Payload.noSnapshotFound
    Type: String 
  nextStep: branchFromResults
- name: branchFromResults
  action: aws:branch
  onFailure: Abort
  inputs:
    Choices:
    - NextStep: createNewRootVolumeFromSnapshot
      Not:
        Variable: "{{ getSnapshotsByStartTime.noSnapshotFound }}"
        StringEquals: "NoSnapshotFound"
  isEnd: true
- name: createNewRootVolumeFromSnapshot
  action: aws:executeAwsApi
  onFailure: Abort
  inputs:
    Service: ec2
    Api: CreateVolume
    AvailabilityZone: "{{ getInstanceDetails.availabilityZone }}"
    SnapshotId: "{{ getSnapshotsByStartTime.latestSnapshotId }}"
  outputs:
    - Name: newRootVolumeId
      Selector: "$.VolumeId"
      Type: String
  nextStep: stopInstance
- name: stopInstance
  action: aws:executeAwsApi
  onFailure: Abort
  inputs:
    Service: ec2
    Api: StopInstances
    InstanceIds:
    - "{{ InstanceId }}"
  nextStep: verifyVolumeAvailability
- name: verifyVolumeAvailability
  action: aws:waitForAwsResourceProperty
  timeoutSeconds: 120
  inputs:
    Service: ec2
    Api: DescribeVolumes
    VolumeIds:
    - "{{ createNewRootVolumeFromSnapshot.newRootVolumeId }}"
    PropertySelector: "$.Volumes[0].State"
    DesiredValues:
    - "available"
  nextStep: verifyInstanceStopped
- name: verifyInstanceStopped
  action: aws:waitForAwsResourceProperty
  timeoutSeconds: 120
  inputs:
    Service: ec2
    Api: DescribeInstances
    InstanceIds:
    - "{{ InstanceId }}"
    PropertySelector: "$.Reservations[0].Instances[0].State.Name"
    DesiredValues:
    - "stopped"
  nextStep: detachRootVolume
- name: detachRootVolume
  action: aws:executeAwsApi
  onFailure: Abort
  isCritical: true
  inputs:
    Service: ec2
    Api: DetachVolume
    VolumeId: "{{ getRootVolumeId.rootVolumeId }}"
  nextStep: verifyRootVolumeDetached
- name: verifyRootVolumeDetached
  action: aws:waitForAwsResourceProperty
  timeoutSeconds: 30
  inputs:
    Service: ec2
    Api: DescribeVolumes
    VolumeIds:
    - "{{ getRootVolumeId.rootVolumeId }}"
    PropertySelector: "$.Volumes[0].State"
    DesiredValues:
    - "available"
  nextStep: attachNewRootVolume
- name: attachNewRootVolume
  action: aws:executeAwsApi
  onFailure: Abort
  inputs:
    Service: ec2
    Api: AttachVolume
    Device: "{{ getInstanceDetails.rootDeviceName }}"
    InstanceId: "{{ InstanceId }}"
    VolumeId: "{{ createNewRootVolumeFromSnapshot.newRootVolumeId }}"
  nextStep: verifyNewRootVolumeAttached
- name: verifyNewRootVolumeAttached
  action: aws:waitForAwsResourceProperty
  timeoutSeconds: 30
  inputs:
    Service: ec2
    Api: DescribeVolumes
    VolumeIds:
    - "{{ createNewRootVolumeFromSnapshot.newRootVolumeId }}"
    PropertySelector: "$.Volumes[0].Attachments[0].State"
    DesiredValues:
    - "attached"
  nextStep: startInstance
- name: startInstance
  action: aws:executeAwsApi
  onFailure: Abort
  inputs:
    Service: ec2
    Api: StartInstances
    InstanceIds:
    - "{{ InstanceId }}"
```