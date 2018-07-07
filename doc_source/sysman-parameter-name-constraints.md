# Requirements and Constraints for Parameter Names<a name="sysman-parameter-name-constraints"></a>

Use the information in this topic to help you specify valid values for parameter names when you create a parameter\. 

This information supplements the details in the topic [PutParameter](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutParameter.html) in the *AWS Systems Manager API Reference*, which also provides information about the values **AllowedPattern**, **Description**, **KeyId**, **Overwrite**, **Type**, and **Value**\.

The requirements and constraints for parameter names include the following:
+ **Case sensivity**: Parameter names are case sensitive\.
+ **Spaces**: Parameter names can't include spaces\.
+ **Valid characters**: Parameter names can consist of the following symbols and letters only: `a-zA-Z0-9_.-/`
+ **Prefixes**: A parameter name *cannot* be prefixed with "aws" or "ssm" \(case\-insensitive\)\. For example, attempts to create parameters with the following names will fail with an exception:
  + awsTestParameter
  + SSM\-testparameter
  + /aws/testparam1
**Note**  
When you specify a parameter in an SSM document, command, or script, you do include `ssm` as part of the syntax, as shown in the following examples\. Note that there is no space between brackets\.   
Valid: \{\{ssm:*parameter\_name*\}\} and \{\{ ssm:*parameter\_name* \}\}, such as `{{ssm:addUsers}}`, and `{{ssm:addUsers }}`, 
Invalid: `{{ssm:ssmAddUsers}}`
+ **Uniquenes**: A parameter name must be unique within an AWS Region\. For example, Systems Manager treats the following as separate parameters, if they exist in the same Region:
  + `/Test/TestParam1`
  + `/TestParam1`

  The following examples are also unique:
  + `/Test/TestParam1/Logpath1`
  + `/Test/TestParam1`

  The following examples, however, if in the same Region, are not unique:
  + `/TestParam1`
  + `TestParam1`
+ **Hierarchy depth**: If you specify a parameter hierarchy, the hierarchy can have a maximum depth of fifteen levels\. You can define a parameter at any level of the hierarchy\. Both of the following examples are structurally valid:
  + `/Level-1/L2/L3/L4/L5/L6/L7/L8/L9/L10/L11/L12/L13/L14/parameter-name`
  + `parameter-name`

  Attempting to create the following parameter would fail with a `HierarchyLevelLimitExceededException` exception:
  + `/Level-1/L2/L3/L4/L5/L6/L7/L8/L9/L10/L11/L12/L13/L14/L15/L16/parameter-name`

**Important**  
If a user has access to a path, then the user can access all levels of that path\. For example, if a user has permission to access path /a, then the user can also access /a/b\. Even if a user has explicitly been denied access in IAM for parameter /a, they can still call the [ GetParametersByPath](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html) API action recursively and view /a/b\.