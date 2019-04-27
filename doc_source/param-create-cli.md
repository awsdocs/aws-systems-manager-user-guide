# Create a Systems Manager Parameter \(AWS CLI\)<a name="param-create-cli"></a>

You can use the AWS CLI to create a parameter that uses the `String`, `StringList`, or `SecureString` data type\. 

For more information about using the AWS CLI to create parameters, see [Walkthrough: Create and Use a Parameter in a Command \(AWS CLI\)](sysman-paramstore-cli.md)\.

**Note**  
Parameters are only available in the Region where they were created\.

**Topics**
+ [Create a `String` or `StringList` Parameter \(AWS CLI\)](#param-create-cli-string-stringlist)
+ [Create a Secure String Parameter \(AWS CLI\)](#param-create-cli-securestring)

## Create a `String` or `StringList` Parameter \(AWS CLI\)<a name="param-create-cli-string-stringlist"></a>

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in Amazon EC2, or you must have been granted the appropriate permission in AWS Identity and Access Management \(IAM\)\.

   ```
   aws configure
   ```

   The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

1. Execute the following command to create a parameter\.

   ```
   aws ssm put-parameter --name "parameter_name" --value "a parameter value, or a comma-separated list of values" --type String or StringList --tier Standard or Advanced
   ```

   If successful, the command returns the version number of the parameter\.

   Here is an example that uses the StringList data type\.

   ```
   aws ssm put-parameter --name /IAD/ERP/Oracle/addUsers --value "Milana,Mariana,Mark,Miguel" --type StringList --tier Standard
   ```
**Note**  
If you don't specify a `tier` parameter, the systems creates a standard parameter\. For more information, about advanced parameters, see [About Systems Manager Advanced Parameters](parameter-store-advanced-parameters.md)\.
Items in a `StringList` must be separated by a comma \(,\)\. You can't use other punctuation or special character to escape items in the list\. If you have a parameter value that requires a comma, then use the `String` data type\.

1. Execute the following command to verify the details of the parameter\.

   ```
   aws ssm get-parameters --name "the_parameter_name_you_specified"
   ```

   Here is an example that uses the name specified in the earlier example\.

   ```
   aws ssm get-parameters --name "/IAD/ERP/Oracle/addUsers"
   ```

## Create a Secure String Parameter \(AWS CLI\)<a name="param-create-cli-securestring"></a>

Before you create a secure string parameter, read about the requirements for this type of parameter\. For more information, see [About Secure String Parameters](sysman-paramstore-securestring.md)\.

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges, or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.

   ```
   aws configure
   ```

   The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

1. Execute the following command to create a parameter\.

   ```
   aws ssm put-parameter --name "parameter_name" --value "parameter value" --type SecureString --key-id "a KMS CMK ID, a KMS CMK ARN, an alias name, or an alias ARN" --tier Standard or Advanced
   ```
**Note**  
To use the AWS Key Management Service \(KMS\) customer master key \(CMK\) assigned to your account, remove the `key-id` parameter from the command\. For more information about CMKs, see [AWS Key Management Service Concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#aws-managed-cmk) in the *AWS Key Management Service Developer Guide*\.

   Here is an example that uses an obfuscated name \(elixir3131\) for a password parameter and a CMK\.

   ```
   aws ssm put-parameter --name /Finance/Payroll/elixir3131 --value "P@sSwW)rd" --type SecureString --key-id arn:aws:kms:us-east-2:123456789012:key/1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d5e --tier Advanced
   ```

1. Execute the following command to verify the details of the parameter\.

   ```
   aws ssm get-parameters --name "the_parameter_name_you_specified" --with-decryption
   ```
**Note**  
If you don't specify the `with-decryption` parameter, or if you specify the `no-with-decryption` parameter, the command returns an encrypted GUID\.