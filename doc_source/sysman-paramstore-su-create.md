# Creating Systems Manager Parameters<a name="sysman-paramstore-su-create"></a>

You can create parameters by using the AWS CLI, AWS Tools for Windows PowerShell, the Amazon EC2 console, or the AWS Systems Manager console\.


+ [About Creating Systems Manager Parameters](#sysman-paramstore-su-create-about)
+ [Create a Systems Manager Parameter \(AWS CLI\)](#param-create-cli)
+ [Create a Systems Manager Parameter \(Tools for Windows PowerShell\)](#param-create-ps)
+ [Create a Systems Manager Parameter \(Console\)](#param-create-console)

## About Creating Systems Manager Parameters<a name="sysman-paramstore-su-create-about"></a>

When you create a parameter, you specify the following information:

+ **Name**: \(Required\) Specify a name to identify your parameter\. 

  Be aware of the following requirements and restrictions for Systems Manager parameter names:

  + Parameter names are case sensitive\.

  + A parameter name must be unique within your AWS account\. For example, Systems Manager treats the following as separate parameters:

    + `/CMH/TestParam1`

    + `/TestParam1`

    The following examples are also unique:

    + `/CMH/TestParam1/Logpath1`

    + `/CMH/TestParam1`

    The following examples are not unique:

    + `/TestParam1`

    + `TestParam1`

  + A parameter name *can't* be prefixed with "aws" or "ssm" \(case\-insensitive\)\. For example, the following will fail with an exception:

    + awsTestParameter

    + SSM\-testparameter

    + /aws/testparam1

  + Parameter names can only include the following symbols and letters:

    + `a-zA-Z0-9_.-/`

  + A parameter name can't include spaces\.

  + If you specify a parameter hierarchy, the hierarchy can have a maximum depth of fifteen levels\. You can define a parameter at any level of the hierarchy\. Both of the following examples are structurally valid:

    + `/Level-1/L2/L3/L4/L5/L6/L7/L8/L9/L10/L11/L12/L13/L14/parameter-name`

    + `parameter-name`

    Attempting to create the following parameter would fail with a `HierarchyLevelLimitExceededException` exception:

    + `/Level-1/L2/L3/L4/L5/L6/L7/L8/L9/L10/L11/L12/L13/L14/L15/L16/parameter-name`

+ **Data Type**: \(Required\) Specify a data type to define how the system uses a parameter\. 

  Parameter Store currently supports the following data types:

  + `String`

  + `StringList`

  + `SecureString`
**Note**  
Items in a `StringList` must be separated by a comma \(,\)\. You can't use other punctuation or special character to escape items in the list\. If you have a parameter value that requires a comma, then use the `String` data type\.

+ **Description** \(Optional, but recommended\): Type a description to help you identify parameters and their intended use\.

+ **Value**: \(Required\) Your parameter value\.

+ **Key ID**: `Key ID` applies only to parameters that use the `SecureString` data type\. `Key ID` can either be the default AWS Key Management Service \(AWS KMS\) key automatically assigned to your AWS account or a custom key\. Note the following:

  + To use your default AWS KMS key, choose the `SecureString` data type, and do *not* specify the `Key ID` when you create the parameter\. The system automatically populates `Key ID` with your default KMS key\. 

  + To use a custom KMS key, choose the `SecureString` data type with the `Key ID` parameter\. 

After you create a parameter, you can specify it in your SSM documents, commands, or scripts using the following syntax \(no space between brackets\):

\{\{ssm:*parameter\_name*\}\} or \{\{ ssm:*parameter\_name* \}\}

**Note**  
The *name* of a Systems Manager parameter can't be prefixed with "ssm" or "aws," but when you specify the parameter in an SSM document or a command, the syntax includes "ssm", as shown in the following examples\.  
Valid: `{{ssm:addUsers}}`  
Invalid: `{{ssm:ssmAddUsers}}`

## Create a Systems Manager Parameter \(AWS CLI\)<a name="param-create-cli"></a>

You can use the AWS CLI to create a parameter that uses the `String`, `StringList`, or `SecureString` data type\. 

For more information about using the AWS CLI to create parameters, see [Walkthrough: Create and Use a Parameter in a Command \(AWS CLI\)](sysman-paramstore-cli.md)\.

**Note**  
Parameters are only available in the Region where they were created\.


+ [Create a `String` or `StringList` Parameter \(AWS CLI\)](#param-create-cli-string-stringlist)
+ [Create a `SecureString` Parameter \(AWS CLI\)](#param-create-cli-securestring)

### Create a `String` or `StringList` Parameter \(AWS CLI\)<a name="param-create-cli-string-stringlist"></a>

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
   aws ssm put-parameter --name "a_name" --value "a value, or a comma-separated list of values" --type String or StringList 
   ```

   If successful, the command has no output\.

   Here is an example that uses the StringList data type\.

   ```
   aws ssm put-parameter --name /IAD/ERP/Oracle/addUsers --value "Milana,Mariana,Mark,Miguel" --type StringList
   ```
**Note**  
Items in a `StringList` must be separated by a comma \(,\)\. You can't use other punctuation or special character to escape items in the list\. If you have a parameter value that requires a comma, then use the `String` data type\.

1. Execute the following command to verify the details of the parameter\.

   ```
   aws ssm get-parameters --name "the name you specified"
   ```

   Here is an example that uses the name specified in the earlier example\.

   ```
   aws ssm get-parameters --name "/IAD/ERP/Oracle/addUsers"
   ```

### Create a `SecureString` Parameter \(AWS CLI\)<a name="param-create-cli-securestring"></a>

Before you create a `SecureString` parameter, read about the requirements for this type of parameter\. For more information, see [Use Secure String Parameters](sysman-paramstore-about.md#sysman-paramstore-securestring)\.

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in Amazon Elastic Compute Cloud \(Amazon EC2\), or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-setting-up.md#systems-manager-prereqs)\.

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
   aws ssm put-parameter --name "a_name" --value "a value" --type SecureString  --key-id "a KMS CMK ID, a KMS CMK ARN, an alias name, or an alias ARN"
   ```
**Note**  
To use the default AWS KMS CMK assigned to your account, remove the `key-id` parameter from the command\.

   Here is an example that uses an obfuscated name \(elixir3131\) for a password and a custom AWS KMS key\.

   ```
   aws ssm put-parameter --name /Finance/Payroll/elixir3131 --value "P@sSwW)rd" --type SecureString --key-id arn:aws:kms:us-east-1:123456789012:key/1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d5e
   ```

1. Execute the following command to verify the details of the parameter\.

   ```
   aws ssm get-parameters --name "the name you specified" --with-decryption
   ```
**Note**  
If you don't specify the `with-decryption` parameter, or if you specify the `no-with-decryption` parameter, the command returns an encrypted GUID\.

## Create a Systems Manager Parameter \(Tools for Windows PowerShell\)<a name="param-create-ps"></a>

You can use Tools for Windows PowerShell to create a parameter that uses the `String`, `StringList`, or `SecureString` data type\. 

**Note**  
Parameters are only available in the Region where they were created\.


+ [Create a `String` or `StringList` parameter \(Tools for Windows PowerShell\)](#param-create-ps-string-stringlist)
+ [Create a `SecureString` parameter \(Tools for Windows PowerShell\)](#param-create-ps-securestring)

### Create a `String` or `StringList` parameter \(Tools for Windows PowerShell\)<a name="param-create-ps-string-stringlist"></a>

1. Open AWS Tools for Windows PowerShell and execute the following command to specify your credentials\. You must either have administrator privileges in Amazon EC2, or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-setting-up.md#systems-manager-prereqs)\.

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

   If successful, the command has no output\.
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

### Create a `SecureString` parameter \(Tools for Windows PowerShell\)<a name="param-create-ps-securestring"></a>

Before you create a `SecureString` parameter, read about the requirements for this type of parameter\. For more information, see [Use Secure String Parameters](sysman-paramstore-about.md#sysman-paramstore-securestring)\.

1. Open AWS Tools for Windows PowerShell and execute the following command to specify your credentials\. You must either have administrator privileges in Amazon EC2, or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-setting-up.md#systems-manager-prereqs)\.

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

   If successful, the command has no output\.
**Note**  
To use the default AWS KMS CMK assigned to your account, remove the `-KeyId` parameter from the command\.

   Here is an example that uses an obfuscated name \(elixir3131\) for a password and the user's default KMS CMK\.

   ```
   Write-SSMParameter -Name "/Finance/Payroll/elixir3131" -Value "P@sSwW)rd" -Type "SecureString"
   ```

1. Execute the following command to verify the details of the parameter\.

   ```
   (Get-SSMParameterValue -Name "the name you specified" –WithDecryption $true).Parameters
   ```

## Create a Systems Manager Parameter \(Console\)<a name="param-create-console"></a>

You can use the Amazon EC2 console or AWS Systems Manager console to create a Systems Manager parameter\.

**Note**  
Parameters are only available in the Region where they were created\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To create a parameter \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose **Create parameter**\.

1. For **Name**, type a hierachy and a parameter name\. For example, type `/Test/helloWorld`\.

1. In the **Description** box, type a description that identifies this parameter as a test parameter\.

1. For **Type**, choose **String**, **StringList**, or **SecureString**\.
**Note**  
If you choose **SecureString,** the **KMS Key ID** field appears\. If you don't provide a KMS CMK ID, a KMS CMK ARN, an alias name, or an alias ARN, then the system uses alias/aws/ssm \(Default\), which is the default KMS CMK for Systems Manager\. If you don't want to use this key, then you can choose a custom key\. For more information, see [Use Secure String Parameters](sysman-paramstore-about.md#sysman-paramstore-securestring)\.

1. In the **Value** box, type a value\. For example, type `MyFirstParameter`\. If you chose **Secure String**, the value is masked as you type\.

1. Choose **Create parameter**\. 

1. In the parameters list, choose the name of the parameter you just created\. Verify the details on the **Overview** tab\. If you created a `SecureString` parameter, choose **Show** to view the unencrypted value\.

**To create a parameter \(Amazon EC2 console\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Shared Resources** in the navigation pane, and then choose **Parameter Store**\. 

1. Choose **Create Parameter**\.

1. For **Name**, type a hierachy and a parameter name\. For example, type `/Test/helloWorld`\.

1. In the **Description** box, type a description that identifies this parameter as a test parameter\.

1. For **Type**, choose **String**, **String List**, or **Secure String**\.
**Note**  
If you choose **SecureString,** the **KMS Key ID** field appears\. If you don't provide a KMS CMK ID, a KMS CMK ARN, an alias name, or an alias ARN, then the system uses alias/aws/ssm \(Default\), which is the default KMS CMK for Systems Manager\. If you don't want to use this key, then you can choose a custom key\. For more information, see [Use Secure String Parameters](sysman-paramstore-about.md#sysman-paramstore-securestring)\.

1. In the **Value** box, type a value\. For example, type `MyFirstParameter`\. If you chose **Secure String**, the value is masked as you type\.

1. Choose **Create Parameter**\. After the system creates the parameter, choose **Close**\. 

1. In the parameters list, choose the parameter you just created\. Verify the details on the **Description** tab\. If you created a `SecureString` parameter, choose **Show** to view the unencrypted value\.