# Restore a root volume from the latest snapshot<a name="automation-document-sample-restore"></a>

The operating system on a root volume can become corrupted for various reasons\. For example, following a patching operation, instances may fail to boot successfully due to a corrupted kernel or registry\. Automating common troubleshooting tasks, like restoring a root volume from the latest snapshot taken before the patching operation, can reduce downtime and expedite your troubleshooting efforts\. AWS Systems Manager Automation actions can help you accomplish this\.

The following sample AWS Systems Manager Automation document performs these actions\. 
+ Uses the `aws:executeAwsApi` Automation action to retrieve details from the root volume of the instance\.
+ Uses the `aws:executeScript` Automation action to retrieve the latest snapshot for the root volume\.
+ Uses the `aws:branch` Automation action to continue the execution if a snapshot is found for the root volume\.

------
#### [ YAML ]

```
---
description: Custom Automation Troubleshooting Sample
schemaVersion: '0.3'
assumeRole: "{{ AutomationAssumeRole }}"
parameters:
  AutomationAssumeRole:
    type: String
    description: "(Required) The ARN of the role that allows Automation to perform
      the actions on your behalf. If no role is specified, Systems Manager Automation
      uses your IAM permissions to execute this document."
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

------
#### [ JSON ]

```
{
   "description": "Custom Automation Troubleshooting Sample",
   "schemaVersion": "0.3",
   "assumeRole": "{{ AutomationAssumeRole }}",
   "parameters": {
      "AutomationAssumeRole": {
         "type": "String",
         "description": "(Required) The ARN of the role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses your IAM permissions to run this document.",
         "default": ""
      },
      "InstanceId": {
         "type": "String",
         "description": "(Required) The Instance Id whose root EBS volume you want to restore the latest Snapshot.",
         "default": ""
      }
   },
   "mainSteps": [
      {
         "name": "getInstanceDetails",
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
               "Name": "availabilityZone",
               "Selector": "$.Reservations[0].Instances[0].Placement.AvailabilityZone",
               "Type": "String"
            },
            {
               "Name": "rootDeviceName",
               "Selector": "$.Reservations[0].Instances[0].RootDeviceName",
               "Type": "String"
            }
         ],
         "nextStep": "getRootVolumeId"
      },
      {
         "name": "getRootVolumeId",
         "action": "aws:executeAwsApi",
         "onFailure": "Abort",
         "inputs": {
            "Service": "ec2",
            "Api": "DescribeVolumes",
            "Filters": [
               {
                  "Name": "attachment.device",
                  "Values": [
                     "{{ getInstanceDetails.rootDeviceName }}"
                  ]
               },
               {
                  "Name": "attachment.instance-id",
                  "Values": [
                     "{{ InstanceId }}"
                  ]
               }
            ]
         },
         "outputs": [
            {
               "Name": "rootVolumeId",
               "Selector": "$.Volumes[0].VolumeId",
               "Type": "String"
            }
         ],
         "nextStep": "getSnapshotsByStartTime"
      },
      {
         "name": "getSnapshotsByStartTime",
         "action": "aws:executeScript",
         "timeoutSeconds": 45,
         "onFailure": "Continue",
         "inputs": {
            "Runtime": "python3.6",
            "Handler": "getSnapshotsByStartTime",
            "InputPayload": {
               "rootVolumeId": "{{ getRootVolumeId.rootVolumeId }}"
            },
            "Attachment": "getSnapshotsByStartTime.py"
         },
         "outputs": [
            {
               "Name": "Payload",
               "Selector": "$.Payload",
               "Type": "StringMap"
            },
            {
               "Name": "latestSnapshotId",
               "Selector": "$.Payload.latestSnapshotId",
               "Type": "String"
            },
            {
               "Name": "noSnapshotFound",
               "Selector": "$.Payload.noSnapshotFound",
               "Type": "String"
            }
         ],
         "nextStep": "branchFromResults"
      },
      {
         "name": "branchFromResults",
         "action": "aws:branch",
         "onFailure": "Abort",
         "inputs": {
            "Choices": [
               {
                  "NextStep": "createNewRootVolumeFromSnapshot",
                  "Not": {
                     "Variable": "{{ getSnapshotsByStartTime.noSnapshotFound }}",
                     "StringEquals": "NoSnapshotFound"
                  }
               }
            ]
         },
         "isEnd": true
      },
      {
         "name": "createNewRootVolumeFromSnapshot",
         "action": "aws:executeAwsApi",
         "onFailure": "Abort",
         "inputs": {
            "Service": "ec2",
            "Api": "CreateVolume",
            "AvailabilityZone": "{{ getInstanceDetails.availabilityZone }}",
            "SnapshotId": "{{ getSnapshotsByStartTime.latestSnapshotId }}"
         },
         "outputs": [
            {
               "Name": "newRootVolumeId",
               "Selector": "$.VolumeId",
               "Type": "String"
            }
         ],
         "nextStep": "stopInstance"
      },
      {
         "name": "stopInstance",
         "action": "aws:executeAwsApi",
         "onFailure": "Abort",
         "inputs": {
            "Service": "ec2",
            "Api": "StopInstances",
            "InstanceIds": [
               "{{ InstanceId }}"
            ]
         },
         "nextStep": "verifyVolumeAvailability"
      },
      {
         "name": "verifyVolumeAvailability",
         "action": "aws:waitForAwsResourceProperty",
         "timeoutSeconds": 120,
         "inputs": {
            "Service": "ec2",
            "Api": "DescribeVolumes",
            "VolumeIds": [
               "{{ createNewRootVolumeFromSnapshot.newRootVolumeId }}"
            ],
            "PropertySelector": "$.Volumes[0].State",
            "DesiredValues": [
               "available"
            ]
         },
         "nextStep": "verifyInstanceStopped"
      },
      {
         "name": "verifyInstanceStopped",
         "action": "aws:waitForAwsResourceProperty",
         "timeoutSeconds": 120,
         "inputs": {
            "Service": "ec2",
            "Api": "DescribeInstances",
            "InstanceIds": [
               "{{ InstanceId }}"
            ],
            "PropertySelector": "$.Reservations[0].Instances[0].State.Name",
            "DesiredValues": [
               "stopped"
            ]
         },
         "nextStep": "detachRootVolume"
      },
      {
         "name": "detachRootVolume",
         "action": "aws:executeAwsApi",
         "onFailure": "Abort",
         "inputs": {
            "Service": "ec2",
            "Api": "DetachVolume",
            "VolumeId": "{{ getRootVolumeId.rootVolumeId }}"
         },
         "nextStep": "verifyRootVolumeDetached"
      },
      {
         "name": "verifyRootVolumeDetached",
         "action": "aws:waitForAwsResourceProperty",
         "timeoutSeconds": 30,
         "inputs": {
            "Service": "ec2",
            "Api": "DescribeVolumes",
            "VolumeIds": [
               "{{ getRootVolumeId.rootVolumeId }}"
            ],
            "PropertySelector": "$.Volumes[0].State",
            "DesiredValues": [
               "available"
            ]
         },
         "nextStep": "attachNewRootVolume"
      },
      {
         "name": "attachNewRootVolume",
         "action": "aws:executeAwsApi",
         "onFailure": "Abort",
         "inputs": {
            "Service": "ec2",
            "Api": "AttachVolume",
            "Device": "{{ getInstanceDetails.rootDeviceName }}",
            "InstanceId": "{{ InstanceId }}",
            "VolumeId": "{{ createNewRootVolumeFromSnapshot.newRootVolumeId }}"
         },
         "nextStep": "verifyNewRootVolumeAttached"
      },
      {
         "name": "verifyNewRootVolumeAttached",
         "action": "aws:waitForAwsResourceProperty",
         "timeoutSeconds": 30,
         "inputs": {
            "Service": "ec2",
            "Api": "DescribeVolumes",
            "VolumeIds": [
               "{{ createNewRootVolumeFromSnapshot.newRootVolumeId }}"
            ],
            "PropertySelector": "$.Volumes[0].Attachments[0].State",
            "DesiredValues": [
               "attached"
            ]
         },
         "nextStep": "startInstance"
      },
      {
         "name": "startInstance",
         "action": "aws:executeAwsApi",
         "onFailure": "Abort",
         "inputs": {
            "Service": "ec2",
            "Api": "StartInstances",
            "InstanceIds": [
               "{{ InstanceId }}"
            ]
         }
      }
   ],
   "files": {
        "getSnapshotsByStartTime.py": {
            "checksums": {
                "sha256": "sampleETagValue"
            }
        }
    }
}
```

------