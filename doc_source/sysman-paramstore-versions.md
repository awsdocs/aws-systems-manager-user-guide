# Working with Parameter Versions<a name="sysman-paramstore-versions"></a>

When you initially create a parameter, Parameter Store assigns version `1` to that parameter\. When you edit a parameter, Parameter Store automatically iterates the version number by one\. You can specify a parameter name and a specific version number in API calls and SSM documents\. If you don't specify a version number, the system automatically uses the latest version\.

Parameter versions provide a layer of protection in the event that a parameter is accidentally changed\. You can view the details, including the values, of all versions\. You can also use parameter versions to see how many times a parameter changed over a period of time\.

You can reference specific parameter versions, including older versions, in commands, API calls, and SSM documents by using the following format: \{\{ssm:*parameter\_name:version*\}\}\. Here is an example for a parameter named *RunCommand* specified in an SSM document: 

```
{
   "schemaVersion":"1.2",
   "description":"Run a shell script or specify the commands to run.",
   "parameters":{
      "commands":{
         "type":"StringList",
         "description":"(Required) Specify a shell script or a command to run.",
         "minItems":1,
         "displayType":"textarea",
         "default":"{{ssm:RunCommand:2}}"
      },
      "executionTimeout":{
         "type":"String",
         "default":"3600",
         "description":"(Optional) The time in seconds for a command to complete before it is considered to have failed. Default is 3600 (1 hour). Maximum is 172800 (48 hours).",
         "allowedPattern":"([1-9][0-9]{0,3})|(1[0-9]{1,4})|(2[0-7][0-9]{1,3})|(28[0-7][0-9]{1,2})|(28800)"
      }
   },
   "runtimeConfig":{
      "aws:runShellScript":{
         "properties":[
            {
               "id":"0.aws:runShellScript",
               "runCommand":"{{ commands }}",
               "timeoutSeconds":"{{ executionTimeout }}"
            }
         ]
      }
   }
}
```

The following procedures show you how to edit a parameter and then verify that a new version was created\.

**Topics**
+ [Create a New Parameter Version \(Console\)](#sysman-paramstore-version-console)

## Create a New Parameter Version \(Console\)<a name="sysman-paramstore-version-console"></a>

You can use the Amazon EC2 console or AWS Systems Manager console to create a new version of a parameter\.

**To create a new parameter version**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose the name of a parameter that you created earlier\. For information about creating a new parameter, see [Creating Systems Manager Parameters](sysman-paramstore-su-create.md)\. 
**Note**  
Parameters are only available in the Region where they were created\. If you don't see a parameter that you want to update, then verify your Region\.

1. Choose **Edit**\.

1. In the **Value** box, type a new value, and then choose **Save changes**\.

1. In the parameters list, choose the name of the parameter you just updated, and then view the **History** tab\. On the **Overview** tab, verify that the version number incremented by 1, and verify the new value\.