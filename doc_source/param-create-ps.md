# Create a Systems Manager parameter \(Tools for Windows PowerShell\)<a name="param-create-ps"></a>

You can use AWS Tools for Windows PowerShell to create `String`, `StringList`, and `SecureString` parameter types\. After deleting a parameter, wait for at least 30 seconds to create a parameter with the same name\.

Parameters can't be referenced or nested in the values of other parameters\. You can't include `{{}}` or `{{ssm:parameter-name}}` in a parameter value\.

**Note**  
Parameters are only available in the AWS Region where they were created\.

**Topics**
+ [Create a `String` parameter \(Tools for Windows PowerShell\)](#param-create-ps-string)
+ [Create a `StringList` parameter \(Tools for Windows PowerShell\)](#param-create-ps-stringlist)
+ [Create a SecureString parameter \(Tools for Windows PowerShell\)](#param-create-ps-securestring)

## Create a `String` parameter \(Tools for Windows PowerShell\)<a name="param-create-ps-string"></a>

1. Install and configure the AWS Tools for PowerShell, if you haven't already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to create a parameter that contains a plain text value\. Replace each *example resource placeholder* with your own information\.

   ```
   Write-SSMParameter `
       -Name "parameter-name" `
       -Value "parameter-value" `
       -Type "String"
   ```

   \-or\-

   Run the following command to create a parameter that contains an Amazon Machine Image \(AMI\) ID as the parameter value\.
**Note**  
To create a parameter with a tag, create the service\.model\.tag before hand as a variable\. Here is an example\.  

   ```
   $tag = New-Object Amazon.SimpleSystemsManagement.Model.Tag
   $tag.Key = "tag-key"
   $tag.Value = "tag-value"
   ```

   ```
   Write-SSMParameter `
       -Name "parameter-name" `
       -Value "an-AMI-id" `
       -Type "String" `
       -DataType "aws:ec2:image" `
       -Tags $tag
   ```

   The `-DataType` option must be specified only if you are creating a parameter that contains an AMI ID\. For all other parameters, the default data type is `text`\. For more information, see [Native parameter support for Amazon Machine Image IDs](parameter-store-ec2-aliases.md)\.

   Here is an example that uses a parameter hierarchy\.

   ```
   Write-SSMParameter `
       -Name "/IAD/Web/SQL/IPaddress" `
       -Value "99.99.99.999" `
       -Type "String" `
       -Tags $tag
   ```

1. Run the following command to verify the details of the parameter\.

   ```
   (Get-SSMParameterValue -Name "the-parameter-name-you-specified").Parameters
   ```

## Create a `StringList` parameter \(Tools for Windows PowerShell\)<a name="param-create-ps-stringlist"></a>

1. Install and configure the AWS Tools for PowerShell, if you haven't already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to create a StringList parameter\. Replace each *example resource placeholder* with your own information\.
**Note**  
To create a parameter with a tag, create the service\.model\.tag before hand as a variable\. Here is an example\.   

   ```
   $tag = New-Object Amazon.SimpleSystemsManagement.Model.Tag
   $tag.Key = "tag-key"
   $tag.Value = "tag-value"
   ```

   ```
   Write-SSMParameter `
       -Name "parameter-name" `
       -Value "a-comma-separated-list-of-values" `
       -Type "StringList" `
       -Tags $tag
   ```

   If successful, the command returns the version number of the parameter\.

   Here is an example\.

   ```
   Write-SSMParameter `
       -Name "stringlist-parameter" `
       -Value "Milana,Mariana,Mark,Miguel" `
       -Type "StringList" `
       -Tags $tag
   ```
**Note**  
Items in a `StringList` must be separated by a comma \(,\)\. You can't use other punctuation or special characters to escape items in the list\. If you have a parameter value that requires a comma, then use the `String` type\.

1. Run the following command to verify the details of the parameter\.

   ```
   (Get-SSMParameterValue -Name "the-parameter-name-you-specified").Parameters
   ```

## Create a SecureString parameter \(Tools for Windows PowerShell\)<a name="param-create-ps-securestring"></a>

Before you create a `SecureString` parameter, read about the requirements for this type of parameter\. For more information, see [Create a SecureString parameter \(AWS CLI\)](param-create-cli.md#param-create-cli-securestring)\.

**Important**  
Only the *value* of a `SecureString` parameter is encrypted\. Parameter names, descriptions, and other properties aren't encrypted\.

**Important**  
Parameter Store only supports [symmetric encryption KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/symm-asymm-concepts.html#symmetric-cmks)\. You can't use an [asymmetric encryption KMS key](https://docs.aws.amazon.com/kms/latest/developerguide/symm-asymm-concepts.html#asymmetric-cmks) to encrypt your parameters\. For help determining whether a KMS key is symmetric or asymmetric, see [Identifying symmetric and asymmetric KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/find-symm-asymm.html) in the *AWS Key Management Service Developer Guide*

1. Install and configure the AWS Tools for PowerShell, if you haven't already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to create a parameter\. Replace each *example resource placeholder* with your own information\.
**Note**  
To create a parameter with a tag, first create the service\.model\.tag as a variable\. Here is an example\.   

   ```
   $tag = New-Object Amazon.SimpleSystemsManagement.Model.Tag
   $tag.Key = "tag-key"
   $tag.Value = "tag-value"
   ```

   ```
   Write-SSMParameter `
       -Name "parameter-name" `
       -Value "parameter-value" `
       -Type "SecureString"  `
       -KeyId "an AWS KMS key ID, an AWS KMS key ARN, an alias name, or an alias ARN" `
       -Tags $tag
   ```

   If successful, the command returns the version number of the parameter\.
**Note**  
To use the AWS managed key assigned to your account, remove the `-KeyId` parameter from the command\.

   Here is an example that uses an obfuscated name \(3l3vat3131\) for a password parameter and an AWS managed key\.

   ```
   Write-SSMParameter `
       -Name "/Finance/Payroll/3l3vat3131" `
       -Value "P@sSwW)rd" `
       -Type "SecureString"`
       -Tags $tag
   ```

1. Run the following command to verify the details of the parameter\.

   ```
   (Get-SSMParameterValue -Name "the-parameter-name-you-specified" –WithDecryption $true).Parameters
   ```

By default, all `SecureString` values are displayed as cipher\-text\. To decrypt a `SecureString` value, a user must have permission to call the AWS KMS [Decrypt](https://docs.aws.amazon.com/kms/latest/APIReference/API_Decrypt.html) API operation\. For information about configuring AWS KMS access control, see [Authentication and Access Control for AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/control-access.html) in the *AWS Key Management Service Developer Guide*\.

**Important**  
If you change the KMS key alias for the KMS key used to encrypt a parameter, then you must also update the key alias the parameter uses to reference AWS KMS\. This only applies to the KMS key alias; the key ID that an alias attaches to stays the same unless you delete the whole key\.