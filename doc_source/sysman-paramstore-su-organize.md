# Organizing Parameters into Hierarchies<a name="sysman-paramstore-su-organize"></a>

Managing dozens or hundreds of parameters as a flat list is time consuming and prone to errors\. It can also be difficult to identify the correct parameter for a task\. This means you might accidentally use the wrong parameter, or you might create multiple parameters that use the same configuration data\. 

You can use parameter hierarchies to help you organize and manage parameters\. A hierarchy is a parameter name that includes a path that you define by using forward slashes\. 

**Important**  
Only the value of a secure string parameter is encrypted\. Parameter names, descriptions, and other properties are not encrypted\. Therefore, to make it less obvious which parameters contain passwords, we recommend using a naming system that avoids the actual word "password" in your parameter names\. For illustration, however, we are using the sample parameter name *my\-password* in our examples\.

Here is an example that uses three hierarchy levels in the name to identify the following:

/Environment/Type of computer/Application/Data

```
/Dev/DBServer/MySQL/db-string13
```

You can create a hierarchy with a maximum of 15 levels\. We suggest that you create hierarchies that reflect an existing hierarchical structure in your environment, as shown in the following examples:
+ Your [Continuous integration](https://aws.amazon.com//devops/continuous-integration/) and [Continuous delivery](https://aws.amazon.com/devops/continuous-delivery/) environment \(CI/CD workflows\)

  ```
  /Dev/DBServer/MySQL/db-string
  ```

  ```
  /Staging/DBServer/MySQL/db-string
  ```

  ```
  /Prod/DBServer/MySQL/db-string
  ```
+ Your applications that use containers

  ```
  /MyApp/.NET/Libraries/my-password
  ```
+ Your business organization

  ```
  /Finance/Accountants/UserList
  ```

  ```
  /Finance/Analysts/UserList
  ```

  ```
  /HR/Employees/EU/UserList
  ```

Parameter hierarchies standardize the way you create parameters and make it easier to manage parameters over time\. A parameter hierarchy can also help you identify the correct parameter for a configuration task\. This helps you to avoid creating multiple parameters with the same configuration data\. 

You can create a hierarchy that allows you to share parameters across different environments, as shown in the following examples that use passwords in development and staging environment\.

```
/DevTest/MyApp/database/my-password
```

You could then create a unique password for your production environment, as shown in the following example:

```
/prod/MyApp/database/my-password
```

You are not required to specify a parameter hierarchy\. You can create parameters at level one\. These are called root parameters\. For backward compatibility, all parameters created in Parameter Store before hierarchies were released are root parameters\. The systems treats both of the following parameters as root parameters\.

`/parameter-name`

`parameter-name`

For an example of how to work with parameter hierarchies, see [Walkthrough: Manage Parameters Using Hierarchies \(AWS CLI\)](sysman-paramstore-walk-hierarchies.md)\.

**Querying Parameters in a Hierarchy**  
Another benefit of using hierarchies is the ability to query for all parameters within a hierarchy by using the [GetParametersByPath](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html) API action\. For example, if you execute the following command from the AWS CLI, the system returns all parameters in the IIS level\.

```
aws ssm get-parameters-by-path --path /Dev/Web/IIS
```

To view decrypted SecureString parameters in a hierarchy, you specify the path and the `--with-decryption` parameter, as shown in the following example\.

```
aws ssm get-parameters-by-path --path /Prod/ERP/SAP --with-decryption
```

**Restricting IAM Permissions Using Hierarchies**  
Using hierarchies and AWS Identity and Access Management \(IAM\) policies for Parameter Store API actions, you can provide or restrict access to all parameters in one level of a hierarchy\. The following example policy allows all Parameter Store operations on all parameters for the AWS account 123456789012 in the US East \(Ohio\) Region \(us\-east\-2\)\. The user can't create parameters because the `PutParameter` action is explicitly denied\. This policy also forbids the user from calling the `GetParametersByPath` action\. 

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:*"
            ],
            "Resource": "arn:aws:ssm:us-east-2:123456789012:parameter/*"
        },
        {
            "Effect": "Deny",
            "Action": [
                "ssm:GetParametersByPath"
            ],
            "Condition": {
                "StringEquals": {
                    "ssm:Recursive": [
                        "true"
                    ]
                }
            },
            "Resource": "arn:aws:ssm:us-east-2:123456789012:parameter/Dev/ERP/Oracle/*"
        },
        {
            "Effect": "Deny",
            "Action": [
                "ssm:PutParameter"
            ],
            "Condition": {
                "StringEquals": {
                    "ssm:Overwrite": [
                        "false"
                    ]
                }
            },
            "Resource": "arn:aws:ssm:us-east-2:123456789012:parameter/*"
        }
    ]
}
```

**Important**  
If a user has access to a path, then the user can access all levels of that path\. For example, if a user has permission to access path /a, then the user can also access /a/b\. Even if a user has explicitly been denied access in IAM for parameter /a, they can still call the GetParametersByPath API action recursively and view /a/b\.