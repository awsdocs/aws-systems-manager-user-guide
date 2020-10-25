# aws:executeScript – Run a script<a name="automation-action-executeScript"></a>

Runs the Python or PowerShell script provided, using the specified runtime and handler\. \(For PowerShell, the handler is not required\.\)

Currently, the `aws:executeScript` action contains the following preinstalled PowerShell Core modules\. 
+ Microsoft\.PowerShell\.Host
+ Microsoft\.PowerShell\.Management
+ Microsoft\.PowerShell\.Security
+ Microsoft\.PowerShell\.Utility
+ PackageManagement
+ PowerShellGet

To use PowerShell Core modules that are not preinstalled, your script must install the module with the `-Force` flag, as shown in the following command\.

```
Install-Module ModuleName -Force
```

To use PowerShell Core cmdlets in your script, we recommend using the `AWS.Tools` modules, as shown in the following commands\. 

**Important**  
Installing the `AWSPowerShell.NetCore` module is not supported\.
+ Amazon S3 cmdlets\.

  ```
  Install-Module AWS.Tools.S3 -Force
  Get-S3Bucket -BucketName bucketname
  ```
+ Amazon EC2 cmdlets\.

  ```
  Install-Module AWS.Tools.EC2 -Force
  Get-EC2InstanceStatus -InstanceId instanceId
  ```
+ Common, or service independent AWS Tools for Windows PowerShell cmdlets\.

  ```
  Install-Module AWS.Tools.Common -Force
  Get-AWSRegion
  ```

If your script initializes new objects in addition to using PowerShell Core cmdlets, you must also import the module as shown in the following command\.

```
Install-Module AWS.Tools.EC2 -Force
Import-Module AWS.Tools.EC2

$tag = New-Object Amazon.EC2.Model.Tag
$tag.Key = "myTag"
$tag.Value = "myTagValue"

New-EC2Tag -Resource i-12345678 -Tag $tag
```

For examples of installing and importing `AWS.Tools` modules, and using PowerShell Core cmdlets in Automation document content, see [ Walkthrough: Using Document Builder to create a custom Automation document](automation-walk-document-builder.md)\.

**Note**  
Each `aws:executeScript` action can run up to a maximum duration of 600 seconds \(ten minutes\)\. You can limit the timeout by specifying the `timeoutSeconds` parameter for an `aws:executeScript` step\.

**Input**  
Provide the runtime and handler required to run the provided Python 3\.6, Python 3\.7, or PowerShell Core 6\.0 script\.

**Important**  
The script input parameter is not supported for JSON documents\. JSON documents must provide script content using the attachment input parameter\.

------
#### [ YAML ]

```
action: "aws:executeScript"
inputs: 
 Runtime: "python3.6"
 Handler: "script_handler"
 InputPayload: 
  "parameter1": "parameter_value1"
  "parameter2": "parameter_value2"
 Script: 
  - 
   "def script_handler(events, context):"
  - 
   "(script commands)"
 Attachment: "zip-file-name-1.zip"
```

------
#### [ JSON ]

```
{
    "action": "aws:executeScript",
    "inputs": {
        "Runtime": "python3.6",
        "Handler": "script_handler",
        "InputPayload": {
            "parameter1": "parameter_value1",
            "parameter2": "parameter_value2"
        },
        "Attachment": "zip-file-name-1.zip"
    }
}
```

------

Runtime  
The runtime language to be used for executing the provided script\. Currently, aws:executeScript supports Python 3\.6 \(python3\.6\), Python 3\.7 \(python3\.7\), and PowerShell Core 6\.0 \(dotnetcore2\.1\) scripts\.  
Supported values: **python3\.6** \| **python3\.7** \| **PowerShell Core 6\.0**  
Type: String  
Required: Yes

Handler  
The entry for script execution, usually a function name\. You must ensure the function defined in the handler has two parameters, `events` and `context`\. \(Not required for PowerShell\.\)  
Type: String  
Required: Yes \(Python\) \| No \(PowerShell\)

InputPayload  
A JSON or YAML object that will be passed to the first parameter of the handler\. This can be used to pass input data to the script\.  
Type: String  
Required: No

Script  
An embedded script that you want to run during the automation execution\. \(Not supported for JSON documents\.\)  
Type: String  
Required: No \(Python\) \| Yes \(PowerShell\)

Attachment  
The name of a standalone script file or \.zip file that can be invoked by the action\. To invoke a file for Python, use the `filename.method_name` format in `Handler`\. For PowerShell, invoke the attachment using and inline script\. Gzip is not supported\.  
When including Python libraries in your attachment, we recommend adding an empty `__init__.py` file in each module directory\. This enables you to import the modules from the library in your attachment within your script content\. For example: `from library import module`  
Type: String  
Required: NoOutput

Payload  
The JSON representation of the object returned by your function\.