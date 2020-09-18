# Walkthrough: Simplify AMI patching using Automation, AWS Lambda, and Parameter Store<a name="automation-walk-patch-windows-ami-simplify"></a>

The following example expands on how to update a Windows AMI, as described in [Walkthrough: Patch a Windows Server AMI](automation-walk-patch-windows-ami-cli.md)\. This example uses the model where an organization maintains and periodically patches their own, proprietary AMIs rather than building from Amazon EC2 AMIs\.

The following procedure shows how to automatically apply operating system \(OS\) patches to a Windows AMI that is already considered to be the most up\-to\-date or *latest* AMI\. In the example, the default value of the parameter `SourceAmiId` is defined by a Systems Manager Parameter Store parameter called `latestAmi`\. The value of `latestAmi` is updated by an AWS Lambda function invoked at the end of the Automation workflow\. As a result of this Automation process, the time and effort spent patching AMIs is minimized because patching is always applied to the most up\-to\-date AMI\.

**Before You Begin**  
Configure Automation roles and, optionally, EventBridge for Automation\. For more information, see [Getting started with Automation](automation-setup.md)\.

**Topics**
+ [Task 1: Create a parameter in Systems Manager Parameter Store](#automation-pet1)
+ [Task 2: Create an IAM role for AWS Lambda](#automation-pet2)
+ [Task 3: Create an AWS Lambda function](#automation-pet3)
+ [Task 4: Create an Automation document and patch the AMI](#automation-pet4)

## Task 1: Create a parameter in Systems Manager Parameter Store<a name="automation-pet1"></a>

Create a string parameter in Parameter Store that uses the following information:
+ **Name**: `latestAmi`\.
+ **Value**: a Windows AMI ID\. For example:` ami-188d6e0e`\.

For information about how to create a Parameter Store string parameter, see [Creating Systems Manager parameters](sysman-paramstore-su-create.md)\.

## Task 2: Create an IAM role for AWS Lambda<a name="automation-pet2"></a>

Use the following procedure to create an IAM service role for AWS Lambda\. This role includes the `AWSLambdaExecute` and `AmazonSSMFullAccess` managed policies\. These policies give Lambda permission to update the value of the `latestAmi` parameter using a Lambda function and Systems Manager\.

**To create an IAM service role for Lambda**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\.

1. Choose the **JSON** tab\.

1. Replace the default content with the following\. Be sure to replace *us\-west\-2* and *123456789012* with the Region and account you want to use\. Replace *updateAmiFunction* with the name of your Lambda function\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "logs:CreateLogGroup",
               "Resource": "arn:aws:logs:us-west-2:123456789012:*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "logs:CreateLogStream",
                   "logs:PutLogEvents"
               ],
               "Resource": [
                   "arn:aws:logs:us-west-2:123456789012:log-group:/aws/lambda/updateAmiFunction:*"
               ]
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter a name for the inline policy, such as **amiLambda**\.

1. Choose **Create policy**\.

1. Repeat steps 2 and 3\.

1. Replace the default content with the following\. Be sure to replace *us\-west\-2* and *123456789012* with the Region and account you want to use\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "ssm:PutParameter",
               "Resource": "arn:aws:ssm:us-west-2:123456789012:parameter/latestAmi"
           },
           {
               "Effect": "Allow",
               "Action": "ssm:DescribeParameters",
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter a name for the inline policy, such as **amiParameter**\.

1. Choose **Create policy**\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. Immediately under **Choose the service that will use this role**, choose **Lambda**, and then choose **Next: Permissions**\.

1. On the **Attach permissions policies** page, use the **Search** field to locate the two policies you created earlier\.

1. Select the check box next to the policies, and then choose **Next: Tags**\.

1. \(Optional\) Add one or more tag key\-value pairs to organize, track, or control access for this role, and then choose **Next: Review**\. 

1. For **Role name**, enter a name for your new role, such as **lambda\-ssm\-role** or another name that you prefer\. 
**Note**  
Because various entities might reference the role, you cannot change the name of the role after it has been created\.

1. Choose **Create role**\.

## Task 3: Create an AWS Lambda function<a name="automation-pet3"></a>

Use the following procedure to create a Lambda function that automatically updates the value of the `latestAmi` parameter\.

**To create a Lambda function**

1. Sign in to the AWS Management Console and open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Choose **Create function**\.

1. On the **Create function** page, choose **Author from scratch**\.

1. For **Function name**, type **Automation\-UpdateSsmParam**\.

1. In the **Runtime** list, choose **Python 3\.8**\.

1. In the **Permissions** section, expand **Choose or create an execution role**\.

1. Choose **Use an existing role**, and then choose the service role for Lambda that you created in Task 2\.

1. Choose **Create function**\.

1. In the **Function code** area, on the **lambda\_function** tab, delete the pre\-populated code in the field, and then paste the following code sample\.

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
   
       #if parameter has a Description field, update it PLUS the Value
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

1. Choose **Save**\.

1. To test the Lambda function, from the **Select a test event** menu, choose **Configure test events**\.

1. For **Event name**, enter a name for the test event, such as **MyTestEvent**\.

1. Replace the existing text with the following JSON, replacing *your\-ami\-id* with the ID of the new AMI to set as your `latestAmi` parameter value\.

   ```
   {
      "parameterName":"latestAmi",
      "parameterValue":"your-ami-id"
   }
   ```

1. Choose **Create**\.

1. Choose **Test** to test the function\. The output should state that the parameter was successfully updated and include details about the update\. For example, “Updated parameter latestAmi with value ami\-123456”\.

## Task 4: Create an Automation document and patch the AMI<a name="automation-pet4"></a>

Use the following procedure to create and run an Automation document that patches the AMI you specified for the **latestAmi** parameter\. After the Automation workflow completes, the value of **latestAmi** is updated with the ID of the newly\-patched AMI\. Subsequent executions use the AMI created by the previous execution\.

**To create an Automation document and patch an AMI**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose **Create automation**\.

1. In the **Name** field, type **UpdateMyLatestWindowsAmi**\.

1. Choose the **Editor** tab, and then choose **Edit**\.

1. Replace the default contents in the **Document editor** field with following JSON sample document\.
**Note**  
You must change the values of *assumeRole* and *IamInstanceProfileName* in this sample with the service role ARN and instance profile role you created when [Getting started with Automation](automation-setup.md)\.

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
               "DocumentName":"AWS-InstallWindowsUpdates",
               "InstanceIds":[
                  "{{ startInstances.InstanceIds }}"
               ],
               "Parameters":{
                  "SeverityLevels":"Important"
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

1. Choose **Create automation** to save the document\.

1. In the navigation pane, choose **Automation**, and then choose **Execute automation**\.

1. In the **Choose document** page, choose the **Owned by me** tab, and then select the button in the **UpdateMyLatestWindowsAmi** card\.

1. In the **Document details** section, verify that **Document version** is set to **1 \(Default\)**\.

1. Choose **Next**\.

1. Choose **Simple execution**\.

1. Choose **Execute**\.

1. After execution completes, choose **Parameter Store** in the navigation pane and confirm that the new value for `latestAmi` matches the value returned by the Automation workflow\. You can also verify the new AMI ID matches the Automation output in the **AMIs** section of the Amazon EC2 console\.