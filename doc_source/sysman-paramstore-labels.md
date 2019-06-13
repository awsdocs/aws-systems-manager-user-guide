# Labeling Parameters<a name="sysman-paramstore-labels"></a>

A parameter label is a user\-defined alias to help you manage different versions of a parameter\. When you modify a parameter, Systems Manager automatically saves a new version and increments the version number by one\. A label can help you remember the purpose of a parameter version when there are multiple versions\.

For example, let's say you have a parameter called /MyApp/DB/ConnectionString\. The value of the parameter is a connection string to a MySQL server in a local database in a test environment\. After you finish updating the application, you want the parameter to use a connection string for a production database\. You change the value of /MyApp/DB/ConnectionString\. Systems Manager automatically creates version two with the new connection string\. To help you remember the purpose of each version, you attach a label to each parameter\. For version one, you attach the label *Test* and for version two you attach the label *Production*\.

You can move labels from one version of a parameter to another version\. For example, if you create version three of the /MyApp/DB/ConnectionString parameter with a connection string for a new production database, then you can move the *Production* label from parameter two to parameter three\. 

Parameter labels are a lightweight alternative to parameter tags\. Your organization might have strict guidelines for tags that must be applied to different AWS resources\. In contrast, a label is simply a text association for a specific version of a parameter\. 

Similar to tags, you can query parameters by using labels\. You can view a list of specific parameter versions that all use the same label if you query your parameter set by using the [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html) API action, as described later in this section\.

**Label Requirements and Restrictions**

Parameter labels have the following requirements and restrictions:
+ A version of a parameter can have a maximum of 10 labels\.
+ You can't attach the same label to different versions of the same parameter\. For example, if version 1 has the label *Production*, then you can't attach *Production* to version 2\.
+ You can move a label from one version of a parameter to another\.
+ You can't create a label when you create a new parameter\. You must attach a label to a specific version of a parameter\.
+ You can't delete a parameter label\. If you no longer want to use a parameter label, then you must move it to a different version of a parameter\.
+ A label can have a maximum of 100 characters\.
+ Labels can contain letters \(case sensitive\), numbers, periods \(\.\), hyphens \(\-\), or underscores \(\_\)\. 
+ Labels can't begin with a number, "aws," or "ssm" \(not case sensitive\)\. If a label fails to meet these requirements, then the label is not attached to the parameter version and the system displays it in the list of `InvalidLabels`\.

**Topics**
+ [Working With Parameter Labels \(Console\)](#sysman-paramstore-labels-console)
+ [Working With Parameter Labels \(AWS CLI\)](#sysman-paramstore-labels-cli)

## Working With Parameter Labels \(Console\)<a name="sysman-paramstore-labels-console"></a>

This section describes how to perform the following tasks by using the AWS Systems Manager console\.
+ [Create a New Parameter Label](#sysman-paramstore-labels-console-create)
+ [View Labels Attached to a Parameter](#sysman-paramstore-labels-console-view)
+ [Move a Parameter Label](#sysman-paramstore-labels-console-move)

### Create a New Parameter Label<a name="sysman-paramstore-labels-console-create"></a>

The following procedure describes how to attach a label to a specific version of an *existing* parameter by using the Systems Manager console\. You can't attach a label when you create a parameter\.

**To attach a label to a parameter version by using the console**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose the name of a parameter to open the details page for that parameter\.

1. Choose the **History** tab\.

1. Choose the parameter version for which you want to attach a label\.

1. Choose **Attach labels**\.

1. Choose **Add another label**\. 

1. In the text box, enter the label\. To add more labels, choose **Add another label**\. You can attach a maximum of ten labels\.

1. When you are finished attaching labels, choose **Confirm**\.

### View Labels Attached to a Parameter<a name="sysman-paramstore-labels-console-view"></a>

A parameter version can have a maximum of ten labels\. The following procedure describes how to view all labels attached to a parameter version by using the Systems Manager console\.

**To view labels attached to a parameter version by using the console**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose the name of a parameter to open the details page for that parameter\.

1. Choose the **History** tab\.

1. Locate the parameter version for which you want to view all attached labels\. The **Labels** column shows all labels attached to the parameter version\.

### Move a Parameter Label<a name="sysman-paramstore-labels-console-move"></a>

You can't delete a parameter label after you create it\. You can, however, move a label between versions of a parameter\. The following procedure describes how to move a parameter label to a different version of the same parameter by using the Systems Manager console\.

**To move a label to a different parameter version by using the console**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose the name of a parameter to open the details page for that parameter\.

1. Choose the **History** tab\.

1. Choose the parameter version for which you want to move the label\.

1. Choose **Attach labels**\.

1. Choose **Add another label**\. 

1. In the text box, enter the label\. The console notifies you of the label move\.

1. When you are finished, choose **Confirm**\.

## Working With Parameter Labels \(AWS CLI\)<a name="sysman-paramstore-labels-cli"></a>

This section describes how to perform the following tasks by using the AWS CLI\.
+ [Create a New Parameter Label](#sysman-paramstore-labels-cli-create)
+ [View Labels for a Parameter](#sysman-paramstore-labels-cli-view)
+ [View a List of Parameters that are Assigned a Label](#sysman-paramstore-labels-cli-view-param)
+ [Move a Parameter Label](#sysman-paramstore-labels-cli-move)

### Create a New Parameter Label<a name="sysman-paramstore-labels-cli-create"></a>

The following procedure describes how to attach a label to a specific version of an *existing* parameter by using the AWS CLI\. You can't attach a label when you create a parameter\.

**To create a new parameter label**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or Upgrade and then Configure the AWS CLI](getting-started-cli.md)\.

1. Run the following command to view a list of parameters for which you have permission to attach a label\.
**Note**  
Parameters are only available in the Region where they were created\. If you don't see a parameter for which you want to attach a label, then verify your Region\.

   ```
   aws ssm describe-parameters
   ```

   Note the name of a parameter for which you want to attach a label\.

1. Run the following command to view all versions of the parameter\.

   ```
   aws ssm get-parameter-history --name "the_parameter_name"
   ```

   Note the parameter version for which you want to attach a label\.

1. Run the following command to retrieve information about a parameter by version number\.

   ```
   aws ssm get-parameters --names “the_parameter_name:the_version_number" 
   ```

   Here is an example\.

   ```
   aws ssm get-parameters --names “/Production/SQLConnectionString:3" 
   ```

1. Run one of the following commands to attach a label to a version of a parameter\. If you attach multiple labels, then you must separate label names with a space\.

   **Attach a label to the latest version of a parameter**

   ```
   aws ssm label-parameter-version --name parameter_name  --labels label_name
   ```

   **Attach a label to a specific version of a parameter**

   ```
   aws ssm label-parameter-version --name parameter_name --parameter-version version_number --labels label_name
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
   aws ssm get-parameter --name parameter_name:label_name --with-decryption
   ```

   The command returns information like the following:

   ```
   {
       "Parameter": {
           "Version": version_number, 
           "Type": "parameter_type", 
           "Name": "parameter_name", 
           "Value": "parameter_value", 
           "Selector": ":label_name"
       }
   }
   ```
**Note**  
*Selector* in the output is either the version number or the label that you specified in the `Name` input field\.

### View Labels for a Parameter<a name="sysman-paramstore-labels-cli-view"></a>

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
   aws ssm get-parameter-history --name parameter_name --with-decryption
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

### View a List of Parameters that are Assigned a Label<a name="sysman-paramstore-labels-cli-view-param"></a>

You can use the [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html) API action to view a list of all parameters in a path that are assigned a specific label\. 

Run the following command to view a list of parameters in a path that are assigned a specific label\.

```
aws ssm get-parameters-by-path --path parameter_path --parameter-filters Key=Label,Values=label_name,Option=Equals --max-results a_number --with-decryption --recursive
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

### Move a Parameter Label<a name="sysman-paramstore-labels-cli-move"></a>

You can't delete a parameter label after you create it\. You can, however, move a label between versions of a parameter\. The following procedure describes how to move a parameter label to a different version of the same parameter\.

**To move a parameter label**

1. Run the following command to view all versions of the parameter\.

   ```
   aws ssm get-parameter-history --name "the_parameter_name"
   ```

   Note the parameter version for which you want to attach a label\.

1. Run the following command to assign an existing label to a different version of a parameter\.

   ```
   aws ssm label-parameter-version --name parameter_name --parameter-version version_number --labels name_of_existing_label
   ```
**Note**  
If you want to move an existing label to the latest version of a parameter, then remove `--parameter-version` from the command\.