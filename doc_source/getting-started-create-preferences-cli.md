# Create Session Manager Preferences \(AWS CLI\)<a name="getting-started-create-preferences-cli"></a>

The following procedure describes how to use the AWS CLI and the [create\-document](https://docs.aws.amazon.com/cli/latest/reference/ssm/create-document.html) command to create Session Manager preferences for your account in the selected AWS Region\. Use Session Manager preferences to specify options for logging session data in an Amazon S3 bucket or Amazon CloudWatch Logs log group\. You can also use Session Manager preferences to encrypt your session data\.

For information about using the CLI to update existing Session Manager preferences, see [Update Session Manager Preferences \(AWS CLI\)](getting-started-configure-preferences-cli.md)\.

**To create Session Manager preferences \(AWS CLI\)**

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

1. Specify where you want to send session data\. You can specify an S3 bucket name \(with an optional prefix\) or a CloudWatch Logs log group name\. For example:

   ```
   {
     "schemaVersion": "1.0",
     "description": "Document to hold regional settings for Session Manager",
     "sessionType": "Standard_Stream",
     "inputs": {
       "s3BucketName": "MyBucketName",
       "s3KeyPrefix": "MyBucketPrefix",
       "s3EncryptionEnabled": true,
       "cloudWatchLogGroupName": "MyLogGroupName",
       "cloudWatchEncryptionEnabled": true
     }
   }
   ```
**Note**  
If you don't want to encrypt log data, change "true" to "false"\.  
If you aren't sending logs to an S3 bucket or a CloudWatch Logs log group, you can delete the lines for those options\. Make sure the last line in the "inputs" section does not end with a comma\.

1. Save the file\.

1. In the directory where you created the JSON file, run the following command:

   ```
   aws ssm create-document --name SSM-SessionManagerRunShell --content "file://SessionManagerRunShell.json" --document-type "Session" --document-format JSON
   ```
**Important**  
Be sure to include `file://` before the file name\. It is required in this command\.

   If successful, the command returns output similar to the following:

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