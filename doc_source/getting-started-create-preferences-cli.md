# Create Session Manager preferences \(command line\)<a name="getting-started-create-preferences-cli"></a>

The following procedure describes how to use your preferred command line tool to create AWS Systems Manager Session Manager preferences for your AWS account in the selected AWS Region\. Use Session Manager preferences to specify options for logging session data in an Amazon Simple Storage Service \(Amazon S3\) bucket or Amazon CloudWatch Logs log group\. You can also use Session Manager preferences to encrypt your session data\.

For information about using command line tools to update existing Session Manager preferences, see [Update Session Manager preferences \(command line\)](getting-started-configure-preferences-cli.md)\.

For an example of how to create session preferences using AWS CloudFormation, see [Create a Systems Manager document for Session Manager preferences](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-ssm-document.html#aws-resource-ssm-document--examples) in the *AWS CloudFormation User Guide*\.

**Note**  
You can use this procedure to create custom Session documents for your Session Manager preferences that override account level settings\. When you create your custom Session documents, specify a value other than `SSM-SessionManagerRunShell` for the name parameter and modify the inputs as needed\. To use your custom Session documents, you must provide the name of your custom Session document for the `--document-name` parameter when starting a session from the AWS Command Line Interface \(AWS CLI\)\. When you start a session from the console, you can't specify custom Session documents\.

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
Add AWS KMS permissions for managed nodes in your account: [Step 2: Verify or create an IAM role with Session Managerpermissions](session-manager-getting-started-instance-profile.md)

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