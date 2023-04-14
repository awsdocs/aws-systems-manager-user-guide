# Create a Session Manager preferences document \(command line\)<a name="getting-started-create-preferences-cli"></a>

Use the following procedure to create SSM documents that define your preferences for AWS Systems Manager Session Manager sessions\. You can use the document to configure session options including data encryption, session duration, and logging\. For example, you can specify whether to store session log data in an Amazon Simple Storage Service \(Amazon S3\) bucket or Amazon CloudWatch Logs log group\. You can create documents that define general preferences for all sessions for an AWS account and AWS Region, or that define preferences for individual sessions\. 

**Note**  
You can also configure general session preferences by using the Session Manager console\.

Documents used to set Session Manager preferences must have a `sessionType` of `Standard_Stream`\. For more information about Session documents, see [Session document schema](session-manager-schema.md)\.

For information about using the command line to update existing Session Manager preferences, see [Update Session Manager preferences \(command line\)](getting-started-configure-preferences-cli.md)\.

For an example of how to create session preferences using AWS CloudFormation, see [Create a Systems Manager document for Session Manager preferences](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ssm-document.html#aws-resource-ssm-document--examples) in the *AWS CloudFormation User Guide*\.

**Note**  
This procedure describes how to create documents for setting Session Manager preferences at the AWS account level\. To create documents that will be used for setting session\-level preferences, specify a value other than `SSM-SessionManagerRunShell` for the file name related command inputs \.   
To use your document to set preferences for sessions started from the AWS Command Line Interface \(AWS CLI\), provide the document name as the `--document-name` parameter value\. To set preferences for sessions started from the Session Manager console, you can type or select the name of your document from a list\.

**To create Session Manager preferences \(command line\)**

1. Create a JSON file on your local machine with a name such as `SessionManagerRunShell.json`, and then paste the following content into it\.

   ```
   {
       "schemaVersion": "1.0",
       "description": "Document to hold regional settings for Session Manager",
       "sessionType": "Standard_Stream",
       "inputs": {
           "s3BucketName": "",
           "s3KeyPrefix": "",
           "s3EncryptionEnabled": true,
           "cloudWatchLogGroupName": "",
           "cloudWatchEncryptionEnabled": true,
           "cloudWatchStreamingEnabled": false,
           "kmsKeyId": "",
           "runAsEnabled": false,
           "runAsDefaultUser": "",
           "idleSessionTimeout": "",
           "maxSessionDuration": "",
           "shellProfile": {
               "windows": "date",
               "linux": "pwd;ls"
           }
       }
   }
   ```

   You can also pass values to your session preferences using parameters instead of hardcoding the values as shown in the following example\.

   ```
   {
      "schemaVersion":"1.0",
      "description":"Session Document Parameter Example JSON Template",
      "sessionType":"Standard_Stream",
      "parameters":{
         "s3BucketName":{
            "type":"String",
            "default":""
         },
         "s3KeyPrefix":{
            "type":"String",
            "default":""
         },
         "s3EncryptionEnabled":{
            "type":"Boolean",
            "default":"false"
         },
         "cloudWatchLogGroupName":{
            "type":"String",
            "default":""
         },
         "cloudWatchEncryptionEnabled":{
            "type":"Boolean",
            "default":"false"
         }
      },
      "inputs":{
         "s3BucketName":"{{s3BucketName}}",
         "s3KeyPrefix":"{{s3KeyPrefix}}",
         "s3EncryptionEnabled":"{{s3EncryptionEnabled}}",
         "cloudWatchLogGroupName":"{{cloudWatchLogGroupName}}",
         "cloudWatchEncryptionEnabled":"{{cloudWatchEncryptionEnabled}}",
         "kmsKeyId":""
      }
   }
   ```

1. Specify where you want to send session data\. You can specify an S3 bucket name \(with an optional prefix\) or a CloudWatch Logs log group name\. If you want to further encrypt data between local client and managed nodes, provide the KMS key to use for encryption\. The following is an example\.

   ```
   {
     "schemaVersion": "1.0",
     "description": "Document to hold regional settings for Session Manager",
     "sessionType": "Standard_Stream",
     "inputs": {
       "s3BucketName": "DOC-EXAMPLE-BUCKET",
       "s3KeyPrefix": "MyBucketPrefix",
       "s3EncryptionEnabled": true,
       "cloudWatchLogGroupName": "MyLogGroupName",
       "cloudWatchEncryptionEnabled": true,
       "cloudWatchStreamingEnabled": false,
       "kmsKeyId": "MyKMSKeyID",
       "runAsEnabled": true,
       "runAsDefaultUser": "MyDefaultRunAsUser",
       "idleSessionTimeout": "20",
       "maxSessionDuration": "60",
       "shellProfile": {
           "windows": "MyCommands",
           "linux": "MyCommands"
       }
     }
   }
   ```
**Note**  
If you don't want to encrypt the session log data, change `true` to `false` for `s3EncryptionEnabled`\.  
If you aren't sending logs to either an Amazon S3 bucket or a CloudWatch Logs log group, don't want to encrypt active session data, or don't want to turn on Run As support for the sessions in your account, you can delete the lines for those options\. Make sure the last line in the `inputs` section doesn't end with a comma\.  
If you add a KMS key ID to encrypt your session data, both the users who start sessions and the managed nodes that they connect to must have permission to use the key\. You provide permission to use the KMS key with Session Manager through IAM policies\. For information, see the following topics:  
Add AWS KMS permissions for users in your account: [Quickstart default IAM policies for Session Manager](getting-started-restrict-access-quickstart.md)
Add AWS KMS permissions for managed nodes in your account: [Step 2: Verify or create an IAM role with Session Manager permissions](session-manager-getting-started-instance-profile.md)

1. Save the file\.

1. In the directory where you created the JSON file, run the following command\.

------
#### [ Linux & macOS ]

   ```
   aws ssm create-document \
       --name SSM-SessionManagerRunShell \
       --content "file://SessionManagerRunShell.json" \
       --document-type "Session" \
       --document-format JSON
   ```

------
#### [ Windows ]

   ```
   aws ssm create-document ^
       --name SSM-SessionManagerRunShell ^
       --content "file://SessionManagerRunShell.json" ^
       --document-type "Session" ^
       --document-format JSON
   ```

------
#### [ PowerShell ]

   ```
   New-SSMDocument `
       -Name "SSM-SessionManagerRunShell" `
       -Content (Get-Content -Raw SessionManagerRunShell.json) `
       -DocumentType "Session" `
       -DocumentFormat JSON
   ```

------

   If successful, the command returns output similar to the following\.

   ```
   {
       "DocumentDescription": {
           "Status": "Creating",
           "Hash": "ce4fd0a2ab9b0fae759004ba603174c3ec2231f21a81db8690a33eb66EXAMPLE",
           "Name": "SSM-SessionManagerRunShell",
           "Tags": [],
           "DocumentType": "Session",
           "PlatformTypes": [
               "Windows",
               "Linux"
           ],
           "DocumentVersion": "1",
           "HashType": "Sha256",
           "CreatedDate": 1547750660.918,
           "Owner": "111122223333",
           "SchemaVersion": "1.0",
           "DefaultVersion": "1",
           "DocumentFormat": "JSON",
           "LatestVersion": "1"
       }
   }
   ```