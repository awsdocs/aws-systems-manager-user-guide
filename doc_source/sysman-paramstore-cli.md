# Walkthrough: Create and Use a Parameter in a Command \(AWS CLI\)<a name="sysman-paramstore-cli"></a>

The following procedure walks you through the process of creating and storing a parameter using the AWS CLI\.

**To create a String parameter using Parameter Store**

1. [Download](https://aws.amazon.com/cli/) the AWS CLI to your local machine\.

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

1. Execute the following command to create a parameter that uses the String data type\. The `--name` parameter uses a hierarchy\. For more information about hierarchies, see [Organizing Parameters into Hierarchies](sysman-paramstore-su-organize.md)\.

   ```
   aws ssm put-parameter --name "a_name" --value "a value" --type String
   ```

   Here is an example that uses a parameter hierarchy in the name\. For more information about parameter hierarchies, see [Organizing Parameters into Hierarchies](sysman-paramstore-su-organize.md)\.

   ```
   aws ssm put-parameter --name "/Test/IAD/helloWorld" --value "My1stParameter" --type String 
   ```

   The command returns the version number of the parameter\.

1. Execute the following command to view the parameter metadata\.

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

1. Execute the following command to change the parameter value\.

   ```
   aws ssm put-parameter --name "/Test/IAD/helloWorld" --value "good day sunshine" --type String --overwrite
   ```

   The command returns the version number of the parameter\.

1. Execute the following command to view the latest parameter value\.

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

1. Execute the following command to view the parameter value history\.

   ```
   aws ssm get-parameter-history --name "/Test/IAD/helloWorld"
   ```

1. Execute the following command to use this parameter in a command\.

   ```
   aws ssm send-command --document-name "AWS-RunShellScript" --parameters "commands=["echo {{ssm:/Test/IAD/helloWorld}}"]" --targets "Key=instance-ids,Values=the ID of an instance configured for Systems Manager"
   ```

Use the following procedure to create a Secure String parameter\. For more information about Secure String parameters, see [Use Secure String Parameters](sysman-paramstore-about.md#sysman-paramstore-securestring)\.

**To create a Secure String parameter using the AWS CLI**

1. Execute one of the following commands to create a parameter that uses the Secure String data type\.

   **Create a Secure String parameter that uses your default KMS key**

   ```
   aws ssm put-parameter --name "a_name" --value "a value, for example P@ssW%rd#1" --type "SecureString"
   ```

   **Create a Secure String parameter that uses a custom AWS KMS key**

   ```
   aws ssm put-parameter --name "a_name" --value "a value" --type "SecureString" --key-id "your AWS user account ID/the custom AWS KMS key"
   ```

   Here is an example that uses a custom AWS KMS key\.

   ```
   aws ssm put-parameter --name "my-password" --value "P@ssW%rd#1" --type "SecureString" --key-id "arn:aws:kms:us-east-2:123456789012:key/1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d5e"
   ```
**Important**  
Only the value of a secure string parameter is encrypted\. Parameter names, descriptions, and other properties are not encrypted\. Therefore, to make it less obvious which parameters contain passwords, we recommend using a naming system that avoids the actual word "password" in your parameter names\. For illustration, however, we are using the sample parameter name *my\-password* in our examples\.

1. Execute the following command to view the parameter metadata\.

   ```
   aws ssm describe-parameters --filters "Key=Name,Values=the name that you specified"
   ```

1. Execute the following command to change the parameter value\.

   ```
   aws ssm put-parameter --name "the name that you specified" --value "new value" --type "SecureString" --overwrite
   ```

   **Updating a Secure String parameter that uses your default KMS key**

   ```
   aws ssm put-parameter --name "the name that you specified" --value "new value" --type "SecureString" --key-id "the AWS KMS key ID" --overwrite
   ```

   **Updating a Secure String parameter that uses a custom KMS key**

   ```
   aws ssm put-parameter --name "the name that you specified" --value "new value" --type "SecureString" --key-id "your AWS user account alias/the custom KMS key" --overwrite
   ```

1. Execute the following command to view the latest parameter value\.

   ```
   aws ssm get-parameters --names "the name that you specified" --with-decryption
   ```

1. Execute the following command to view the parameter value history\.

   ```
   aws ssm get-parameter-history --name "the name that you specified"
   ```

**Important**  
Only the value of a secure string parameter is encrypted\. Parameter names, descriptions, and other properties are not encrypted\. Therefore, to make it less obvious which parameters contain passwords, we recommend using a naming system that avoids the actual word "password" in your parameter names\. For illustration, however, we are using the sample parameter name *my\-password* in our examples\.