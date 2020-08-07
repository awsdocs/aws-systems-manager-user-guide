# Working with parameter labels \(AWS CLI\)<a name="sysman-paramstore-labels-cli"></a>

This section describes how to perform the following tasks by using the AWS CLI\.
+ [Create a new parameter label \(CLI\)](#sysman-paramstore-labels-cli-create)
+ [View labels for a parameter \(CLI\)](#sysman-paramstore-labels-cli-view)
+ [View a list of parameters that are assigned a label \(CLI\)](#sysman-paramstore-labels-cli-view-param)
+ [Move a parameter label \(CLI\)](#sysman-paramstore-labels-cli-move)

## Create a new parameter label \(CLI\)<a name="sysman-paramstore-labels-cli-create"></a>

The following procedure describes how to attach a label to a specific version of an *existing* parameter by using the AWS CLI\. You can't attach a label when you create a parameter\.

**To create a new parameter label**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to view a list of parameters for which you have permission to attach a label\.
**Note**  
Parameters are only available in the Region where they were created\. If you don't see a parameter for which you want to attach a label, then verify your Region\.

   ```
   aws ssm describe-parameters
   ```

   Note the name of a parameter for which you want to attach a label\.

1. Run the following command to view all versions of the parameter\.

   ```
   aws ssm get-parameter-history --name "parameter-name"
   ```

   Note the parameter version for which you want to attach a label\.

1. Run the following command to retrieve information about a parameter by version number\.

   ```
   aws ssm get-parameters --names "parameter-name:version-number" 
   ```

   Here is an example\.

   ```
   aws ssm get-parameters --names "/Production/SQLConnectionString:3" 
   ```

1. Run one of the following commands to attach a label to a version of a parameter\. If you attach multiple labels, then you must separate label names with a space\.

   **Attach a label to the latest version of a parameter**

   ```
   aws ssm label-parameter-version --name parameter-name  --labels label-name
   ```

   **Attach a label to a specific version of a parameter**

   ```
   aws ssm label-parameter-version --name parameter-name --parameter-version version-number --labels label-name
   ```

   Here are some examples:

   ```
   aws ssm label-parameter-version --name /config/endpoint --labels production east-region finance
   ```

   ```
   aws ssm label-parameter-version --name /config/endpoint --parameter-version 3 --labels MySQL-test
   ```
**Note**  
If the output shows the label you created in the `InvalidLabels` list, then the label does not meet the requirements described earlier in this topic\. Review the requirements and try again\. If the `InvalidLabels` list is empty, then your label was successfully applied to the version of the parameter\.

1. You can view the details of the parameter by using either a version number or a label name\. Run the following command and specify the label you created in the previous step\.

   ```
   aws ssm get-parameter --name parameter-name:label-name --with-decryption
   ```

   The command returns information like the following:

   ```
   {
       "Parameter": {
           "Version": version-number, 
           "Type": "parameter-type", 
           "Name": "parameter-name", 
           "Value": "parameter-value", 
           "Selector": ":label-name"
       }
   }
   ```
**Note**  
*Selector* in the output is either the version number or the label that you specified in the `Name` input field\.

## View labels for a parameter \(CLI\)<a name="sysman-paramstore-labels-cli-view"></a>

You can use the [GetParameterHistory](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameterHistory.html) API action to view the full history and all labels attached to a specified parameter\. Or, you can use the [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html) API action to view a list of all parameters that are assigned a specific label\. 

**To view labels for a parameter by using the GetParameterHistory API action**

1. Run the following command to view a list of parameters for which you can view labels\.
**Note**  
Parameters are only available in the Region where they were created\. If you don't see a parameter for which you want to move a label, then verify your Region\.

   ```
   aws ssm describe-parameters
   ```

   Note the name of a parameter for which you want to move a label\.

1. Run the following command to view all versions of the parameter\.

   ```
   aws ssm get-parameter-history --name parameter-name --with-decryption
   ```

   The system returns information like the following:

   ```
   {
       "Parameters": [
           {
               "Name": "/Config/endpoint", 
               "LastModifiedDate": 1528932105.382, 
               "Labels": [
                   "Deprecated"
               ], 
               "Value": "MyTestService-June-Release.example.com", 
               "Version": 1, 
               "LastModifiedUser": "arn:aws:iam::123456789012:user/test", 
               "Type": "String"
           }, 
           {
               "Name": "/Config/endpoint", 
               "LastModifiedDate": 1528932111.222, 
               "Labels": [
                   "Current"
               ], 
               "Value": "MyTestService-July-Release.example.com", 
               "Version": 2, 
               "LastModifiedUser": "arn:aws:iam::123456789012:user/test", 
               "Type": "String"
           }
       ]
   }
   ```

## View a list of parameters that are assigned a label \(CLI\)<a name="sysman-paramstore-labels-cli-view-param"></a>

You can use the [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html) API action to view a list of all parameters in a path that are assigned a specific label\. 

Run the following command to view a list of parameters in a path that are assigned a specific label\.

```
aws ssm get-parameters-by-path --path parameter-path --parameter-filters Key=Label,Values=label-name,Option=Equals --max-results a-number --with-decryption --recursive
```

The system returns information like the following\. For this example, the user searched under the /Config path:

```
{
    "Parameters": [
        {
            "Version": 3, 
            "Type": "SecureString", 
            "Name": "/Config/DBpwd", 
            "Value": "MyS@perGr&pass33"
        }, 
        {
            "Version": 2, 
            "Type": "String", 
            "Name": "/Config/DBusername", 
            "Value": "TestUserDB"
        }, 
        {
            "Version": 2, 
            "Type": "String", 
            "Name": "/Config/endpoint", 
            "Value": "MyTestService-July-Release.example.com"
        }
    ]
}
```

## Move a parameter label \(CLI\)<a name="sysman-paramstore-labels-cli-move"></a>

You can't delete a parameter label after you create it\. You can, however, move a label between versions of a parameter\. The following procedure describes how to move a parameter label to a different version of the same parameter\.

**To move a parameter label**

1. Run the following command to view all versions of the parameter\.

   ```
   aws ssm get-parameter-history --name "parameter-name"
   ```

   Note the parameter version for which you want to attach a label\.

1. Run the following command to assign an existing label to a different version of a parameter\.

   ```
   aws ssm label-parameter-version --name parameter-name --parameter-version version-number --labels name-of-existing-label
   ```
**Note**  
If you want to move an existing label to the latest version of a parameter, then remove `--parameter-version` from the command\.