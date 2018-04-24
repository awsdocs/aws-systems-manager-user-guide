# Simplify AMI Patching Using Automation, Lambda, and Parameter Store<a name="automation-simpatch"></a>

The following example expands on how to update a Windows AMI, as described in [Walkthrough: Create an Automation Document](automation-createdoc.md)\. This example uses the model where an organization maintains and periodically patches their own, proprietary AMIs rather than building from Amazon EC2 AMIs\.

The following procedure shows how to automatically apply operating system \(OS\) patches to a Windows AMI that is already considered to be the most up\-to\-date or *latest* AMI\. In the example, the default value of the parameter `SourceAmiId` is defined by a Systems Manager Parameter Store parameter called `latestAmi`\. The value of `latestAmi` is updated by an AWS Lambda function invoked at the end of the Automation workflow\. As a result of this Automation process, the time and effort spent patching AMIs is minimized because patching is always applied to the most up\-to\-date AMI\.

**Before You Begin**  
Configure Automation roles and, optionally, CloudWatch Events for Automation\. For more information, see [Setting Up Automation](automation-setup.md)\.

**Topics**
+ [Task 1: Create a Parameter in Systems Manager Parameter Store](#automation-pet1)
+ [Task 2: Create an IAM Role for AWS Lambda](#automation-pet2)
+ [Task 3: Create an AWS Lambda Function](#automation-pet3)
+ [Task 4: Create an Automation Document and Patch the AMI](#automation-pet4)

## Task 1: Create a Parameter in Systems Manager Parameter Store<a name="automation-pet1"></a>

Create a string parameter in Parameter Store that uses the following information:
+ **Name**: latestAmi\.
+ **Value**: a Windows AMI ID\. For example: ami\-188d6e0e\.

For information about how to create a Parameter Store string parameter, see [Creating Systems Manager Parameters](sysman-paramstore-su-create.md)\.

## Task 2: Create an IAM Role for AWS Lambda<a name="automation-pet2"></a>

Use the following procedure to create an IAM service role for AWS Lambda\. This role includes the **AWSLambdaExecute** and **AmazonSSMFullAccess** managed policies\. These policies give Lambda permission to update the value of the `latestAmi` parameter using a Lambda function and Systems Manager\.

**To create an IAM service role for Lambda**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create New Role**\.

1. For **Role name**, type a role name that can help you identify the purpose of this role, for example, lambda\-ssm\-role\. Role names must be unique within your AWS account\. After you type the name, choose **Next Step** at the bottom of the page\.
**Note**  
Because various entities might reference the role, you cannot change the name of the role after it has been created\.

1. On the **Select Role Type** page, choose the **AWS Service Roles** section, and then choose **AWS Lambda**\.

1. On the **Attach Policy** page, choose **AWSLambdaExecute** and **AmazonSSMFullAccess**, and then choose **Next Step**\.

1. Choose **Create Role**\.

## Task 3: Create an AWS Lambda Function<a name="automation-pet3"></a>

Use the following procedure to create a Lambda function that automatically updates the value of the `latestAmi` parameter\.

**To create a Lambda function**

1. Sign in to the AWS Management Console and open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Choose **Create a Lambda function**\.

1. On the **Select blueprint** page, choose **Blank Function**\.

1. On the **Configure triggers** page, choose **Next**\.

1. On the **Configure function** page, type Automation\-UpdateSsmParam in the **Name** field, and enter a description, if you want\.

1. In the **Runtime** list, choose **Python 2\.7**\.

1. In the **Lambda function code** section, delete the pre\-populated code in the field, and then paste the following code sample\.

   ```
   from __future__ import print_function
   
   import json
   import boto3
   
   print('Loading function')
   
   
   #Updates an SSM parameter
   #Expects parameterName, parameterValue
   def lambda_handler(event, context):
       print("Received event: " + json.dumps(event, indent=2))
   
       # get SSM client
       client = boto3.client('ssm')
   
       #confirm  parameter exists before updating it
       response = client.describe_parameters(
          Filters=[
             {
              'Key': 'Name',
              'Values': [ event['parameterName'] ]
             },
           ]
       )
   
       if not response['Parameters']:
           print('No such parameter')
           return 'SSM parameter not found.'
   
       #if parameter has a Descrition field, update it PLUS the Value
       if 'Description' in response['Parameters'][0]:
           description = response['Parameters'][0]['Description']
           
           response = client.put_parameter(
             Name=event['parameterName'],
             Value=event['parameterValue'],
             Description=description,
             Type='String',
             Overwrite=True
           )
       
       #otherwise just update Value
       else:
           response = client.put_parameter(
             Name=event['parameterName'],
             Value=event['parameterValue'],
             Type='String',
             Overwrite=True
           )
           
       reponseString = 'Updated parameter %s with value %s.' % (event['parameterName'], event['parameterValue'])
           
       return reponseString
   ```

1. In the **Lambda function handler and role** section, in the **Role** list, choose the service role for Lambda that you created in Task 2\.

1. Choose **Next**, and then choose **Create function**\.

1. To test the Lambda function, from the **Actions** menu, choose **Configure Test Event**\.

1. Replace the existing text with the following JSON\.

   ```
   {
      "parameterName":"latestAmi",
      "parameterValue":"your AMI ID"
   }
   ```

1. Choose **Save and test**\. The output should state that the parameter was successfully updated and include details about the update\. For example, “Updated parameter latestAmi with value ami\-123456”\.

## Task 4: Create an Automation Document and Patch the AMI<a name="automation-pet4"></a>

Use the following procedure to create and run an Automation document that patches the AMI you specified for the **latestAmi** parameter\. After the Automation workflow completes, the value of **latestAmi** is updated with the ID of the newly\-patched AMI\. Subsequent executions use the AMI created by the previous execution\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To create an Automation document and patch an AMI \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose **Create document**\.

1. In the **Name** field, type UpdateMyLatestWindowsAmi\.

1. In the **Document type** list, choose **Automation document**\.

1. Delete the brackets in the **Content** field, and then paste the following JSON sample document\.
**Note**  
You must change the values of *assumeRole* and *IamInstanceProfileName* in this sample with the service role ARN and instance profile role you created when [Setting Up Automation](automation-setup.md)\.

   ```
   {
      "description":"Systems Manager Automation Demo – Patch AMI and Update SSM Param",
      "schemaVersion":"0.3",
      "assumeRole":"the role ARN you created",
      "parameters":{
         "sourceAMIid":{
            "type":"String",
            "description":"AMI to patch",
            "default":"{{ssm:latestAmi}}"
         },
         "targetAMIname":{
            "type":"String",
            "description":"Name of new AMI",
            "default":"patchedAMI-{{global:DATE_TIME}}"
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
               "IamInstanceProfileName":"the name of the IAM role you created"
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
            "name":"updateSsmParam",
            "action":"aws:invokeLambdaFunction",
            "timeoutSeconds":1200,
            "maxAttempts":1,
            "onFailure":"Abort",
            "inputs":{
               "FunctionName":"Automation-UpdateSsmParam",
               "Payload":"{\"parameterName\":\"latestAmi\", \"parameterValue\":\"{{createImage.ImageId}}\"}"
            }
         }
      ],
      "outputs":[
         "createImage.ImageId"
      ]
   }
   ```

1. Choose **Create document** to save the document\.

1. In the navigation pane, choose **Automations**, and then choose **Execute automation**\.

1. In the **Automation document** list, choose **UpdateMyLatestWindowsAmi**\.

1. In the **Document details** section verify that **Document version** is set to **1**\.

1. In the **Execution mode** section, choose **Execute the entire automation at once**\.

1. Leave the **Targets and Rate Control** option disabled\.

1. After execution completes, choose **Parameter Store** in the navigation pane and confirm that the new value for latestAmi matches the value returned by the Automation workflow\. You can also verify the new AMI ID matches the Automation output in the **AMIs** section of the EC2 console\.

**To create an Automation document and patch an AMI \(Amazon EC2 Systems Manager\)**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Documents**\.

1. Choose **Create Document**\.

1. In the **Name** field, type UpdateMyLatestWindowsAmi\.

1. In the **Document Type** list, choose **Automation**\.

1. Delete the brackets in the **Content** field, and then paste the following JSON sample document\.
**Note**  
You must change the values of *assumeRole* and *IamInstanceProfileName* in this sample with the service role ARN and instance profile role you created when [Setting Up Automation](automation-setup.md)\.

   ```
   {
      "description":"Systems Manager Automation Demo – Patch AMI and Update SSM Param",
      "schemaVersion":"0.3",
      "assumeRole":"the role ARN you created",
      "parameters":{
         "sourceAMIid":{
            "type":"String",
            "description":"AMI to patch",
            "default":"{{ssm:latestAmi}}"
         },
         "targetAMIname":{
            "type":"String",
            "description":"Name of new AMI",
            "default":"patchedAMI-{{global:DATE_TIME}}"
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
               "IamInstanceProfileName":"the name of the IAM role you created"
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
            "name":"updateSsmParam",
            "action":"aws:invokeLambdaFunction",
            "timeoutSeconds":1200,
            "maxAttempts":1,
            "onFailure":"Abort",
            "inputs":{
               "FunctionName":"Automation-UpdateSsmParam",
               "Payload":"{\"parameterName\":\"latestAmi\", \"parameterValue\":\"{{createImage.ImageId}}\"}"
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

1. In the **Document name** list, choose **UpdateMyLatestWindowsAmi**\.

1. In the **Version** list, choose **1**, and then choose **Run automation**\.

1. After execution completes, in the Amazon EC2 console, choose **Parameter Store** and confirm that the new value for latestAmi matches the value returned by the Automation workflow\. You can also verify the new AMI ID matches the Automation output in the **AMIs** section of the EC2 console\.