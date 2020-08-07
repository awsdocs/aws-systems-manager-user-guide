# Running commands using the document version parameter<a name="run-command-version"></a>

You can use the document version parameter to specify which version of an SSM document to use when the command runs\. You can specify one of the following options for this parameter:
+ $DEFAULT
+ $LATEST
+ Version number

Use the following procedure to run a command using the document version parameter\. 

------
#### [ Linux ]

**To run commands using the AWS CLI on local Linux machines**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. List all available documents

   This command lists all of the documents available for your account based on IAM permissions\.

   ```
   aws ssm list-documents
   ```

1. Use the following command to view the different versions of a document\.

   ```
   aws ssm list-document-versions \
       --name "document-name"
   ```

1. Use the following command to run a command that uses an SSM document version\.

   ```
   aws ssm send-command \
       --document-name "AWS-RunShellScript" \
       --parameters commands="echo Hello" \
       --instance-ids instance-ID \
       --document-version '$LATEST'
   ```

------
#### [ Windows ]

**To run commands using the AWS CLI on local Windows machines**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. List all available documents

   This command lists all of the documents available for your account based on IAM permissions\.

   ```
   aws ssm list-documents
   ```

1. Use the following command to view the different versions of a document\.

   ```
   aws ssm list-document-versions ^
       --name "document-name"
   ```

1. Use the following command to run a command that uses an SSM document version\.

   ```
   aws ssm send-command ^
       --document-name "AWS-RunShellScript" ^
       --parameters commands="echo Hello" ^
       --instance-ids instance-ID ^
       --document-version "$LATEST"
   ```

------
#### [ PowerShell ]

**To run commands using the Tools for PowerShell**

1. Install and configure the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. List all available documents

   This command lists all of the documents available for your account based on IAM permissions\. The command returns a list of Linux and Windows documents\.

   ```
   Get-SSMDocumentList
   ```

1. Use the following command to view the different versions of a document\.

   ```
   Get-SSMDocumentVersionList `
       -Name "document-name"
   ```

1. Use the following command to run a command that uses an SSM document version\.

   ```
   Send-SSMCommand `
       -DocumentName "AWS-RunShellScript" `
       -Parameter @{commands = "echo helloWorld"} `
       -InstanceIds "instance-ID" `
       -DocumentVersion $LATEST
   ```

------