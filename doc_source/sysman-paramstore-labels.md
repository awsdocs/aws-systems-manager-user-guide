# Working with parameter labels<a name="sysman-paramstore-labels"></a>

A parameter label is a user\-defined alias to help you manage different versions of a parameter\. When you modify a parameter, AWS Systems Manager automatically saves a new version and increments the version number by one\. A label can help you remember the purpose of a parameter version when there are multiple versions\.

For example, let's say you have a parameter called /MyApp/DB/ConnectionString\. The value of the parameter is a connection string to a MySQL server in a local database in a test environment\. After you finish updating the application, you want the parameter to use a connection string for a production database\. You change the value of /MyApp/DB/ConnectionString\. Systems Manager automatically creates version two with the new connection string\. To help you remember the purpose of each version, you attach a label to each parameter\. For version one, you attach the label *Test* and for version two you attach the label *Production*\.

You can move labels from one version of a parameter to another version\. For example, if you create version 3 of the `/MyApp/DB/ConnectionString` parameter with a connection string for a new production database, then you can move the *Production* label from version 2 of the parameter to version 3 of the parameter\.

Parameter labels are a lightweight alternative to parameter tags\. Your organization might have strict guidelines for tags that must be applied to different AWS resources\. In contrast, a label is simply a text association for a specific version of a parameter\. 

Similar to tags, you can query parameters by using labels\. You can view a list of specific parameter versions that all use the same label if you query your parameter set by using the [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html) API operation, as described later in this section\.

**Label requirements and restrictions**

Parameter labels have the following requirements and restrictions:
+ A version of a parameter can have a maximum of 10 labels\.
+ You can't attach the same label to different versions of the same parameter\. For example, if version 1 of the parameter has the label *Production*, then you can't attach *Production* to version 2\.
+ You can move a label from one version of a parameter to another\.
+ You can't create a label when you create a parameter\. You must attach a label to a specific version of a parameter\.
+ If you no longer want to use a parameter label, then you can move it to a different version of a parameter or delete it\.
+ A label can have a maximum of 100 characters\.
+ Labels can contain letters \(case sensitive\), numbers, periods \(\.\), hyphens \(\-\), or underscores \(\_\)\. 
+ Labels can't begin with a number, "aws", or "ssm" \(not case sensitive\)\. If a label doesn't meet these requirements, then the label isn't attached to the parameter version and the system displays it in the list of `InvalidLabels`\.

**Topics**
+ [Working with parameter labels \(console\)](#sysman-paramstore-labels-console)
+ [Working with parameter labels \(AWS CLI\)](#sysman-paramstore-labels-cli)

## Working with parameter labels \(console\)<a name="sysman-paramstore-labels-console"></a>

This section describes how to perform the following tasks by using the Systems Manager console\.
+ [Create a parameter label \(console\)](#sysman-paramstore-labels-console-create)
+ [View labels attached to a parameter \(console\)](#sysman-paramstore-labels-console-view)
+ [Move a parameter label \(console\)](#sysman-paramstore-labels-console-move)
+ [Delete parameter labels \(console\)](#systems-manager-parameter-store-labels-console-delete)

### Create a parameter label \(console\)<a name="sysman-paramstore-labels-console-create"></a>

The following procedure describes how to attach a label to a specific version of an *existing* parameter by using the Systems Manager console\. You can't attach a label when you create a parameter\.

**To attach a label to a parameter version**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose the name of a parameter to open the details page for that parameter\.

1. Choose the **History** tab\.

1. Choose the parameter version for which you want to attach a label\.

1. Choose **Manage labels**\.

1. Choose **Add new label**\. 

1. In the text box, enter the label name\. To add more labels, choose **Add new label**\. You can attach a maximum of ten labels\.

1. When you're finished, choose **Save changes**\. 

### View labels attached to a parameter \(console\)<a name="sysman-paramstore-labels-console-view"></a>

A parameter version can have a maximum of ten labels\. The following procedure describes how to view all labels attached to a parameter version by using the Systems Manager console\.

**To view labels attached to a parameter version**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose the name of a parameter to open the details page for that parameter\.

1. Choose the **History** tab\.

1. Locate the parameter version for which you want to view all attached labels\. The **Labels** column shows all labels attached to the parameter version\.

### Move a parameter label \(console\)<a name="sysman-paramstore-labels-console-move"></a>

The following procedure describes how to move a parameter label to a different version of the same parameter by using the Systems Manager console\.

**To move a label to a different parameter version**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose the name of a parameter to open the details page for that parameter\.

1. Choose the **History** tab\.

1. Choose the parameter version for which you want to move the label\.

1. Choose **Manage labels**\.

1. Choose **Add new label**\. 

1. In the text box, enter the label name\.

1. When you're finished, choose **Save changes**\. 

### Delete parameter labels \(console\)<a name="systems-manager-parameter-store-labels-console-delete"></a>

The following procedure describes how to delete one or multiple parameter labels using the Systems Manager console\.

**To delete labels from a parameter**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose the name of a parameter to open the details page for that parameter\.

1. Choose the **History** tab\.

1. Choose the parameter version for which you want to delete labels\.

1. Choose **Manage labels**\.

1. Choose **Remove**\. next to each label you want to delete\. 

1. When you're finished, choose **Save changes**\.

1. Confirm that your changes are correct, enter `Confirm` in the text box, and choose **Confirm**\.

## Working with parameter labels \(AWS CLI\)<a name="sysman-paramstore-labels-cli"></a>

This section describes how to perform the following tasks by using the AWS Command Line Interface \(AWS CLI\)\.
+ [Create a new parameter label \(AWS CLI\)](#sysman-paramstore-labels-cli-create)
+ [View labels for a parameter \(AWS CLI\)](#sysman-paramstore-labels-cli-view)
+ [View a list of parameters that are assigned a label \(AWS CLI\)](#sysman-paramstore-labels-cli-view-param)
+ [Move a parameter label \(AWS CLI\)](#sysman-paramstore-labels-cli-move)
+ [Delete parameter labels \(AWS CLI\)](#systems-manager-parameter-store-labels-cli-delete)

### Create a new parameter label \(AWS CLI\)<a name="sysman-paramstore-labels-cli-create"></a>

The following procedure describes how to attach a label to a specific version of an *existing* parameter by using the AWS CLI\. You can't attach a label when you create a parameter\.

**To create a parameter label**

1. Install and configure the AWS Command Line Interface \(AWS CLI\), if you haven't already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to view a list of parameters for which you have permission to attach a label\.
**Note**  
Parameters are only available in the AWS Region where they were created\. If you don't see a parameter for which you want to attach a label, then verify your Region\.

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

1. Run one of the following commands to attach a label to a version of a parameter\. If you attach multiple labels, separate label names with a space\.

   **Attach a label to the latest version of a parameter**

   ```
   aws ssm label-parameter-version --name parameter-name  --labels label-name
   ```

   **Attach a label to a specific version of a parameter**

   ```
   aws ssm label-parameter-version --name parameter-name --parameter-version version-number --labels label-name
   ```

   Here are some examples\.

   ```
   aws ssm label-parameter-version --name /config/endpoint --labels production east-region finance
   ```

   ```
   aws ssm label-parameter-version --name /config/endpoint --parameter-version 3 --labels MySQL-test
   ```
**Note**  
If the output shows the label you created in the `InvalidLabels` list, then the label doesn't meet the requirements described earlier in this topic\. Review the requirements and try again\. If the `InvalidLabels` list is empty, then your label was successfully applied to the version of the parameter\.

1. You can view the details of the parameter by using either a version number or a label name\. Run the following command and specify the label you created in the previous step\.

   ```
   aws ssm get-parameter --name parameter-name:label-name --with-decryption
   ```

   The command returns information like the following\.

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

### View labels for a parameter \(AWS CLI\)<a name="sysman-paramstore-labels-cli-view"></a>

You can use the [GetParameterHistory](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameterHistory.html) API operation to view the full history and all labels attached to a specified parameter\. Or, you can use the [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html) API operation to view a list of all parameters that are assigned a specific label\. 

**To view labels for a parameter by using the GetParameterHistory API operation**

1. Run the following command to view a list of parameters for which you can view labels\.
**Note**  
Parameters are only available in the Region where they were created\. If you don't see a parameter for which you want to move a label, then verify your Region\.

   ```
   aws ssm describe-parameters
   ```

   Note the name of the parameter you want to view the labels of\.

1. Run the following command to view all versions of the parameter\.

   ```
   aws ssm get-parameter-history --name parameter-name --with-decryption
   ```

   The system returns information like the following\.

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

### View a list of parameters that are assigned a label \(AWS CLI\)<a name="sysman-paramstore-labels-cli-view-param"></a>

You can use the [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html) API operation to view a list of all parameters in a path that are assigned a specific label\. 

Run the following command to view a list of parameters in a path that are assigned a specific label\. Replace each *example resource placeholder* with your own information\.

```
aws ssm get-parameters-by-path \
    --path parameter-path \
    --parameter-filters Key=Label,Values=label-name,Option=Equals \
    --max-results a-number \
    --with-decryption --recursive
```

The system returns information like the following\. For this example, the user searched under the `/Config` path\.

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

### Move a parameter label \(AWS CLI\)<a name="sysman-paramstore-labels-cli-move"></a>

The following procedure describes how to move a parameter label to a different version of the same parameter\.

**To move a parameter label**

1. Run the following command to view all versions of the parameter\. Replace *parameter name* with your own information\.

   ```
   aws ssm get-parameter-history \
       --name "parameter name"
   ```

   Note the parameter versions you want to move the label to and from\.

1. Run the following command to assign an existing label to a different version of a parameter\. Replace each *example resource placeholder* with your own information\.

   ```
   aws ssm label-parameter-version \
       --name parameter name \
       --parameter-version version number \
       --labels name-of-existing-label
   ```
**Note**  
If you want to move an existing label to the latest version of a parameter, then remove `--parameter-version` from the command\.

### Delete parameter labels \(AWS CLI\)<a name="systems-manager-parameter-store-labels-cli-delete"></a>

The following procedure describes how to delete parameter labels by using the AWS CLI\.

**To delete a parameter label**

1. Run the following command to view all versions of the parameter\. Replace *parameter name* with your own information\.

   ```
   aws ssm get-parameter-history \
       --name "parameter name"
   ```

   The system returns information like the following\.

   ```
   {
       "Parameters": [
           {
               "Name": "foo",
               "DataType": "text",
               "LastModifiedDate": 1607380761.11,
               "Labels": [
                   "l3",
                   "l2"
               ],
               "Value": "test",
               "Version": 1,
               "LastModifiedUser": "arn:aws:iam::123456789012:user/test",
               "Policies": [],
               "Tier": "Standard",
               "Type": "String"
           },
           {
               "Name": "foo",
               "DataType": "text",
               "LastModifiedDate": 1607380763.11,
               "Labels": [
                   "l1"
               ],
               "Value": "test",
               "Version": 2,
               "LastModifiedUser": "arn:aws:iam::123456789012:user/test",
               "Policies": [],
               "Tier": "Standard",
               "Type": "String"
           }
       ]
   }
   ```

   Note the parameter version for which you want to delete a label or labels\.

1. Run the following command to delete the labels you choose from that parameter\. Replace each *example resource placeholder* with your own information\.

   ```
   aws ssm unlabel-parameter-version \
       --name parameter name \
       --parameter-version version \
       --labels label 1,label 2,label 3
   ```

   The system returns information like the following\.

   ```
   {
       "InvalidLabels": ["invalid"], 
       "DeletedLabels" : ["Prod"]
    }
   ```