# Patch an AMI and Update an Auto Scaling Group<a name="automation-asgroup"></a>

The following example builds on the [Simplify AMI Patching Using Automation, Lambda, and Parameter Store](automation-simpatch.md) example by adding a step that updates an Auto Scaling group with the newly\-patched AMI\. This approach ensures that new images are automatically made available to different computing environments that use Auto Scaling groups\.

The final step of the Automation workflow in this example uses an AWS Lambda function to copy an existing launch configuration and set the AMI ID to the newly\-patched AMI\. The Auto Scaling group is then updated with the new launch configuration\. In this type of Auto Scaling scenario, users could terminate existing instances in the Auto Scaling group to force a new instance to launch that uses the new image\. Or, users could wait and allow scale\-in or scale\-out events to naturally launch newer instances\.

**Before You Begin**  
Complete the following tasks before you begin this example\.
+ Complete the [Simplify AMI Patching Using Automation, Lambda, and Parameter Store](automation-simpatch.md) example\. The following example uses the **UpdateMyLatestWindowsAmi** Automation document created in that example\.
+ Configure IAM roles for Automation\. Systems Manager requires an instance profile role and a service role ARN to process Automation workflows\. For more information, see [Setting Up Automation](automation-setup.md)\.
+ If you are not familiar with Lambda, we recommend that you create a simple Lambda function by using the [Create a Simple Lambda Function](http://docs.aws.amazon.com/lambda/latest/dg/get-started-create-function.html) topic in the *AWS Lambda Developer Guide*\. The topic will help you understand, in detail, some of the steps required to create a Lambda function\.

## Task 1: Create an IAM Role for AWS Lambda<a name="automation-asg1"></a>

Use the following procedure to create an IAM service role for AWS Lambda\. This role includes the **AWSLambdaExecute** and **AutoScalingFullAccess** managed policies\. These policies give Lambda permission to create a new Auto Scaling group with the latest, patched AMI using a Lambda function\.

**To create an IAM service role for Lambda**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. On the **Select type of trusted entity** page, under **AWS Service**, choose **Lambda**\.

1. In the **Select your use case** section, choose **Lambda**, and then choose **Next: Permissions**\.

1. On the **Attach permissions policy** page, search for **AWSLambdaExecute**, and then choose the option beside it\. Search for **AutoScalingFullAccess**, and then choose the option beside it\.

1. Chooes **Next: Review**\.

1. On the **Review** page, verify that **AWSLambdaExecute** and **AutoScalingFullAccess** are listed under **Policies**\.  
![\[Paste the sample code into the lambda_function field\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/automation-asg-lamb-role.png)

1. Type a name in the **Role name** box, and then type a description\.

1. Choose **Create role**\. The system returns you to the **Roles** page\.

## Task 2: Create an AWS Lambda Function<a name="automation-asg2"></a>

Use the following procedure to create a Lambda function that automatically creates a new Auto Scaling group with the latest, patched AMI\.

**To create a Lambda function**

1. Sign in to the AWS Management Console and open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Choose **Create function**\.

1. Verify that **Author from scratch** is selected\.

1. In the **Name** field type Automation\-UpdateAsg\.

1. In the **Runtime** list, choose **Python 2\.7**\.

1. In the **Role** list, verify that **Choose an existing role** is selected\.

1. In the **Existing role** list, choose the role you created earlier\.

1. Choose **Create function**\. The systems displays a code and configuration page for Automation\-UpdateSAsg\.

1. Make no changes in the **Designer** section\.

1. In the **Function code** section, delete the pre\-populated code in the **lambda\_function** field, and then paste the following code sample\.  
![\[Paste the sample code into the lambda_function field\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/automation-asg-lamb.png)

   ```
   from __future__ import print_function
   
   import json
   import datetime
   import time
   import boto3
   
   print('Loading function')
   
   
   def lambda_handler(event, context):
       print("Received event: " + json.dumps(event, indent=2))
   
       # get autoscaling client
       client = boto3.client('autoscaling')
   
       # get object for the ASG we're going to update, filter by name of target ASG
       response = client.describe_auto_scaling_groups(AutoScalingGroupNames=[event['targetASG']])
   
       if not response['AutoScalingGroups']:
           return 'No such ASG'
   
       # get name of InstanceID in current ASG that we'll use to model new Launch Configuration after
       sourceInstanceId = response.get('AutoScalingGroups')[0]['Instances'][0]['InstanceId']
   
       # create LC using instance from target ASG as a template, only diff is the name of the new LC and new AMI
       timeStamp = time.time()
       timeStampString = datetime.datetime.fromtimestamp(timeStamp).strftime('%Y-%m-%d  %H-%M-%S')
       newLaunchConfigName = 'LC '+ event['newAmiID'] + ' ' + timeStampString
       client.create_launch_configuration(
           InstanceId = sourceInstanceId,
           LaunchConfigurationName=newLaunchConfigName,
           ImageId= event['newAmiID'] )
   
       # update ASG to use new LC
       response = client.update_auto_scaling_group(AutoScalingGroupName = event['targetASG'],LaunchConfigurationName = newLaunchConfigName)
   
       return 'Updated ASG `%s` with new launch configuration `%s` which includes AMI `%s`.' % (event['targetASG'], newLaunchConfigName, event['newAmiID'])
   ```

1. Specify the remaining configuration options on this page\.

1. Choose **Save**\.

1. Choose **Test**\.

1. In the **Configure test event** page, verify that **Create new test event** is selected\.

1. In the **Event template** list, verify that **Hello World** is selected\.

1. In the **Event name** field, type a name\.

1. Replace the existing sample with the following JSON\. Enter an AMI ID and Auto Scaling group\.

   ```
   {
      "newAmiID":"valid AMI ID",
      "targetASG":"name of your Auto Scaling group"
   }
   ```

1. Choose **Save**\.

1. Choose **Test**\. The output states that the Auto Scaling group was successfully updated with a new launch configuration\.

## Task 3: Create an Automation Document, Patch the AMI, and Update the Auto Scaling Group<a name="automation-asg3"></a>

Use the following procedure to create and run an Automation document that patches the AMI you specified for the **latestAmi** parameter\. The Automation workflow then updates the Auto Scaling group to use the latest, patched AMI\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To create and run the Automation document \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose **Create document**\.

1. In the **Name** field, type PatchAmiandUpdateAsg\.

1. In the **Document type** list, choose **Automation document**\.

1. Delete the brackets in the **Content** field, and then paste the following JSON sample document\.
**Note**  
You must change the values of *assumeRole* and *IamInstanceProfileName* in this sample with the service role ARN and instance profile role you created when [Setting Up Automation](automation-setup.md)\.

   ```
   {
      "description":"Systems Manager Automation Demo - Patch AMI and Update ASG",
      "schemaVersion":"0.3",
      "assumeRole":"the service role ARN you created",
      "parameters":{
         "sourceAMIid":{
            "type":"String",
            "description":"AMI to patch"
         },
         "targetAMIname":{
            "type":"String",
            "description":"Name of new AMI",
            "default":"patchedAMI-{{global:DATE_TIME}}"
         },
        "targetASG":{
            "type":"String",
            "description":"Autosaling group to Update"
         }
      },
      "mainSteps":[
         {
            "name":"startInstances",
            "action":"aws:runInstances",
            "timeoutSeconds":1200,
            "maxAttempts":1,
            "onFailure":"Abort",
            "inputs":{
               "ImageId":"{{ sourceAMIid }}",
               "InstanceType":"m3.large",
               "MinInstanceCount":1,
               "MaxInstanceCount":1,
               "IamInstanceProfileName":"the name of the instance IAM role you created"
            }
         },
         {
            "name":"installMissingWindowsUpdates",
            "action":"aws:runCommand",
            "maxAttempts":1,
            "onFailure":"Continue",
            "inputs":{
               "DocumentName":"AWS-InstallMissingWindowsUpdates",
               "InstanceIds":[
                  "{{ startInstances.InstanceIds }}"
               ],
               "Parameters":{
                  "UpdateLevel":"Important"
               }
            }
         },
         {
            "name":"stopInstance",
            "action":"aws:changeInstanceState",
            "maxAttempts":1,
            "onFailure":"Continue",
            "inputs":{
               "InstanceIds":[
                  "{{ startInstances.InstanceIds }}"
               ],
               "DesiredState":"stopped"
            }
         },
         {
            "name":"createImage",
            "action":"aws:createImage",
            "maxAttempts":1,
            "onFailure":"Continue",
            "inputs":{
               "InstanceId":"{{ startInstances.InstanceIds }}",
               "ImageName":"{{ targetAMIname }}",
               "NoReboot":true,
               "ImageDescription":"AMI created by EC2 Automation"
            }
         },
         {
            "name":"terminateInstance",
            "action":"aws:changeInstanceState",
            "maxAttempts":1,
            "onFailure":"Continue",
            "inputs":{
               "InstanceIds":[
                  "{{ startInstances.InstanceIds }}"
               ],
               "DesiredState":"terminated"
            }
         },
         {
            "name":"updateASG",
            "action":"aws:invokeLambdaFunction",
            "timeoutSeconds":1200,
            "maxAttempts":1,
            "onFailure":"Abort",
            "inputs": {
               "FunctionName": "Automation-UpdateAsg",
               "Payload": "{\"targetASG\":\"{{targetASG}}\", \"newAmiID\":\"{{createImage.ImageId}}\"}"
            }
         }
      ],
      "outputs":[
         "createImage.ImageId"
      ]
   }
   ```

1. Choose **Create document** to save the document\.

1. Choose **Automations**, and then choose **Execute automation**\.

1. In the **Automation document** list, choose **PatchAmiandUpdateAsg**\.

1. In the **Document details** section verify that **Document version** is set to **1**\.

1. In the **Execution mode** section, choose **Execute the entire automation at once**\.

1. Leave the **Targets and Rate Control** option disabled\.

1. Specify a Windows AMI ID for **sourceAMIid** and your Auto Scaling group name for **targetASG**\.

1. Choose **Execute automation**\.

1. After execution completes, in the Amazon EC2 console, choose **Auto Scaling**, and then choose **Launch Configurations**\. Verify that you see the new launch configuration, and that it uses the new AMI ID\.

1. Choose **Auto Scaling**, and then choose **Auto Scaling Groups**\. Verify that the Auto Scaling group uses the new launch configuration\.

1. Terminate one or more instances in your Auto Scaling group\. Replacement instances will be launched with the new AMI ID\.

**To create and run the Automation document \(Amazon EC2 Systems Manager\)**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Documents**\.

1. Choose **Create Document**\.

1. In the **Name** field, type PatchAmiandUpdateAsg\.

1. In the **Document Type** list, choose **Automation**\.

1. Delete the brackets in the **Content** field, and then paste the following JSON sample document\.
**Note**  
You must change the values of *assumeRole* and *IamInstanceProfileName* in this sample with the service role ARN and instance profile role you created when [Setting Up Automation](automation-setup.md)\.

   ```
   {
      "description":"Systems Manager Automation Demo - Patch AMI and Update ASG",
      "schemaVersion":"0.3",
      "assumeRole":"the service role ARN you created",
      "parameters":{
         "sourceAMIid":{
            "type":"String",
            "description":"AMI to patch"
         },
         "targetAMIname":{
            "type":"String",
            "description":"Name of new AMI",
            "default":"patchedAMI-{{global:DATE_TIME}}"
         },
        "targetASG":{
            "type":"String",
            "description":"Autosaling group to Update"
         }
      },
      "mainSteps":[
         {
            "name":"startInstances",
            "action":"aws:runInstances",
            "timeoutSeconds":1200,
            "maxAttempts":1,
            "onFailure":"Abort",
            "inputs":{
               "ImageId":"{{ sourceAMIid }}",
               "InstanceType":"m3.large",
               "MinInstanceCount":1,
               "MaxInstanceCount":1,
               "IamInstanceProfileName":"the name of the instance IAM role you created"
            }
         },
         {
            "name":"installMissingWindowsUpdates",
            "action":"aws:runCommand",
            "maxAttempts":1,
            "onFailure":"Continue",
            "inputs":{
               "DocumentName":"AWS-InstallMissingWindowsUpdates",
               "InstanceIds":[
                  "{{ startInstances.InstanceIds }}"
               ],
               "Parameters":{
                  "UpdateLevel":"Important"
               }
            }
         },
         {
            "name":"stopInstance",
            "action":"aws:changeInstanceState",
            "maxAttempts":1,
            "onFailure":"Continue",
            "inputs":{
               "InstanceIds":[
                  "{{ startInstances.InstanceIds }}"
               ],
               "DesiredState":"stopped"
            }
         },
         {
            "name":"createImage",
            "action":"aws:createImage",
            "maxAttempts":1,
            "onFailure":"Continue",
            "inputs":{
               "InstanceId":"{{ startInstances.InstanceIds }}",
               "ImageName":"{{ targetAMIname }}",
               "NoReboot":true,
               "ImageDescription":"AMI created by EC2 Automation"
            }
         },
         {
            "name":"terminateInstance",
            "action":"aws:changeInstanceState",
            "maxAttempts":1,
            "onFailure":"Continue",
            "inputs":{
               "InstanceIds":[
                  "{{ startInstances.InstanceIds }}"
               ],
               "DesiredState":"terminated"
            }
         },
         {
            "name":"updateASG",
            "action":"aws:invokeLambdaFunction",
            "timeoutSeconds":1200,
            "maxAttempts":1,
            "onFailure":"Abort",
            "inputs": {
               "FunctionName": "Automation-UpdateAsg",
               "Payload": "{\"targetASG\":\"{{targetASG}}\", \"newAmiID\":\"{{createImage.ImageId}}\"}"
            }
         }
      ],
      "outputs":[
         "createImage.ImageId"
      ]
   }
   ```

1. Choose **Create Document** to save the document\.

1. Expand **Systems Manager Services** in the navigation pane, choose **Automations**, and then choose **Run automation**\.

1. In the **Document name** list, choose **PatchAmiandUpdateAsg**\.

1. In the **Version** list, choose **1**, and then choose **Run automation**\.

1. Specify a Windows AMI ID for **sourceAMIid** and your Auto Scaling group name for **targetASG**\.

1. Choose **Run automation**\.

1. After execution completes, in the Amazon EC2 console, choose **Auto Scaling**, and then choose **Launch Configurations**\. Verify that you see the new launch configuration, and that it uses the new AMI ID\.

1. Choose **Auto Scaling**, and then choose **Auto Scaling Groups**\. Verify that the Auto Scaling group uses the new launch configuration\.

1. Terminate one or more instances in your Auto Scaling group\. Replacement instances will be launched with the new AMI ID\.

**Note**  
You can further automate deployment of the new AMI by editing the Lambda function to gracefully terminate instances\. You can also invoke your own Lambda function and utilize the ability of AWS CloudFormation to update Auto Scaling groups\. For more information, see [UpdatePolicy Attribute](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-attribute-updatepolicy.html)\.