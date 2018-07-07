# Automation System Variables<a name="automation-variables"></a>

Systems Manager Automation documents use the following variables\. For an example of how these variables are used, view the JSON source of the AWS\-UpdateWindowsAmi document\. 

**Note**  
The following procedure describes steps that you perform in the Amazon EC2 console\. You can also perform these steps in the new [AWS Systems Manager console](https://console.aws.amazon.com/systems-manager/)\. The steps in the new console will differ from the steps below\.

**To view the JSON source of the AWS\-UpdateWindowsAmi document**

1. In the Amazon EC2 console, expand **Systems Manager Shared Resources**, and then choose **Documents**\.

1. Choose **AWS\-UpdateWindowsAmi**\. 

1. In the lower pane, choose the **Content** tab\.

**System Variables**  
Automation documents currently support the following system variables\.


****  

| Variable | Details | 
| --- | --- | 
|  global:ACCOUNT\_ID  |  The AWS account ID of the AWS Identity and Access Management \(IAM\) user or role in which Automation runs\.  | 
|  global:DATE  |  The date \(at execution time\) in the format yyyy\-MM\-dd\.  | 
|  global:DATE\_TIME  |  The date and time \(at execution time\) in the format yyyy\-MM\-dd\_HH\.mm\.ss\.  | 
|  global:REGION  |  The region which the document is run in\. For example, us\-east\-2\.  | 

**Automation Variables**  
Automation documents currently support the following automation variables\.


****  

| Variable | Details | 
| --- | --- | 
|  automation:EXECUTION\_ID  |  The unique identifier assigned to the current automation execution\. For example 1a2b3c\-1a2b3c\-1a2b3c\-1a2b3c1a2b3c1a2b3c\.  | 

**Topics**
+ [Terminology](#automation-terms)
+ [Supported Scenarios](#automation-variables-support)
+ [Unsupported Scenarios](#automation-variables-unsupported)

## Terminology<a name="automation-terms"></a>

The following terms describe how variables and parameters are resolved\.


****  

| Term | Definition | Example | 
| --- | --- | --- | 
|  Constant ARN  |  A valid ARN without variables  |  arn:aws:iam::123456789012:role/roleName  | 
|  Document Parameter  |  A parameter defined at the document level for an Automation document \(for example, instanceId\)\. The parameter is used in a basic string replace\. Its value is supplied at Start Execution time\.  |  <pre>{ <br />   "description": "Create Image Demo",<br />   "version": "0.3",<br />   "assumeRole": "Your_Automation_Assume_Role_ARN",<br />   "parameters":{ <br />      "instanceId": { <br />         "type": "STRING",<br />         "description": "Instance to create image from"<br />   }<br />}</pre>  | 
|  System variable  |  A general variable substituted into the document when any part of the document is evaluated\.  |  <pre>"activities": [ <br />   { <br />      "id": "copyImage",<br />      "activityType": "AWS-CopyImage",<br />      "maxAttempts": 1,<br />      "onFailure": "Continue",<br />      "inputs": { <br />         "ImageName": "{{imageName}}",<br />         "SourceImageId": "{{sourceImageId}}",<br />         "SourceRegion": "{{sourceRegion}}",<br />         "Encrypted": true,<br />         "ImageDescription": "Test CopyImage Description created on {{global:DATE}}"<br />      }<br />   }<br />]</pre>  | 
|  Automation variable  |  A variable relating to the automation execution substituted into the document when any part of the document is evaluated\.  |  <pre>{ <br />   "name": "runFixedCmds",<br />   "action": "aws:runCommand",<br />   "maxAttempts": 1,<br />   "onFailure": "Continue",<br />   "inputs": { <br />      "DocumentName": "AWS-RunPowerShellScript",<br />      "InstanceIds": [ <br />         "{{LaunchInstance.InstanceIds}}"<br />      ],<br />      "Parameters": { <br />         "commands": [ <br />            "dir",<br />            "date",<br />            "echo {Hello {{ssm:administratorName}}}",<br />            "“{{outputFormat}}” -f “left”,”right”,”{{global:DATE}}”,”{{automation:EXECUTION_ID}}”,”{{global:TIME}}”"<br />         ]<br />      }<br />   }<br />}</pre>  | 
|  SSM parameter  |  A variable defined within the Parameter Service\. It is not declared as a Document Parameter\. It may require permissions to access\.  |  <pre>{ <br />   "description": "Run Command Demo",<br />   "schemaVersion": "0.3",<br />   "assumeRole": "arn:aws:iam::123456789012:role/roleName",<br />   "parameters": { <br />      "commands": { <br />         "type": "STRING_LIST",<br />         "description": "list of commands to run as part of first step"<br />      },<br />      "instanceIds": { <br />         "type": "STRING_LIST",<br />         "description": "list of instances to run commands on"<br />      }<br />   },<br />   "mainSteps": [ <br />      { <br />         "name": "runFixedCmds",<br />         "action": "aws:runCommand",<br />         "maxAttempts": 1,<br />         "onFailure": "Continue",<br />         "inputs": { <br />            "DocumentName": "AWS-RunPowerShellScript",<br />            "InstanceIds": [ <br />               "{{LaunchInstance.InstanceIds}}"<br />            ],<br />            "Parameters": { <br />               "commands": [ <br />                  "dir",<br />                  "date",<br />                  "echo {Hello {{ssm:administratorName}}}",<br />                  ""{{outputFormat}}" -f "left","right","{{global:DATE}}","{{automation:EXECUTION_ID}}","{{global:TIME}}""<br />               ]<br />            }<br />         }<br />}</pre>  | 

## Supported Scenarios<a name="automation-variables-support"></a>


****  

| Scenario | Comments | Example | 
| --- | --- | --- | 
|  Constant ARN assumeRole at create  |  An authorization check will be performed to check the calling user is permitted to pass the given assume role\.  |  <pre>{<br />  "description": "Test all Automation resolvable parameters",<br />  "schemaVersion": "0.3",<br />  "assumeRole": "arn:aws:iam::123456789012:role/roleName",<br />  "parameters": { <br />  ...</pre>  | 
|  Document Parameter supplied for assumeRole at create  |  Must be defined in the Parameter list of the document\.  |  <pre>{<br />  "description": "Test all Automation resolvable parameters",<br />  "schemaVersion": "0.3",<br />  "assumeRole": "{{dynamicARN}}",<br />  "parameters": {<br /> ...</pre>  | 
|  Value supplied for Document Parameter at start\.  |  Customer supplies the value to use for a parameter\. Any execution inputs supplied at start time need to be defined in the parameter list of the document\.  |  <pre>...<br />"parameters": {<br />    "amiId": {<br />      "type": "STRING",<br />      "default": "ami-7f2e6015",<br />      "description": "list of commands to run as part of first step"<br />    },<br />...</pre> Inputs to Start Automation Execution include : \{"amiId" : \["ami\-12345678"\] \}  | 
|  SSM parameter referenced within step definition  |  The variable exists within the customers account and the assumeRole for the document has access to the variable\. A check will be performed at create time to confirm the assumeRole has access\. SSM parameters do not need to be set in the parameter list of the document\.  |  <pre>...<br />"mainSteps": [<br />    {<br />      "name": "RunSomeCommands",<br />      "action": "aws:runCommand",<br />      "maxAttempts": 1,<br />      "onFailure": "Continue",<br />      "inputs": {<br />        "DocumentName": "AWS:RunPowerShell",<br />        "InstanceIds": [{{LaunchInstance.InstanceIds}}],<br />        "Parameters": {<br /><br />"commands" : [<br /><br />"echo {Hello {{ssm:administratorName}}}"<br /><br />]<br /><br />       }<br />      }<br />    },<br /><br />... </pre>  | 
|  System variable referenced within step definition  |  A system variable is substituted into the document at execution time\. The value injected into the document is relative to when the substitution occurs\. e\.g\. The value of a time variable injected at step 1 will be different to the value injected at step 3 due to the time taken to run the steps between\. System variables do not need to be set in the parameter list of the document\.  |  <pre>...<br />  "mainSteps": [<br />    {<br />      "name": "RunSomeCommands",<br />      "action": "aws:runCommand",<br />      "maxAttempts": 1,<br />      "onFailure": "Continue",<br />      "inputs": {<br />        "DocumentName": "AWS:RunPowerShell",<br />        "InstanceIds": [{{LaunchInstance.InstanceIds}}],<br />        "Parameters": {<br /><br />"commands" : [<br /><br />"echo {The time is now {{global:TIME}}}"<br /><br />]<br /><br />       }<br />      }<br />    }, ... </pre>  | 
|  Automation variable referenced within step definition\.  |  Automation variables do not need to be set in the parameter list of the document\. The only supported Automation variable is **automation:EXECUTION\_ID**\.  |  <pre>...<br />"mainSteps": [<br />    {<br />      "name": "invokeLambdaFunction",<br />      "action": "aws:invokeLambdaFunction",<br />      "maxAttempts": 1,<br />      "onFailure": "Continue",<br />      "inputs": {<br />        "FunctionName": "Hello-World-LambdaFunction",<br /><br />"Payload" : "{ "executionId" : "{{automation:EXECUTION_ID}}" }"<br />      }<br />    }<br />... </pre>  | 
|  Refer to output from previous step within next step definition\.  |  This is parameter redirection\. The output of a previous step is referenced using the syntax \{\{stepName\.OutputName\}\}\. This syntax cannot be used by the customer for Document Parameters\. This is resolved at the time of execution for the referring step\. The parameter is not listed in the parameters of the document\.  |  <pre>...<br />"mainSteps": [<br />    {<br />      "name": "LaunchInstance",<br />      "action": "aws:runInstances",<br />      "maxAttempts": 1,<br />      "onFailure": "Continue",<br />      "inputs": {<br />        "ImageId": "{{amiId}}",<br />        "MinInstanceCount": 1,<br />        "MaxInstanceCount": 2<br />      }<br />    },<br />    {<br />      "name":"changeState",<br />      "action": "aws:changeInstanceState",<br />      "maxAttempts": 1,<br />      "onFailure": "Continue",<br />      "inputs": {<br />        "InstanceIds": ["{{LaunchInstance.InstanceIds}}"],<br />        "DesiredState": "terminated"<br />      }<br />    }<br /><br />... </pre>  | 

## Unsupported Scenarios<a name="automation-variables-unsupported"></a>


****  

| Scenario | Comment | Example | 
| --- | --- | --- | 
|  SSM Parameter supplied for assumeRole at create  |  Not supported\.  |  <pre>...<br /><br />{<br />  "description": "Test all Automation resolvable parameters",<br />  "schemaVersion": "0.3",<br />  "assumeRole": "{{ssm:administratorRoleARN}}",<br />  "parameters": {<br /><br />... </pre>  | 
|  SSM Parameter supplied for Document Parameter at start  |  The user supplies an input parameter at start time which is an SSM parameter  |  <pre>...<br /><br />"parameters": {<br />    "amiId": {<br />      "type": "STRING",<br />      "default": "ami-7f2e6015",<br />      "description": "list of commands to run as part of first step"<br />    },<br /><br />...<br /><br />User supplies input : { "amiId" : "{{ssm:goldenAMIId}}" } </pre>  | 
|  Variable step definition  |  The definition of a step in the document is constructed by variables\.  |  <pre>...<br /><br />"mainSteps": [<br />    {<br />      "name": "LaunchInstance",<br />      "action": "aws:runInstances",<br />      "{{attemptModel}}": 1,<br />      "onFailure": "Continue",<br />      "inputs": {<br />        "ImageId": "ami-12345678",<br />        "MinInstanceCount": 1,<br />        "MaxInstanceCount": 2<br />      }<br /><br />...<br /><br />User supplies input : { "attemptModel" : "minAttempts" } </pre>  | 
|  Cross referencing Document Parameters  |  The user supplies an input parameter at start time which is a reference to another parameter in the document\.  |  <pre>...<br />"parameters": {<br />    "amiId": {<br />      "type": "STRING",<br />      "default": "ami-7f2e6015",<br />      "description": "list of commands to run as part of first step"<br />    },<br />    "otherAmiId": {<br />      "type": "STRING",<br />      "description": "The other amiId to try if this one fails".<br /><br />"default" : "{{amiId}}"<br />    },<br /><br />... </pre>  | 
|  Multi\-level expansion  |  The document defines a variable which evaluates to the name of a variable\. This sits within the variable delimeters \(that is *\{\{ \}\}*\) and is expanded to the value of that variable/parameter\.  |  <pre>...<br />  "parameters": {<br />    "param1": {<br />      "type": "STRING",<br />      "default": "param2",<br />      "description": "The parameter to reference"<br />    },<br />    "param2": {<br />      "type": "STRING",<br />      "default" : "echo {Hello world}",<br />      "description": "What to run"<br />    }<br />  },<br />  "mainSteps": [{<br />      "name": "runFixedCmds",<br />      "action": "aws:runCommand",<br />      "maxAttempts": 1,<br />      "onFailure": "Continue",<br />      "inputs": {<br />        "DocumentName": "AWS-RunPowerShellScript",<br /><br />"InstanceIds" : ""{{LaunchInstance.InstanceIds}},<br />        "Parameters": {<br />          "commands": [ "{{ {{param1}} }}"]<br /><br />}<br /><br />...<br /><br />Note: The customer intention here would be to run a runCommand of "echo {Hello world}" </pre>  | 