# Sending Commands that Use the Document Version Parameter<a name="run-command-version"></a>

You can use the document\-version parameter to specify which version of an SSM document to use when the command runs\. You can specify one of the following options for this parameter:
+ $DEFAULT
+ $LATEST
+ Version number

If you run commands by using the AWS CLI, then you must escape the first two options by using a backslash\. If you specify a version number, then you don't need to use the backslash\. For example:

\-\-document\-version "\\$DEFAULT"

\-\-document\-version "\\$LATEST"

\-\-document\-version "3"

Use the following procedure to run a command by using the AWS CLI that uses the document\-version parameter\. 

**To run commands using the AWS CLI**

1. Run the following command to specify your credentials and the region\.

   ```
   aws configure
   ```

1. The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

   *region* represents the Region identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager table of regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

1. List all available documents

   This command lists all of the documents available for your account based on IAM permissions\. The command returns a list of Linux and Windows documents\.

   ```
   aws ssm list-documents
   ```

1. Use the following command to view the different versions of a document\.

   ```
   aws ssm list-document-versions --name "document name"
   ```

1. Use the following command to run a command that uses an SSM document version\.

   ```
   aws ssm send-command --document-name "AWS-RunShellScript" --parameters commands="echo Hello",executionTimeout=3600 --instance-ids instance-ID --endpoint-url "https://us-east-2.amazonaws.com" --region "us-east-2" --document-version "\$DEFAULT, \$LATEST, or a version number"
   ```