# Creating Systems Manager Parameters<a name="sysman-paramstore-su-create"></a>

When you create a parameter, you specify the following information:

+ **Name**: \(Required\) Specify a name to identify your parameter\. 

  Be aware of the following requirements and restrictions for Systems Manager parameter names:

  + Parameter names are case sensitive\.

  + A parameter name must be unique within an AWS Region\. For example, Systems Manager treats the following as separate parameters, if they exist in the same Region:

    + `/CMH/TestParam1`

    + `/TestParam1`

    The following examples are also unique:

    + `/CMH/TestParam1/Logpath1`

    + `/CMH/TestParam1`

    The following examples, if in the same Region, are not unique:

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
**Important**  
If a user has access to a path, then the user can access all levels of that path\. For example, if a user has permission to access path /a, then the user can also access /a/b\. Even if a user has explicitly been denied access in IAM for parameter /a, they can still call the GetParametersByPath API action recursively and view /a/b\.

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

You can create parameters by using the AWS CLI, AWS Tools for Windows PowerShell, the Amazon EC2 console, or the AWS Systems Manager console\.


+ [Create a Systems Manager Parameter \(AWS CLI\)](param-create-cli.md)
+ [Create a Systems Manager Parameter \(Tools for Windows PowerShell\)](param-create-ps.md)
+ [Create a Systems Manager Parameter \(Console\)](param-create-console.md)