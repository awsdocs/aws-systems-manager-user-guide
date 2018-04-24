# Working with Parameter Versions<a name="sysman-paramstore-versions"></a>

When you initially create a parameter, Parameter Store assigns version 1 to that parameter\. When you edit a parameter, Parameter Store automatically iterates the version number by 1\. You can specify a parameter name and a specific version number in API calls and SSM documents\. If you don't specify a version number, the system automatically uses the latest version\.

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
+ [Create a New Parameter Version \(AWS CLI\)](#sysman-paramstore-version-cli)
+ [Create a New Parameter Version \(Windows PowerShell\)](#sysman-paramstore-version-powershell)

## Create a New Parameter Version \(Console\)<a name="sysman-paramstore-version-console"></a>

You can use the Amazon EC2 console or AWS Systems Manager console to create a new version of a parameter\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To create a new parameter version \(AWS Systems Manager\)**

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

**To create a new parameter version \(Amazon EC2 console\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Shared Resources** in the navigation pane, and then choose **Parameter Store**\. 

1. Choose a parameter that you created earlier\. For information about creating a new parameter, see [Creating Systems Manager Parameters](sysman-paramstore-su-create.md)\. 
**Note**  
Parameters are only available in the Region where they were created\. If you don't see a parameter that you want to update, then verify your Region\.

1. Choose **Actions**, **Edit parameter**\.

1. In the **Value** box, type a new value, and then choose **Save parameter**\.

1. In the parameters list, choose the parameter you just updated, and then choose the **History** tab\. Verify that the version number incremented by 1, and verify the new value\.

## Create a New Parameter Version \(AWS CLI\)<a name="sysman-paramstore-version-cli"></a>

Use the following procedure to create a new version of a parameter by using the AWS CLI\.

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in Amazon EC2 or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\. 

   ```
   aws configure
   ```

   The system prompts you to specify the following:

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

1. Execute the following command to list parameters that you can update\.
**Note**  
Parameters are only available in the Region where they were created\. If you don't see a parameter that you want to update, then verify your Region\.

   ```
   aws ssm describe-parameters
   ```

   Note the name of a parameter that you want to update\.

1. Execute the following command to update the parameter value\.

   ```
   aws ssm put-parameter --name "the_parameter_name" --type the_parameter_type --value "the_new_value" --overwrite
   ```

1. Execute the following command to view all versions of the parameter\.

   ```
   aws ssm get-parameter-history --name "the_parameter_name"
   ```

1. Execute the following command to retrieve information about a parameter by version number\.

   ```
   aws ssm get-parameters --names “the_parameter_name:the_version_number" 
   ```

   Here is an example\.

   ```
   aws ssm get-parameters --names “/Production/SQLConnectionString:3" 
   ```

## Create a New Parameter Version \(Windows PowerShell\)<a name="sysman-paramstore-version-powershell"></a>

Use the following procedure to create a new version of a parameter by using the AWS Tools for Windows PowerShell\.

1. Open AWS Tools for Windows PowerShell and execute the following command to specify your credentials\. You must either have administrator privileges in Amazon EC2 or you must have been granted the appropriate permission in IAM\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\.

   ```
   Set-AWSCredentials –AccessKey key_name –SecretKey key_name
   ```

1. Execute the following command to set the Region for your PowerShell session\. The example uses the us\-east\-2 Region\. Systems Manager is currently available in the following [Regions](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region)\.

   ```
   Set-DefaultAWSRegion -Region us-east-2
   ```

1. Execute the following command to list parameters that you can update\.
**Note**  
Parameters are only available in the Region where they were created\. If you don't see a parameter that you want to update, then verify your Region\.

   ```
   Get-SSMParameterList
   ```

   Note the name of a parameter that you want to update\.

1. Execute the following command to update the parameter value\.

   ```
   Write-SSMParameter -Name "the_parameter_name" -Value "the_new_value" -Type "the_parameter_type" -Overwrite $true 
   ```

1. Execute the following command to view all versions of the parameter\.

   ```
   Get-SSMParameterHistory -Name "the_parameter_name"
   ```

1. Execute the following command to retrieve information about a parameter by version number\.

   ```
   (Get-SSMParameterValue -Names "the_parameter_name").Parameters | fl 
   ```