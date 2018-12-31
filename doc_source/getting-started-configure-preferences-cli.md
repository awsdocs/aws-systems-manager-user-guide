# Use the AWS CLI to Update Session Manager Preferences<a name="getting-started-configure-preferences-cli"></a>

Follow these steps to use the AWS CLI and the [update\-document](https://docs.aws.amazon.com/cli/latest/reference/ssm/update-document.html) command to specify or change Session Manager preferences for your account in the selected AWS Region, such as the S3 bucket or CloudWatch log group that session data is sent to\. 

Note: The SSM-SessionManagerRunShell document does not exist by default. In order to generate the document either use the create-document CLI command or access Session Manager preferences in the AWS Console. 

1. Create a JSON file on your local machine with a name such as `SessionManagerRunShell.json`, and then paste the following content into it:

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
           "cloudWatchEncryptionEnabled": true
       }
   }
   ```

1. Specify an S3 bucket name and optional prefix, or a CloudWatch log group name to send session data to\. For example:

   ```
   {
     "schemaVersion": "1.0",
     "description": "Document to hold regional settings for Session Manager",
     "sessionType": "Standard_Stream",
     "inputs": {
       "s3BucketName": "MyBucketName",
       "s3KeyPrefix": "MyBucketPrefix",
       "s3EncryptionEnabled": true,
       "cloudWatchLogGroupName": "MyLogGroupName,
       "cloudWatchEncryptionEnabled": true"
     }
   }
   ```
**Note**  
If you do not want to encrypt the log data, change "true" to "false"\.  
If you are not sending logs to either an S3 bucket or a CloudWatch log group, you can delete the lines for those options\. Make sure the last line in the "inputs" section does not end with a comma\.

1. Save the file\.

1. In the directory where you created the JSON file, run the following command:

   ```
   aws ssm update-document --name "SSM-SessionManagerRunShell" --content "file://SessionManagerRunShell.json" --document-version "\$LATEST"
   ```
**Important**  
Be sure to include `file://` before the file name\. It is required in this command\.

   If successful, the command returns output similar to the following:

   ```
   {
       "DocumentDescription": {
           "Status": "Updating",
           "Hash": "ce4fd0a2ab9b0fae759004ba603174c3ec2231f21a81db8690a33eb66EXAMPLE",
           "Name": "SSM-SessionManagerRunShell",
           "Tags": [],
           "DocumentType": "Session",
           "PlatformTypes": [
               "Windows",
               "Linux"
           ],
           "DocumentVersion": "2",
           "HashType": "Sha256",
           "CreatedDate": 1537206341.565,
           "Owner": "111122223333",
           "SchemaVersion": "1.0",
           "DefaultVersion": "1",
           "DocumentFormat": "JSON",
           "LatestVersion": "2"
       }
   }
   ```
