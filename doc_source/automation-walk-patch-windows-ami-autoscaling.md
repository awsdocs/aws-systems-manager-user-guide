# Patch an AMI and update an Auto Scaling group<a name="automation-walk-patch-windows-ami-autoscaling"></a>

The following example updates an Auto Scaling group with a newly patched AMI\. This approach ensures that new images are automatically made available to different computing environments that use Auto Scaling groups\.

The final step of the automation in this example uses a Python function to create a new launch template that uses the newly patched AMI\. Then the Auto Scaling group is updated to use the new launch template\. In this type of Auto Scaling scenario, users could terminate existing instances in the Auto Scaling group to force a new instance to launch that uses the new image\. Or, users could wait and allow scale\-in or scale\-out events to naturally launch newer instances\.

**Before you begin**  
Complete the following tasks before you begin this example\.
+ Configure IAM roles for Automation, a capability of AWS Systems Manager\. Systems Manager requires an instance profile role and a service role ARN to process automations\. For more information, see [Setting up Automation](automation-setup.md)\.

## Create the **PatchAMIAndUpdateASG** runbook<a name="update-asg"></a>

Use the following procedure to create the **PatchAMIAndUpdateASG** runbook that patches the AMI you specify for the **SourceAMI** parameter\. The runbook also updates an Auto Scaling group to use the latest, patched AMI\.

**To create and run the runbook**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. In the **Create document** dropdown, choose **Automation**\.

1. In the **Name** field, enter **PatchAMIAndUpdateASG**\.

1. Choose the **Editor** tab, and choose the **Edit**\.

1. Choose **OK** when prompted, and delete the content in the **Document editor** field\.

1. In the **Document editor** field, paste the following YAML sample runbook content\.

   ```
   ---
   description: Systems Manager Automation Demo - Patch AMI and Update ASG
   schemaVersion: '0.3'
   assumeRole: '{{ AutomationAssumeRole }}'
   parameters:
     AutomationAssumeRole:
       type: String
       description: '(Required) The ARN of the role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses your IAM permissions to execute this document.'
       default: ''
     SourceAMI:
       type: String
       description: '(Required) The ID of the AMI you want to patch.'
     SubnetId:
       type: String
       description: '(Required) The ID of the subnet where the instance from the SourceAMI parameter is launched.'
     SecurityGroupIds:
       type: StringList
       description: '(Required) The IDs of the security groups to associate with the instance launched from the SourceAMI parameter.'
     NewAMI:
       type: String
       description: '(Optional) The name of of newly patched AMI.'
       default: 'patchedAMI-{{global:DATE_TIME}}'
     TargetASG:
       type: String
       description: '(Required) The name of the Auto Scaling group you want to update.'
     InstanceProfile:
       type: String
       description: '(Required) The name of the IAM instance profile you want the source instance to use.'
     SnapshotId:
       type: String
       description: (Optional) The snapshot ID to use to retrieve a patch baseline snapshot.
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
       description: (Optional) The update or configuration to perform on the instance. The system checks if patches specified in the patch baseline are installed on the instance. The install operation installs patches missing from the baseline.
       allowedValues:
         - Install
         - Scan
       default: Install
   mainSteps:
     - name: startInstances
       action: 'aws:runInstances'
       timeoutSeconds: 1200
       maxAttempts: 1
       onFailure: Abort
       inputs:
         ImageId: '{{ SourceAMI }}'
         InstanceType: m5.large
         MinInstanceCount: 1
         MaxInstanceCount: 1
         IamInstanceProfileName: '{{ InstanceProfile }}'
         SubnetId: '{{ SubnetId }}'
         SecurityGroupIds: '{{ SecurityGroupIds }}'
     - name: verifyInstanceManaged
       action: 'aws:waitForAwsResourceProperty'
       timeoutSeconds: 600
       inputs:
         Service: ssm
         Api: DescribeInstanceInformation
         InstanceInformationFilterList:
           - key: InstanceIds
             valueSet:
               - '{{ startInstances.InstanceIds }}'
         PropertySelector: '$.InstanceInformationList[0].PingStatus'
         DesiredValues:
           - Online
       onFailure: 'step:terminateInstance'
     - name: installPatches
       action: 'aws:runCommand'
       timeoutSeconds: 7200
       onFailure: Abort
       inputs:
         DocumentName: AWS-RunPatchBaseline
         Parameters:
           SnapshotId: '{{SnapshotId}}'
           RebootOption: '{{RebootOption}}'
           Operation: '{{Operation}}'
         InstanceIds:
           - '{{ startInstances.InstanceIds }}'
     - name: stopInstance
       action: 'aws:changeInstanceState'
       maxAttempts: 1
       onFailure: Continue
       inputs:
         InstanceIds:
           - '{{ startInstances.InstanceIds }}'
         DesiredState: stopped
     - name: createImage
       action: 'aws:createImage'
       maxAttempts: 1
       onFailure: Continue
       inputs:
         InstanceId: '{{ startInstances.InstanceIds }}'
         ImageName: '{{ NewAMI }}'
         NoReboot: false
         ImageDescription: Patched AMI created by Automation
     - name: terminateInstance
       action: 'aws:changeInstanceState'
       maxAttempts: 1
       onFailure: Continue
       inputs:
         InstanceIds:
           - '{{ startInstances.InstanceIds }}'
         DesiredState: terminated
     - name: updateASG
       action: 'aws:executeScript'
       timeoutSeconds: 300
       maxAttempts: 1
       onFailure: Abort
       inputs:
         Runtime: python3.8
         Handler: update_asg
         InputPayload:
           TargetASG: '{{TargetASG}}'
           NewAMI: '{{createImage.ImageId}}'
         Script: |-
           from __future__ import print_function
           import datetime
           import json
           import time
           import boto3
   
           # create auto scaling and ec2 client
           asg = boto3.client('autoscaling')
           ec2 = boto3.client('ec2')
   
           def update_asg(event, context):
               print("Received event: " + json.dumps(event, indent=2))
   
               target_asg = event['TargetASG']
               new_ami = event['NewAMI']
   
               # get object for the ASG we're going to update, filter by name of target ASG
               asg_query = asg.describe_auto_scaling_groups(AutoScalingGroupNames=[target_asg])
               if 'AutoScalingGroups' not in asg_query or not asg_query['AutoScalingGroups']:
                   return 'No ASG found matching the value you specified.'
   
               # gets details of an instance from the ASG that we'll use to model the new launch template after
               source_instance_id = asg_query.get('AutoScalingGroups')[0]['Instances'][0]['InstanceId']
               instance_properties = ec2.describe_instances(
                   InstanceIds=[source_instance_id]
               )
               source_instance = instance_properties['Reservations'][0]['Instances'][0]
   
               # create list of security group IDs
               security_groups = []
               for group in source_instance['SecurityGroups']:
                   security_groups.append(group['GroupId'])
   
               # create a list of dictionary objects for block device mappings
               mappings = []
               for block in source_instance['BlockDeviceMappings']:
                   volume_query = ec2.describe_volumes(
                       VolumeIds=[block['Ebs']['VolumeId']]
                   )
                   volume_details = volume_query['Volumes']
                   device_name = block['DeviceName']
                   volume_size = volume_details[0]['Size']
                   volume_type = volume_details[0]['VolumeType']
                   device = {'DeviceName': device_name, 'Ebs': {'VolumeSize': volume_size, 'VolumeType': volume_type}}
                   mappings.append(device)
   
               # create new launch template using details returned from instance in the ASG and specify the newly patched AMI
               time_stamp = time.time()
               time_stamp_string = datetime.datetime.fromtimestamp(time_stamp).strftime('%m-%d-%Y_%H-%M-%S')
               new_template_name = f'{new_ami}_{time_stamp_string}'
               try:
                   ec2.create_launch_template(
                       LaunchTemplateName=new_template_name,
                       LaunchTemplateData={
                           'BlockDeviceMappings': mappings,
                           'ImageId': new_ami,
                           'InstanceType': source_instance['InstanceType'],
                           'IamInstanceProfile': {
                               'Arn': source_instance['IamInstanceProfile']['Arn']
                           },
                           'KeyName': source_instance['KeyName'],
                           'SecurityGroupIds': security_groups
                       }
                   )
               except Exception as e:
                   return f'Exception caught: {str(e)}'
               else:
                   # update ASG to use new launch template
                   asg.update_auto_scaling_group(
                       AutoScalingGroupName=target_asg,
                       LaunchTemplate={
                           'LaunchTemplateName': new_template_name
                       }
                   )
                   return f'Updated ASG {target_asg} with new launch template {new_template_name} which uses AMI {new_ami}.'
   outputs:
     - createImage.ImageId
   ```

1. Choose **Create automation**\.

1. In the navigation pane, choose **Automation**, and then choose **Execute automation**\.

1. In the **Choose document** page, choose the **Owned by me** tab\.

1. Search for the **PatchAMIAndUpdateASG** runbook, and select the button in the **PatchAMIAndUpdateASG** card\.

1. Choose **Next**\.

1. Choose **Simple execution**\.

1. Specify values for the input parameters\. Be sure the `SubnetId` and `SecurityGroupIds` you specify allow access to the public Systems Manager endpoints, or your interface endpoints for Systems Manager\.

1. Choose **Execute**\.

1. After automation completes, in the Amazon EC2 console, choose **Auto Scaling**, and then choose **Launch Templates**\. Verify that you see the new launch template, and that it uses the new AMI\.

1. Choose **Auto Scaling**, and then choose **Auto Scaling Groups**\. Verify that the Auto Scaling group uses the new launch template\.

1. Terminate one or more instances in your Auto Scaling group\. Replacement instances will be launched using the new AMI\.