# Create a Systems Manager parameter \(AWS CLI\)<a name="param-create-cli"></a>

You can use the AWS CLI to create `String`, `StringList`, and `SecureString` parameter types\. 

For more information about using the AWS CLI to create parameters, see [Walkthrough: Create and update a String parameter \(AWS CLI\)](sysman-paramstore-cli.md)\.

**Note**  
Parameters are only available in the AWS Region where they were created\.  
Parameters can't be referenced or nested in the values of other parameters\. You can't include `{{}}` or `{{ssm:parameter-name}}` in a parameter value\.

**Topics**
+ [Create a `String` Parameter \(AWS CLI\)](#param-create-cli-string)
+ [Create a `StringList` parameter \(AWS CLI\)](#param-create-cli-stringlist)
+ [Create a SecureString parameter \(AWS CLI\)](#param-create-cli-securestring)
+ [Create a multi\-line parameter \(AWS CLI\)](param-create-cli-multiline.md)

## Create a `String` Parameter \(AWS CLI\)<a name="param-create-cli-string"></a>

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to create a parameter that contains a plain text value\.

------
#### [ Linux ]

   ```
   aws ssm put-parameter \
       --name "parameter-name" \
       --value "parameter-value" \
       --type String \
       --tags "Key=tag-key,Value=tag-value"
   ```

------
#### [ Windows ]

   ```
   aws ssm put-parameter ^
       --name "parameter-name" ^
       --value "parameter-value" ^
       --type String ^
       --tags "Key=tag-key,Value=tag-value"
   ```

------

   \-or\-

   Run the following command to create a parameter that contains an Amazon Machine Image \(AMI\) ID as the parameter value\.

------
#### [ Linux ]

   ```
   aws ssm put-parameter \
       --name "parameter-name" \
       --value "an-AMI-id" \
       --type String \
       --data-type "aws:ec2:image" \
       --tags "Key=tag-key,Value=tag-value"
   ```

------
#### [ Windows ]

   ```
   aws ssm put-parameter ^
       --name "parameter-name" ^
       --value "an-AMI-id" ^
       --type String ^
       --data-type "aws:ec2:image" ^
       --tags "Key=tag-key,Value=tag-value"
   ```

------

   The `--data-type` option must be specified only if you are creating a parameter that contains an AMI ID\. For all other parameters, the default data type is `text`\. For more information, see [Native parameter support for Amazon Machine Image IDs](parameter-store-ec2-aliases.md)\.
**Important**  
If successful, the command returns the version number of the parameter\. **Exception**: If you have specified `aws:ec2:image` as the data type, a new version number in the response does not mean that the parameter value has been validated yet\. For more information, see [Native parameter support for Amazon Machine Image IDs](parameter-store-ec2-aliases.md)\.

   The following example adds two key\-value pair tags to a parameter\. 

------
#### [ Linux ]

   ```
   aws ssm put-parameter \
       --name parameter-name \
       --value "parameter-value" \
       --type "String" \
       --tags '[{"Key":"Region","Value":"East"},{"Key":"Environment", "Value":"Production"}]'
   ```

------
#### [ Windows ]

   ```
   aws ssm put-parameter ^
       --name parameter-name ^
       --value "parameter-value" ^
       --type "String" ^
       --tags [{\"Key\":\"Region1\",\"Value\":\"East1\"},{\"Key\":\"Environment1\",\"Value\":\"Production1\"}]
   ```

------

1. Run the following command to verify the details of the parameter\.

   ```
   aws ssm get-parameters --name "parameter-name"
   ```

## Create a `StringList` parameter \(AWS CLI\)<a name="param-create-cli-stringlist"></a>

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to create a parameter\.

------
#### [ Linux ]

   ```
   aws ssm put-parameter \
       --name "parameter-name" \
       --value "a-comma-separated-list-of-values" \
       --type StringList \
       --tags "Key=tag-key,Value=tag-value"
   ```

------
#### [ Windows ]

   ```
   aws ssm put-parameter ^
       --name "parameter-name" ^
       --value "a-comma-separated-list-of-values" ^
       --type StringList ^
       --tags "Key=tag-key,Value=tag-value"
   ```

------
**Note**  
If successful, the command returns the version number of the parameter\.

   This example adds two key\-value pair tags to a parameter\. \(Depending on the operating system type on your local machine, run one of the following commands\. The version to run from a local Windows machine includes the escape characters \("\\"\) that you need to run the command from your command line tool\.\)

   Here is a `StringList` example that uses a parameter hierarchy\.

------
#### [ Linux ]

   ```
   aws ssm put-parameter \
       --name /IAD/ERP/Oracle/addUsers \
       --value "Milana,Mariana,Mark,Miguel" \
       --type StringList
   ```

------
#### [ Windows ]

   ```
   aws ssm put-parameter ^
       --name /IAD/ERP/Oracle/addUsers ^
       --value "Milana,Mariana,Mark,Miguel" ^
       --type StringList
   ```

------
**Note**  
Items in a `StringList` must be separated by a comma \(,\)\. You can't use other punctuation or special character to escape items in the list\. If you have a parameter value that requires a comma, then use the `String` type\.

1. Run the `get-parameters` command to verify the details of the parameter\. For example:

   ```
   aws ssm get-parameters --name "/IAD/ERP/Oracle/addUsers"
   ```

## Create a SecureString parameter \(AWS CLI\)<a name="param-create-cli-securestring"></a>

Before you create a `SecureString` parameter, read about the requirements for this type of parameter\. For more information, see [SecureString parameters](sysman-paramstore-securestring.md)\.

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to create a parameter\.

------
#### [ Linux ]

   ```
   aws ssm put-parameter \
       --name "parameter-name" \
       --value "parameter-value" \
       --type SecureString \
       --key-id "a KMS CMK ID, a KMS CMK ARN, an alias name, or an alias ARN" \
       --tags "Key=tag-key,Value=tag-value"
   ```

------
#### [ Windows ]

   ```
   aws ssm put-parameter ^
       --name "parameter-name" ^
       --value "parameter-value" ^
       --type SecureString ^
       --key-id "a KMS CMK ID, a KMS CMK ARN, an alias name, or an alias ARN" ^
       --tags "Key=tag-key,Value=tag-value"
   ```

------
**Note**  
To use the AWS Key Management Service \(KMS\) customer master key \(CMK\) assigned to your account, remove the `key-id` parameter from the command\. For more information about CMKs, see [AWS Key Management Service Concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#aws-managed-cmk) in the *AWS Key Management Service Developer Guide*\.

   The following example uses an obfuscated name \(`3l3vat3131`\) for a password parameter and a CMK\.

------
#### [ Linux ]

   ```
   aws ssm put-parameter \
       --name /Finance/Payroll/3l3vat3131 \
       --value "P@sSwW)rd" \
       --type SecureString \
       --key-id arn:aws:kms:us-east-2:123456789012:key/1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d5e
   ```

------
#### [ Windows ]

   ```
   aws ssm put-parameter ^
       --name /Finance/Payroll/3l3vat3131 ^
       --value "P@sSwW)rd" ^
       --type SecureString ^
       --key-id arn:aws:kms:us-east-2:123456789012:key/1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d5e
   ```

------

1. Run the following command to verify the details of the parameter\.

------
#### [ Linux ]

   ```
   aws ssm get-parameters \
       --name "the-parameter-name-you-specified" \
       --with-decryption
   ```

------
#### [ Windows ]

   ```
   aws ssm get-parameters ^
       --name "the-parameter-name-you-specified" ^
       --with-decryption
   ```

------
**Note**  
If you don't specify the `with-decryption` parameter, or if you specify the `no-with-decryption` parameter, the command returns an encrypted GUID\.