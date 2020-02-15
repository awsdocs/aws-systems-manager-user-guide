# aws:executeScript – Run a script<a name="automation-action-executeScript"></a>

Runs the python or PowerShell script provided using the specified runtime and handler\. \(For PowerShell, handler is not required\.\)

**Input**  
Provide the runtime and handler required to run the provided Python 3\.6, Python 3\.7, or PowerShell Core 6\.0 script\.

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
        "Script": [
            "def script_handler(events, context):",
            "(script commands)"
        ],
        "Attachment": "zip-file-name-1.zip"
    }
}
```

------

Runtime  
The runtime language to be used for executing the provided script\. Currently, aws:executeScript supports Python 3\.6 \(python3\.6\), Python 3\.7 \(python3\.7\), and PowerShell Core 6\.0 \(dotnetcore2\.1\) scripts\.  
Supported values: **python3\.6** \| **python3\.7** \| **dotnetcore2\.1**  
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
An embedded script that you want to run during the automation execution\.  
Type: String  
Required: No \(Python\) \| Yes \(PowerShell\)

Attachment  
The name of a standalone script file or \.zip file that can be invoked by the action\. To invoke a file for python, use the `filename.method_name` format in `Handler`\. For PowerShell, invoke the attachment using and inline script\. Gzip is not supported\.  
Type: String  
Required: No