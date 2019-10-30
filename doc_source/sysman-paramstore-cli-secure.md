# Walkthrough: Create and Update a SecureString Parameter \(AWS CLI\)<a name="sysman-paramstore-cli-secure"></a>

Use the following procedure to create a secure string parameter\. For more information about secure string parameters, see [About Secure String Parameters](sysman-paramstore-securestring.md)\.

**To create a secure string parameter using the AWS CLI**

1. Run one of the following commands to create a parameter that uses the `SecureString` datatype\.

   **Create a secure string parameter that uses a customer managed customer master key \(CMK\)**

   ```
   aws ssm put-parameter --name "parameter_name" --value "a value, for example P@ssW%rd#1" --type "SecureString"
   ```

   **Create a secure string parameter that uses a custom AWS KMS key**

   ```
   aws ssm put-parameter --name "parameter_name" --value "a parameter value" --type "SecureString" --key-id "your-AWS-user-account ID/the-custom-AWS KMS-key"
   ```

   Here is an example that uses a customer managed CMK\.

   ```
   aws ssm put-parameter --name "my-password" --value "P@ssW%rd#1" --type "SecureString" --key-id "arn:aws:kms:us-east-2:123456789012:key/1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d5e"
   ```

1. Run the following command to view the parameter metadata\.

   ```
   aws ssm describe-parameters --filters "Key=Name,Values=the_name_that_you_specified"
   ```

1. Run the following command to change the parameter value\.

   ```
   aws ssm put-parameter --name "the_name_that_you_specified" --value "new parameter value" --type "SecureString" --overwrite
   ```

   **Updating a secure string parameter that uses a customer managed customer master key \(CMK\)**

   ```
   aws ssm put-parameter --name "the_name_that_you_specified" --value "new parameter value" --type "SecureString" --key-id "the-CMK-ID" --overwrite
   ```

   **Updating a secure string parameter that uses a customer managed CMK**

   ```
   aws ssm put-parameter --name "the_name_that_you_specified" --value "new parameter value" --type "SecureString" --key-id "your-AWS-user-account-alias/the-CMK-ID" --overwrite
   ```

1. Run the following command to view the latest parameter value\.

   ```
   aws ssm get-parameters --names "the_name_that_you_specified" --with-decryption
   ```

1. Run the following command to view the parameter value history\.

   ```
   aws ssm get-parameter-history --name "the_name_that_you_specified"
   ```

**Important**  
Only the *value* of a secure string parameter is encrypted\. Parameter names, descriptions, and other properties are not encrypted\.