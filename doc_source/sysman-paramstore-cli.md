# Walkthrough: Create and Use a Parameter in a Command \(AWS CLI\)<a name="sysman-paramstore-cli"></a>

The following procedure walks you through the process of creating and storing a parameter using the AWS CLI\.

**To create and use a parameter in a command \(AWS CLI\)**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or Upgrade and then Configure the AWS CLI](getting-started-cli.md)\.

1. Run the following command to create a parameter that uses the String data type\. The `--name` parameter uses a hierarchy\. For more information about hierarchies, see [Organizing Parameters into Hierarchies](sysman-paramstore-su-organize.md)\.

   ```
   aws ssm put-parameter --name "parameter_name" --value "a parameter value" --type String --tier Standard or Advanced
   ```

   Here is an example that uses a parameter hierarchy in the name\. For more information about parameter hierarchies, see [Organizing Parameters into Hierarchies](sysman-paramstore-su-organize.md)\.

   ```
   aws ssm put-parameter --name "/Test/IAD/helloWorld" --value "My1stParameter" --type String --tier Advanced
   ```

   The command returns the version number of the parameter\.

1. Run the following command to view the parameter metadata\.

   ```
   aws ssm describe-parameters --filters "Key=Name,Values=/Test/IAD/helloWorld"
   ```
**Note**  
*Name* must be capitalized\.

   The system returns information like the following\.

   ```
   {
       "Parameters": [
           {
               "LastModifiedUser": "arn:aws:iam::123456789:user/User's name",
               "LastModifiedDate": 1494529763.156,
               "Type": "String",
               "Name": "helloworld"
           }
       ]
   }
   ```

1. Run the following command to change the parameter value\.

   ```
   aws ssm put-parameter --name "/Test/IAD/helloWorld" --value "good day sunshine" --type String --overwrite
   ```

   The command returns the version number of the parameter\.

1. Run the following command to view the latest parameter value\.

   ```
   aws ssm get-parameters --names "/Test/IAD/helloWorld"
   ```

   The system returns information like the following\.

   ```
   {
       "InvalidParameters": [],
       "Parameters": [
           {
               "Type": "String",
               "Name": "/Test/IAD/helloWorld",
               "Value": "good day sunshine"
           }
       ]
   }
   ```

1. Run the following command to view the parameter value history\.

   ```
   aws ssm get-parameter-history --name "/Test/IAD/helloWorld"
   ```

1. Run the following command to use this parameter in a command\.

   ```
   aws ssm send-command --document-name "AWS-RunShellScript" --parameters '{"commands":["echo {{ssm:/Test/IAD/helloWorld}}"]}' --targets "Key=instanceids,Values=instance-ids"
   ```

Use the following procedure to create a secure string parameter\. For more information about secure string parameters, see [About Secure String Parameters](sysman-paramstore-securestring.md)\.

**To create a secure string parameter using the AWS CLI**

1. Run one of the following commands to create a parameter that uses the `SecureString` datatype\.

   **Create a secure string parameter that uses a customer managed customer master key \(CMK\)**

   ```
   aws ssm put-parameter --name "parameter_name" --value "a value, for example P@ssW%rd#1" --type "SecureString"
   ```

   **Create a secure string parameter that uses a custom AWS KMS key**

   ```
   aws ssm put-parameter --name "parameter_name" --value "a parameter value" --type "SecureString" --key-id "your-AWS-user-account ID/the-custom-AWS KMS-key" --tier Standard or Advanced
   ```

   Here is an example that uses a customer managed CMK\.

   ```
   aws ssm put-parameter --name "my-password" --value "P@ssW%rd#1" --type "SecureString" --key-id "arn:aws:kms:us-east-2:123456789012:key/1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d5e" --tier Advanced
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