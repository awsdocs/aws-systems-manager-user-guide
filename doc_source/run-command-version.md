# Sending Commands that Use the Document Version Parameter<a name="run-command-version"></a>

You can use the document\-version parameter to specify which version of an SSM document to use when the command executes\. You can specify one of the following options for this parameter:
+ $DEFAULT
+ $LATEST
+ Version number

If you execute commands by using the AWS CLI, then you must escape the first two options by using a backslash\. If you specify a version number, then you don't need to use the backslash\. For example:

\-\-document\-version "\\$DEFAULT"

\-\-document\-version "\\$LATEST"

\-\-document\-version "3"

Use the following procedure to execute a command by using the AWS CLI that uses the document\-version parameter\. 

**To execute commands using the AWS CLI**

1. Run the following command to specify your credentials and the region\.

   ```
   aws configure
   ```

1. The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: us-east-1
   Default output format [None]: ENTER
   ```

1. List all available documents

   This command lists all of the documents available for your account based on IAM permissions\. The command returns a list of Linux and Windows documents\.

   ```
   aws ssm list-documents
   ```

1. Use the following command to view the different versions of a document\.

   ```
   aws ssm list-document-versions --name "document name"
   ```

1. Use the following command to execute a command that uses an SSM document version\.

   ```
   aws ssm send-command --document-name "AWS-RunShellScript" --parameters commands="echo Hello",executionTimeout=3600 --instance-ids instance-ID --endpoint-url "https://sonic.us-east-1.amazonaws.com" --region "us-east-1" --document-version "\$DEFAULT, \$LATEST, or a version number"
   ```