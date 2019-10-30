# Create a Systems Manager Parameter \(AWS CLI\)<a name="param-create-cli"></a>

You can use the AWS CLI to create a parameter that uses the `String`, `StringList`, or `SecureString` data type\. 

For more information about using the AWS CLI to create parameters, see [Walkthrough: Create and Update a String Parameter \(AWS CLI\)](sysman-paramstore-cli.md)\.

**Note**  
Parameters are only available in the AWS Region where they were created\.

**Topics**
+ [Create a `String` or `StringList` Parameter \(AWS CLI\)](#param-create-cli-string-stringlist)
+ [Create a Secure String Parameter \(AWS CLI\)](#param-create-cli-securestring)

## Create a `String` or `StringList` Parameter \(AWS CLI\)<a name="param-create-cli-string-stringlist"></a>

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or Upgrade the AWS CLI](getting-started-cli.md)\.

1. Run the following command to create a parameter\.

   ```
   aws ssm put-parameter --name "parameter_name" --value "a parameter value, or a comma-separated list of values" --type String or StringList
   ```

   If successful, the command returns the version number of the parameter\.

   This example adds two key\-value pair tags to a parameter\. \(Depending on the operating system type on your local machine, run one of the following commands\. The version to run from a local Windows machine includes the escape characters \("\\"\) that you need to run the command from your command line tool\.\)

   **Windows** local machine:

   ```
   aws ssm put-parameter --name parameter-name --value "parameter-value, or a comma-separated-list-of-values" --type "String" --tags [{\"Key\":\"Region1\",\"Value\":\"East1\"},{\"Key\":\"Environment1\",\"Value\":\"Production1\"}]
   ```

   **Linux** local machine:

   ```
   aws ssm put-parameter --name parameter-name --value "parameter-value, or a comma-separated-list-of-values" --type "String" --tags '[{"Key":"Region","Value":"East"},{"Key":"Environment", "Value":"Production"}]'
   ```

   Here is an example that uses the `StringList` data type\.

   ```
   aws ssm put-parameter --name /IAD/ERP/Oracle/addUsers --value "Milana,Mariana,Mark,Miguel" --type StringList --tier Standard
   ```
**Note**  
Items in a `StringList` must be separated by a comma \(,\)\. You can't use other punctuation or special character to escape items in the list\. If you have a parameter value that requires a comma, then use the `String` data type\.

1. Run the following command to verify the details of the parameter\.

   ```
   aws ssm get-parameters --name "the_parameter_name_you_specified"
   ```

   Here is an example that uses the name specified in the earlier example\.

   ```
   aws ssm get-parameters --name "/IAD/ERP/Oracle/addUsers"
   ```

## Create a Secure String Parameter \(AWS CLI\)<a name="param-create-cli-securestring"></a>

Before you create a Secure String parameter, read about the requirements for this type of parameter\. For more information, see [About Secure String Parameters](sysman-paramstore-securestring.md)\.

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or Upgrade the AWS CLI](getting-started-cli.md)\.

1. Run the following command to create a parameter\.

   ```
   aws ssm put-parameter --name "parameter_name" --value "parameter value" --type SecureString --key-id "a KMS CMK ID, a KMS CMK ARN, an alias name, or an alias ARN"
   ```
**Note**  
To use the AWS Key Management Service \(KMS\) customer master key \(CMK\) assigned to your account, remove the `key-id` parameter from the command\. For more information about CMKs, see [AWS Key Management Service Concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#aws-managed-cmk) in the *AWS Key Management Service Developer Guide*\.

   The following example uses an obfuscated name \(`elixir3131`\) for a password parameter and a CMK\.

   ```
   aws ssm put-parameter --name /Finance/Payroll/elixir3131 --value "P@sSwW)rd" --type SecureString --key-id arn:aws:kms:us-east-2:123456789012:key/1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d5e
   ```

1. Run the following command to verify the details of the parameter\.

   ```
   aws ssm get-parameters --name "the_parameter_name_you_specified" --with-decryption
   ```
**Note**  
If you don't specify the `with-decryption` parameter, or if you specify the `no-with-decryption` parameter, the command returns an encrypted GUID\.