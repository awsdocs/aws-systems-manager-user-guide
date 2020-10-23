# Create an AMI and cross\-Region copy<a name="automation-document-sample-bandr"></a>

Creating an Amazon Machine Image \(AMI\) of an instance is a common process used in backup and recovery\. You might also choose to copy an AMI to another Region as part of a disaster recovery architecture\. Automating common maintenance tasks can reduce downtime if an issue requires failover\. AWS Systems Manager Automation actions can help you accomplish this\.

The following sample AWS Systems Manager Automation document below performs these actions\. 
+ Uses the `aws:executeAwsApi` Automation action to create an AMI\.
+ Uses the `aws:waitForAwsResourceProperty` Automation action to confirm the availability of the AMI\.
+ Uses the `aws:executeScript` Automation action to copy the AMI to the destination Region\.

------
#### [ YAML ]

```
---
description: Custom Automation Backup and Recovery Sample
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
    description: "(Required) The ID of the EC2 instance."
    default: ''
mainSteps:
- name: createImage
  action: aws:executeAwsApi
  onFailure: Abort
  inputs:
    Service: ec2
    Api: CreateImage
    InstanceId: "{{ InstanceId }}"
    Name: "Automation Image for {{ InstanceId }}"
    NoReboot: false
  outputs:
    - Name: newImageId
      Selector: "$.ImageId"
      Type: String
  nextStep: verifyImageAvailability
- name: verifyImageAvailability
  action: aws:waitForAwsResourceProperty
  timeoutSeconds: 600
  inputs:
    Service: ec2
    Api: DescribeImages
    ImageIds:
    - "{{ createImage.newImageId }}"
    PropertySelector: "$.Images[0].State"
    DesiredValues:
    - available
  nextStep: copyImage
- name: copyImage
  action: aws:executeScript
  timeoutSeconds: 45
  onFailure: Abort
  inputs:
    Runtime: python3.6
    Handler: crossRegionImageCopy
    InputPayload:
      newImageId : "{{ createImage.newImageId }}"
    Script: |-
      def crossRegionImageCopy(events,context):
        import boto3

        #Initialize client
        ec2 = boto3.client('ec2', region_name='us-east-1')
        newImageId = events['newImageId']

        ec2.copy_image(
          Name='DR Copy for ' + newImageId,
          SourceImageId=newImageId,
          SourceRegion='us-west-2'
        )
```

------
#### [ JSON ]

```
{
   "description": "Custom Automation Backup and Recovery Sample",
   "schemaVersion": "0.3",
   "assumeRole": "{{ AutomationAssumeRole }}",
   "parameters": {
      "AutomationAssumeRole": {
         "type": "String",
         "description": "(Required) The ARN of the role that allows Automation to perform\nthe actions on your behalf. If no role is specified, Systems Manager Automation\nuses your IAM permissions to run this document.",
         "default": ""
      },
      "InstanceId": {
         "type": "String",
         "description": "(Required) The ID of the EC2 instance.",
         "default": ""
      }
   },
   "mainSteps": [
      {
         "name": "createImage",
         "action": "aws:executeAwsApi",
         "onFailure": "Abort",
         "inputs": {
            "Service": "ec2",
            "Api": "CreateImage",
            "InstanceId": "{{ InstanceId }}",
            "Name": "Automation Image for {{ InstanceId }}",
            "NoReboot": false
         },
         "outputs": [
            {
               "Name": "newImageId",
               "Selector": "$.ImageId",
               "Type": "String"
            }
         ],
         "nextStep": "verifyImageAvailability"
      },
      {
         "name": "verifyImageAvailability",
         "action": "aws:waitForAwsResourceProperty",
         "timeoutSeconds": 600,
         "inputs": {
            "Service": "ec2",
            "Api": "DescribeImages",
            "ImageIds": [
               "{{ createImage.newImageId }}"
            ],
            "PropertySelector": "$.Images[0].State",
            "DesiredValues": [
               "available"
            ]
         },
         "nextStep": "copyImage"
      },
      {
         "name": "copyImage",
         "action": "aws:executeScript",
         "timeoutSeconds": 45,
         "onFailure": "Abort",
         "inputs": {
            "Runtime": "python3.6",
            "Handler": "crossRegionImageCopy",
            "InputPayload": {
               "newImageId": "{{ createImage.newImageId }}"
            },
            "Attachment": "crossRegionImageCopy.py"
         }
      }
   ],
   "files": {
        "crossRegionImageCopy.py": {
            "checksums": {
                "sha256": "sampleETagValue"
            }
        }
    }
}
```

------