# Walkthrough: Create an Automation Document<a name="automation-createdoc"></a>

This walkthrough shows you how to create and run a custom Automation document\. After you run Automation, the system performs the following tasks\.
+ Launches a Windows instance from a specified AMI\.
+ Runs a command using Run Command that applies Windows updates to the instance\.
+ Stops the instance\.
+ Creates a new Windows AMI\.
+ Tag the Windows AMI\.
+ Terminates the original instance\.

**Automation Sample Document**  
Automation runs Systems Manager automation documents written in JSON or YAML\. Automation documents include the actions to be performed during workflow execution\. For more information about Systems Manager documents, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\. For information about actions you can add to a document, see [Systems Manager Automation Document Reference](automation-actions.md)

**Note**  
By default, when Automation runs the AWS\-UpdateWindowsAmi document and creates a temporary instance, the system uses the default VPC \(172\.30\.0\.0/16\)\. If you deleted the default VPC, you will receive the following error:  
VPC not defined 400  
To solve this problem, you must make a copy of the AWS\-UpdateWindowsAmi document and specify a subnet ID\. For more information, see [VPC not defined 400](automation-troubleshooting.md#automation-trbl-common-vpc)\.

**To create a patched AMI using Automation**

1. Collect the following information\. You will specify this information later in this procedure\.
   + The source ID of the AMI to update\.
   + Create an AWS Identity and Access Management \(IAM\) instance profile role and Automation service role \(or assume role\)\. For more information about these roles and how to quickly create them from an AWS CloudFormation template, see [Method 1: Use AWS CloudFormation to Configure Roles for Automation](automation-cf.md)\. Be sure to copy the name of the instance profile role and the Amazon Resource Name \(ARN\) of the Automation service role, as described in [Copy Role Information for Automation](automation-cf.md#automation-cf-copy)\.

1. Copy the following example document into a text editor such as Notepad\. Change the value of `assumeRole` to the role ARN you created earlier when you created an IAM role for Automation and change the value of `IamInstanceProfileName` to the name of the role you created earlier\. Save the document on a local drive as patchWindowsAmi\.json or patchWindowsAmi\.yaml\.

   ```
   {
      "description":"Systems Manager Automation Demo - Patch and Create a New AMI",
      "schemaVersion":"0.3",
      "assumeRole":"the role ARN you created",
      "parameters":{
         "sourceAMIid":{
            "type":"String",
            "description":"AMI to patch"
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
            "name":"createTags",
            "action":"aws:createTags",
            "maxAttempts":1,
            "onFailure":"Continue",
            "inputs":{
               "ResourceType":"EC2",
               "ResourceIds":[
                  "{{createImage.ImageId}}"
               ],
               "Tags":[
                  {
                     "Key": "Generated By Automation",
                     "Value": "{{automation:EXECUTION_ID}}"
                  },
                  {
                     "Key": "From Source AMI",
                     "Value": "{{sourceAMIid}}"
                  }
               ]
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
         }
      ],
      "outputs":[
         "createImage.ImageId"
      ]
   }
   ```

1. [Download](https://aws.amazon.com/cli/) the AWS CLI to your local machine\.

1. Edit the following command, and specify the path to the patchWindowsAmi\.json/yaml file on your local machine\. Run the command to create the required Automation document\.
**Note**  
For the `name` parameter, you can't prefix documents with AWS\. If you specify AWS\-*name* or AWS*name*, you will receive an error\.

   ```
   aws ssm create-document --name "patchWindowsAmi" --content file:///Users/test-user/Documents/patchWindowsAmi.json/yaml --document-type Automation
   ```

   The system returns information about the command progress\.

   ```
   {
       "DocumentDescription": {
           "Status": "Creating",
           "Hash": "bce98f80b89668b092cd094d2f2895f57e40942bcc1598d85338dc9516b0b7f1",
           "Name": "test",
           "Parameters": [
               {
                   "Type": "String",
                   "Name": "sourceAMIid",
                   "Description": "AMI to patch"
               },
               {
                   "DefaultValue": "patchedAMI-{{global:DATE_TIME}}",
                   "Type": "String",
                   "Name": "targetAMIname",
                   "Description": "Name of new AMI"
               }
           ],
           "DocumentType": "Automation",
           "PlatformTypes": [
               "Windows",
               "Linux"
           ],
           "DocumentVersion": "1",
           "HashType": "Sha256",
           "CreatedDate": 1488303738.572,
           "Owner": "12345678901",
           "SchemaVersion": "0.3",
           "DefaultVersion": "1",
           "LatestVersion": "1",
           "Description": "Systems Manager Automation Demo - Patch and Create a New AMI"
       }
   }
   ```

1. Run the following command to view a list of documents that you can access\.

   ```
   aws ssm list-documents --document-filter-list key=Owner,value=Self
   ```

   The system returns information like the following:

   ```
   {
      "DocumentIdentifiers":[
         {
            "Name":" patchWindowsAmi",
            "PlatformTypes": [
   
            ],
            "DocumentVersion": "5",
            "DocumentType": "Automation",
            "Owner": "12345678901",
            "SchemaVersion": "0.3"
         }
      ]
   }
   ```

1. Run the following command to view details about the patchWindowsAmi document\.

   ```
   aws ssm describe-document --name patchWindowsAmi
   ```

   The system returns information like the following:

   ```
   {
      "Document": {
         "Status": "Active",
         "Hash": "99d5b2e33571a6bb52c629283bca0a164026cd201876adf0a76de16766fb98ac",
         "Name": "patchWindowsAmi",
         "Parameters": [
            {
               "DefaultValue": "ami-3f0c4628",
               "Type": "String",
               "Name": "sourceAMIid",
               "Description": "AMI to patch"
            },
            {
               "DefaultValue": "patchedAMI-{{global:DATE_TIME}}",
               "Type": "String",
               "Name": "targetAMIname",
               "Description": "Name of new AMI"
            }
         ],
         "DocumentType": "Automation",
         "PlatformTypes": [
   
         ],
         "DocumentVersion": "5",
         "HashType": "Sha256",
         "CreatedDate": 1478904417.477,
         "Owner": "12345678901",
         "SchemaVersion": "0.3",
         "DefaultVersion": "5",
         "LatestVersion": "5",
         "Description": "Automation Demo - Patch and Create a New AMI"
      }
   }
   ```

1. Run the following command to run the patchWindowsAmi document and run the Automation workflow\. This command takes two input parameters: the ID of the AMI to be patched, and the name of the new AMI\. The example command below uses a recent EC2 AMI to minimize the number of patches that need to be applied\. If you run this command more than once, you must specify a unique value for `targetAMIname`\. AMI names must be unique\.

   ```
   aws ssm start-automation-execution --document-name="patchWindowsAmi" --parameters sourceAMIid="ami-bd3ba0aa"
   ```

   The command returns an execution ID\. Copy this ID to the clipboard\. You will use this ID to view the status of the workflow\.

   ```
   {
       "AutomationExecutionId": "ID"
   }
   ```

   You can monitor the status of the workflow in the console\. Check the console to verify that a new instance is launching\. After the instance launch is complete, you can confirm that the Run Command action was run\. After Run Command execution is complete, you should see a new AMI in your list of AMI images\.

1. To view the workflow execution using the CLI, run the following command:

   ```
   aws ssm describe-automation-executions
   ```

1. To view details about the execution progress, run the following command\.

   ```
   aws ssm get-automation-execution --automation-execution-id ID
   ```

**Note**  
Depending on the number of patches applied, the Windows patching process run in this sample workflow can take 30 minutes or more to complete\.

For more examples of how to use Automation, including examples that build on the walkthrough you just completed, see [Systems Manager Automation Examples](automation-examples.md)\.