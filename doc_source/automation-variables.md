# Automation system variables<a name="automation-variables"></a>

Systems Manager Automation documents use the following variables\. For an example of how these variables are used, view the JSON source of the `AWS-UpdateWindowsAmi` document\. 

**To view the JSON source of the AWS\-UpdateWindowsAmi document**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

1. In the document list, use either the Search bar or the numbers to the right of the Search bar to choose the document **AWS\-UpdateWindowsAmi**\.

1. Choose the **Content** tab\. 

**System Variables**  
Automation documents currently support the following system variables\.


****  

| Variable | Details | 
| --- | --- | 
|  `global:ACCOUNT_ID`  |  The AWS account ID of the AWS Identity and Access Management \(IAM\) user or role in which Automation runs\.  | 
|  `global:DATE`  |  The date \(at execution time\) in the format yyyy\-MM\-dd\.  | 
|  `global:DATE_TIME`  |  The date and time \(at execution time\) in the format yyyy\-MM\-dd\_HH\.mm\.ss\.  | 
|  `global:REGION`  |  The Region that the document is run in\. For example, us\-east\-2\.  | 

**Automation Variables**  
Automation documents currently support the following automation variables\.


****  

| Variable | Details | 
| --- | --- | 
|  `automation:EXECUTION_ID`  |  The unique identifier assigned to the current automation execution\. For example, `1a2b3c-1a2b3c-1a2b3c-1a2b3c1a2b3c1a2b3c`\.  | 

**Topics**
+ [Terminology](#automation-terms)
+ [Supported scenarios](#automation-variables-support)
+ [Unsupported scenarios](#automation-variables-unsupported)

## Terminology<a name="automation-terms"></a>

The following terms describe how variables and parameters are resolved\.


****  

| Term | Definition | Example | 
| --- | --- | --- | 
|  Constant ARN  |  A valid ARN without variables  |  `arn:aws:iam::123456789012:role/roleName`  | 
|  Document parameter  |  A parameter defined at the document level for an Automation document \(for example, `instanceId`\)\. The parameter is used in a basic string replace\. Its value is supplied at Start Execution time\.  |  <pre>{ <br />   "description": "Create Image Demo",<br />   "version": "0.3",<br />   "assumeRole": "Your_Automation_Assume_Role_ARN",<br />   "parameters":{ <br />      "instanceId": { <br />         "type": "String",<br />         "description": "Instance to create image from"<br />   }<br />}</pre>  | 
|  System variable  |  A general variable substituted into the document when any part of the document is evaluated\.  |  <pre>"activities": [ <br />   { <br />      "id": "copyImage",<br />      "activityType": "AWS-CopyImage",<br />      "maxAttempts": 1,<br />      "onFailure": "Continue",<br />      "inputs": { <br />         "ImageName": "{{imageName}}",<br />         "SourceImageId": "{{sourceImageId}}",<br />         "SourceRegion": "{{sourceRegion}}",<br />         "Encrypted": true,<br />         "ImageDescription": "Test CopyImage Description created on {{global:DATE}}"<br />      }<br />   }<br />]</pre>  | 
|  Automation variable  |  A variable relating to the automation execution substituted into the document when any part of the document is evaluated\.  |  <pre>{ <br />   "name": "runFixedCmds",<br />   "action": "aws:runCommand",<br />   "maxAttempts": 1,<br />   "onFailure": "Continue",<br />   "inputs": { <br />      "DocumentName": "AWS-RunPowerShellScript",<br />      "InstanceIds": [ <br />         "{{LaunchInstance.InstanceIds}}"<br />      ],<br />      "Parameters": { <br />         "commands": [ <br />            "dir",<br />            "date",<br />            "“{{outputFormat}}” -f “left”,”right”,”{{global:DATE}}”,”{{automation:EXECUTION_ID}}”<br />         ]<br />      }<br />   }<br />}</pre>  | 
|  SSM Parameter  |  A variable defined within Parameter Store\. It cannot be directly referenced in step input\. Permissions might be required to access the parameter\.  |  <pre><br />description: Launch new Windows test instance<br />schemaVersion: '0.3'<br />assumeRole: '{{AutomationAssumeRole}}'<br />parameters:<br />  AutomationAssumeRole:<br />    type: String<br />    default: ''<br />    description: >-<br />      (Required) The ARN of the role that allows Automation to perform the<br />      actions on your behalf. If no role is specified, Systems Manager<br />      Automation uses your IAM permissions to run this document.<br />  LatestAmi:<br />    type: String<br />    default: >-<br />      {{ssm:/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-Base}}<br />    description: The latest Windows Server 2016 AMI queried from the public parameter.<br />mainSteps:<br />  - name: launchInstance<br />    action: 'aws:runInstances'<br />    maxAttempts: 3<br />    timeoutSeconds: 1200<br />    onFailure: Abort<br />    inputs:<br />      ImageId: '{{LatestAmi}}'<br />...</pre>  | 

## Supported scenarios<a name="automation-variables-support"></a>


****  

| Scenario | Comments | Example | 
| --- | --- | --- | 
|  Constant ARN `assumeRole` at creation\.  |  An authorization check is performed to verify that the calling user is permitted to pass the given `assumeRole`\.  |  <pre>{<br />  "description": "Test all Automation resolvable parameters",<br />  "schemaVersion": "0.3",<br />  "assumeRole": "arn:aws:iam::123456789012:role/roleName",<br />  "parameters": { <br />  ...</pre>  | 
|  Document parameter supplied for `assumeRole` at execution\.  |  Must be defined in the parameter list of the document\.  |  <pre>{<br />  "description": "Test all Automation resolvable parameters",<br />  "schemaVersion": "0.3",<br />  "assumeRole": "{{dynamicARN}}",<br />  "parameters": {<br /> ...</pre>  | 
|  Value supplied for document parameter at start\.  |  Customer supplies the value to use for a parameter\. Any execution inputs supplied at start time need to be defined in the parameter list of the document\.  |  <pre>...<br />"parameters": {<br />    "amiId": {<br />      "type": "String",<br />      "default": "ami-12345678",<br />      "description": "list of commands to run as part of first step"<br />    },<br />...</pre> Inputs to Start Automation Execution include : `{"amiId" : ["ami-12345678"] }`  | 
|  SSM Parameter referenced within document content\.  |  The variable exists within the customer's account, or is a publicly accessibly parameter, and the `assumeRole` for the document has access to the variable\. A check is performed at create time to confirm the `assumeRole` has access\. The parameter cannot be directly referenced in step input\.  |  <pre><br />...<br />parameters:<br />    LatestAmi:<br />    type: String<br />    default: >-<br />      {{ssm:/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-Base}}<br />    description: The latest Windows Server 2016 AMI queried from the public parameter.<br />mainSteps:<br />  - name: launchInstance<br />    action: 'aws:runInstances'<br />    maxAttempts: 3<br />    timeoutSeconds: 1200<br />    onFailure: Abort<br />    inputs:<br />      ImageId: '{{LatestAmi}}'<br />...</pre>  | 
|  System variable referenced within step definition  |  A system variable is substituted into the document at execution time\. The value injected into the document is relative to when the substitution occurs\. That is, the value of a time variable injected at step 1 is different from the value injected at step 3 because of the time it takes to run the steps between\. System variables do not need to be set in the parameter list of the document\.  |  <pre>...<br />  "mainSteps": [<br />    {<br />      "name": "RunSomeCommands",<br />      "action": "aws:runCommand",<br />      "maxAttempts": 1,<br />      "onFailure": "Continue",<br />      "inputs": {<br />        "DocumentName": "AWS:RunPowerShell",<br />        "InstanceIds": ["{{LaunchInstance.InstanceIds}}"],<br />        "Parameters": {<br />            "commands" : [<br />                "echo {The time is now {{global:DATE_TIME}}}"<br />            ]<br />        }<br />    }<br />}, ... </pre>  | 
|  Automation variable referenced within step definition\.  |  Automation variables do not need to be set in the parameter list of the document\. The only supported Automation variable is **automation:EXECUTION\_ID**\.  |  <pre>...<br />"mainSteps": [<br />    {<br />      "name": "invokeLambdaFunction",<br />      "action": "aws:invokeLambdaFunction",<br />      "maxAttempts": 1,<br />      "onFailure": "Continue",<br />      "inputs": {<br />        "FunctionName": "Hello-World-LambdaFunction",<br /><br />"Payload" : "{ "executionId" : "{{automation:EXECUTION_ID}}" }"<br />      }<br />    }<br />... </pre>  | 
|  Refer to output from previous step within next step definition\.  |  This is parameter redirection\. The output of a previous step is referenced using the syntax `{{stepName.OutputName}}`\. This syntax cannot be used by the customer for document parameters\. This is resolved at the time of execution for the referring step\. The parameter is not listed in the parameters of the document\.  |  <pre>...<br />"mainSteps": [<br />    {<br />      "name": "LaunchInstance",<br />      "action": "aws:runInstances",<br />      "maxAttempts": 1,<br />      "onFailure": "Continue",<br />      "inputs": {<br />        "ImageId": "{{amiId}}",<br />        "MinInstanceCount": 1,<br />        "MaxInstanceCount": 2<br />      }<br />    },<br />    {<br />      "name":"changeState",<br />      "action": "aws:changeInstanceState",<br />      "maxAttempts": 1,<br />      "onFailure": "Continue",<br />      "inputs": {<br />        "InstanceIds": ["{{LaunchInstance.InstanceIds}}"],<br />        "DesiredState": "terminated"<br />      }<br />    }<br /><br />... </pre>  | 

## Unsupported scenarios<a name="automation-variables-unsupported"></a>


****  

| Scenario | Comment | Example | 
| --- | --- | --- | 
|  SSM Parameter supplied for `assumeRole` at create  |  Not supported\.  |  <pre>...<br /><br />{<br />  "description": "Test all Automation resolvable parameters",<br />  "schemaVersion": "0.3",<br />  "assumeRole": "{{ssm:administratorRoleARN}}",<br />  "parameters": {<br /><br />... </pre>  | 
|  SSM Parameter directly referenced in step input\.  |  Returns `InvalidDocumentContent` exception at create time\.  |  <pre><br />...<br />mainSteps:<br />  - name: launchInstance<br />    action: 'aws:runInstances'<br />    maxAttempts: 3<br />    timeoutSeconds: 1200<br />    onFailure: Abort<br />    inputs:<br />      ImageId: '{{ssm:/aws/service/ami-windows-latest/Windows_Server-2016-English-Full-Base}}'<br />...</pre>  | 
|  Variable step definition  |  The definition of a step in the document is constructed by variables\.  |  <pre>...<br /><br />"mainSteps": [<br />    {<br />      "name": "LaunchInstance",<br />      "action": "aws:runInstances",<br />      "{{attemptModel}}": 1,<br />      "onFailure": "Continue",<br />      "inputs": {<br />        "ImageId": "ami-12345678",<br />        "MinInstanceCount": 1,<br />        "MaxInstanceCount": 2<br />      }<br /><br />...<br /><br />User supplies input : { "attemptModel" : "minAttempts" } </pre>  | 
|  Cross referencing document parameters  |  The user supplies an input parameter at start time, which is a reference to another parameter in the document\.  |  <pre>...<br />"parameters": {<br />    "amiId": {<br />      "type": "String",<br />      "default": "ami-7f2e6015",<br />      "description": "list of commands to run as part of first step"<br />    },<br />    "alternateAmiId": {<br />      "type": "String",<br />      "description": "The alternate AMI to try if this first fails".<br /><br />"default" : "{{amiId}}"<br />    },<br /><br />... </pre>  | 
|  Multi\-level expansion  |  The document defines a variable that evaluates to the name of a variable\. This sits within the variable delimiters \(that is *\{\{ \}\}*\) and is expanded to the value of that variable/parameter\.  |  <pre>...<br />  "parameters": {<br />    "firstParameter": {<br />      "type": "String",<br />      "default": "param2",<br />      "description": "The parameter to reference"<br />    },<br />    "secondParameter": {<br />      "type": "String",<br />      "default" : "echo {Hello world}",<br />      "description": "What to run"<br />    }<br />  },<br />  "mainSteps": [{<br />      "name": "runFixedCmds",<br />      "action": "aws:runCommand",<br />      "maxAttempts": 1,<br />      "onFailure": "Continue",<br />      "inputs": {<br />        "DocumentName": "AWS-RunPowerShellScript",<br /><br />"InstanceIds" : "{{LaunchInstance.InstanceIds}}",<br />        "Parameters": {<br />          "commands": [ "{{ {{firstParameter}} }}"]<br /><br />}<br /><br />...<br /><br />Note: The customer intention here would be to run a command of "echo {Hello world}" </pre>  | 
|  Referencing output from a document step that is a different variable type  |  The user references the output from a preceding document step within a subsequent step\. The output is a variable type that does not meet the requirements of the action in the subsequent step\.  |  <pre>...<br />mainSteps:<br />- name: getImageId<br />  action: aws:executeAwsApi<br />  inputs:<br />    Service: ec2<br />    Api: DescribeImages<br />    Filters:  <br />    - Name: "name"<br />      Values: <br />      - "{{ImageName}}"<br />  outputs:<br />  - Name: ImageIdList<br />    Selector: "$.Images"<br />    Type: "StringList"<br />- name: copyMyImages<br />  action: aws:copyImage<br />  maxAttempts: 3<br />  onFailure: Abort<br />  inputs:<br />    SourceImageId: {{getImageId.ImageIdList}}<br />    SourceRegion: ap-northeast-2<br />    ImageName: Encrypted Copies of LAMP base AMI in ap-northeast-2<br />    Encrypted: true <br />... <br />Note: You must provide the type required by the Automation action. <br />In this case, aws:copyImage requires a "String" type variable but the preceding step outputs a "StringList" type variable.<br />                                        </pre>  | 