# Create a Systems Manager Parameter \(Tools for Windows PowerShell\)<a name="param-create-ps"></a>

You can use Tools for Windows PowerShell to create a parameter that uses the `String`, `StringList`, or `SecureString` data type\. 

**Note**  
Parameters are only available in the Region where they were created\.

**Topics**
+ [Create a `String` or `StringList` parameter \(Tools for Windows PowerShell\)](#param-create-ps-string-stringlist)
+ [Create a `SecureString` parameter \(Tools for Windows PowerShell\)](#param-create-ps-securestring)

## Create a `String` or `StringList` parameter \(Tools for Windows PowerShell\)<a name="param-create-ps-string-stringlist"></a>

1. Open AWS Tools for Windows PowerShell and execute the following command to specify your credentials\. You must either have administrator privileges in Amazon EC2, or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.

   ```
   Set-AWSCredentials –AccessKey key_name –SecretKey key_name
   ```

1. Execute the following command to set the Region for your PowerShell session\. The example uses the us\-east\-2 Region\.

   ```
   Set-DefaultAWSRegion -Region us-east-2
   ```

1. Execute the following command to create a parameter\.

   ```
   Write-SSMParameter -Name "a name" -Value "a value, or a comma-separated list of values" -Type "String or StringList" 
   ```

   If successful, the command returns the version number of the parameter\.
**Note**  
Items in a `StringList` must be separated by a comma \(,\)\. You can't use other punctuation or special character to escape items in the list\. If you have a parameter value that requires a comma, then use the `String` data type\.

   Here is an example that uses a String data type\.

   ```
   Write-SSMParameter -Name "/IAD/Web/SQL/IPaddress" -Value "99.99.99.999" -Type "String"
   ```

1. Execute the following command to verify the details of the parameter\.

   ```
   (Get-SSMParameterValue -Name "the name you specified").Parameters
   ```

## Create a `SecureString` parameter \(Tools for Windows PowerShell\)<a name="param-create-ps-securestring"></a>

Before you create a `SecureString` parameter, read about the requirements for this type of parameter\. For more information, see [Use Secure String Parameters](sysman-paramstore-about.md#sysman-paramstore-securestring)\.

1. Open AWS Tools for Windows PowerShell and execute the following command to specify your credentials\. You must either have administrator privileges in Amazon EC2, or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.

   ```
   Set-AWSCredentials –AccessKey key_name –SecretKey key_name
   ```

1. Execute the following command to set the Region for your PowerShell session\. The example uses the us\-east\-2 region\.

   ```
   Set-DefaultAWSRegion -Region us-east-2
   ```

1. Execute the following command to create a parameter\.

   ```
   Write-SSMParameter -Name "a name" -Value "a value" -Type "SecureString"  -KeyId "a KMS CMK ID, a KMS CMK ARN, an alias name, or an alias ARN"
   ```

   If successful, the command returns the version number of the parameter\.
**Note**  
To use the default AWS KMS CMK assigned to your account, remove the `-KeyId` parameter from the command\.

   Here is an example that uses an obfuscated name \(elixir3131\) for a password parameter and the user's default KMS CMK\.

   ```
   Write-SSMParameter -Name "/Finance/Payroll/elixir3131" -Value "P@sSwW)rd" -Type "SecureString"
   ```

1. Execute the following command to verify the details of the parameter\.

   ```
   (Get-SSMParameterValue -Name "the name you specified" –WithDecryption $true).Parameters
   ```