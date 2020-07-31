# Create a Systems Manager parameter \(Tools for Windows PowerShell\)<a name="param-create-ps"></a>

You can use Tools for Windows PowerShell to create `String`, `StringList`, and `SecureString` parameter types\. 

**Note**  
Parameters can't be referenced or nested in the values of other parameters\. You can't include `{{}}` or `{{ssm:parameter-name}}` in a parameter value\.  
Parameters are only available in the AWS Region where they were created\.

**Topics**
+ [Create a `String` parameter \(Tools for Windows PowerShell\)](#param-create-ps-string)
+ [Create a `StringList` parameter \(Tools for Windows PowerShell\)](#param-create-ps-stringlist)
+ [Create a SecureString parameter \(Tools for Windows PowerShell\)](#param-create-ps-securestring)

## Create a `String` parameter \(Tools for Windows PowerShell\)<a name="param-create-ps-string"></a>

1. Install and configure the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to create a parameter that contains a plain text value\.

   ```
   Write-SSMParameter `
       -Name "parameter-name" `
       -Value "parameter-value" `
       -Type "String"
   ```

   \-or\-

   Run the following command to create a parameter that contains an Amazon Machine Image \(AMI\) ID as the parameter value\. 

   ```
   Write-SSMParameter `
       -Name "parameter-name" `
       -Value "an-AMI-id" `
       -Type "String" `
       -DataType "aws:ec2:image" `
       -Tags "Key=tag-key,Value=tag-value"
   ```

   The `-DataType` option must be specified only if you are creating a parameter that contains an AMI ID\. For all other parameters, the default data type is `text`\. For more information, see [Native parameter support for Amazon Machine Image IDs](parameter-store-ec2-aliases.md)\.

   Here is a String example that uses a parameter hierarchy\.

   ```
   Write-SSMParameter `
       -Name "/IAD/Web/SQL/IPaddress" `
       -Value "99.99.99.999" `
       -Type "String" `
       Tags "Key=Region,Value=IAD"
   ```

1. Run the following command to verify the details of the parameter\.

   ```
   (Get-SSMParameterValue -Name "the-parameter-name-you-specified").Parameters
   ```

## Create a `StringList` parameter \(Tools for Windows PowerShell\)<a name="param-create-ps-stringlist"></a>

1. Install and configure the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to create a StringList parameter\.

   ```
   Write-SSMParameter `
       -Name "parameter-name" `
       -Value "a-comma-separated-list-of-values" `
       -Type "StringList" `
       -Tags "Key=tag-key,Value=tag-value"
   ```

   If successful, the command returns the version number of the parameter\.
**Note**  
Items in a `StringList` must be separated by a comma \(,\)\. You can't use other punctuation or special character to escape items in the list\. If you have a parameter value that requires a comma, then use the `String` type\.

1. Run the following command to verify the details of the parameter\.

   ```
   (Get-SSMParameterValue -Name "the-parameter-name-you-specified").Parameters
   ```

## Create a SecureString parameter \(Tools for Windows PowerShell\)<a name="param-create-ps-securestring"></a>

Before you create a `SecureString` parameter, read about the requirements for this type of parameter\. For more information, see [SecureString parameters](sysman-paramstore-securestring.md)\.

1. Install and configure the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to create a parameter\.

   ```
   Write-SSMParameter `
       -Name "parameter-name" `
       -Value "parameter-value" `
       -Type "SecureString"  `
       -KeyId "a KMS CMK ID, a KMS CMK ARN, an alias name, or an alias ARN" `
       --tags "Key=tag-key,Value=tag-value"
   ```

   If successful, the command returns the version number of the parameter\.
**Note**  
To use the AWS\-managed customer master key \(CMK\) assigned to your account, remove the `-KeyId` parameter from the command\.

   Here is an example that uses an obfuscated name \(3l3vat3131\) for a password parameter and an AWS\-managed customer master key \(CMK\)\.

   ```
   Write-SSMParameter `
       -Name "/Finance/Payroll/3l3vat3131" `
       -Value "P@sSwW)rd" `
       -Type "SecureString"
   ```

1. Run the following command to verify the details of the parameter\.

   ```
   (Get-SSMParameterValue -Name "the-parameter-name-you-specified" –WithDecryption $true).Parameters
   ```