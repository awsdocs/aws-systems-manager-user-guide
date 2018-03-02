# AWS Systems Manager Documents<a name="sysman-ssm-docs"></a>

An AWS Systems Manager document \(SSM document\) defines the actions that Systems Manager performs on your managed instances\. Systems Manager includes more than a dozen pre\-configured documents that you can use by specifying parameters at runtime\. Documents use JavaScript Object Notation \(JSON\) or YAML, and they include steps and parameters that you specify\.

**Types of SSM Documents**  
The following table describes the different types of SSM documents\.


****  

| Type | Use with | Details | 
| --- | --- | --- | 
|  Command document  |  [Run Command](execute-remote-commands.md) [State Manager](systems-manager-state.md)  |  Run Command uses command documents to execute commands\. State Manager uses command documents to apply a configuration\. These actions can be run on one or more targets at any point during the lifecycle of an instance,  | 
|  Policy document  |  [State Manager](systems-manager-state.md)  |  Policy documents enforce a policy on your targets\. If the policy document is removed, the policy action \(for example, collecting inventory\) no longer happens\.  | 
|  Automation document  |  [Automation](systems-manager-automation.md)  |  Use automation documents when performing common maintenance and deployment tasks such as creating or updating an Amazon Machine Image \(AMI\)\.  | 

**SSM Document Versions and Execution**  
You can create and save different versions of documents\. You can then specify a default version for each document\. The default version of a document can be updated to a newer version or reverted to an older version of the document\. When you change the content of a document, Systems Manager automatically increments the version of the document\. You can retrieve and use previous versions of a document\.

**Customizing a Document**  
If you want to customize the steps and actions in a document, you can create your own\. The first time you use a document to perform an action on an instance, the system stores the document with your AWS account\. For more information about how to create a Systems Manager document, see [Creating Systems Manager Documents](create-ssm-doc.md)\.

**Tagging a Document**  
You can tag your documents to help you quickly identify one or more documents based on the tags you've assigned to them\. For example, you can tag documents for specific environments, departments, users, groups, or periods\. You can also restrict access to documents by creating an IAM policy that specifies the tags that a user or group can access\. For more information, see [Tagging Systems Manager Documents](sysman-ssm-docs-tagging.md)\.

**Sharing a Document**  
You can make your documents public or share them with specific AWS accounts\. For more information, see [Sharing Systems Manager Documents](ssm-sharing.md)\.

**SSM Document Limits**  
For information about SSM document limits, see [AWS Systems Manager Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_ssm)\.


+ [Systems Manager Pre\-Defined Documents](#predefined-documents)
+ [SSM Document Schemas and Features](#document-schemas-features)
+ [SSM Document Syntax](#sysman-doc-syntax)
+ [Creating Systems Manager Documents](create-ssm-doc.md)
+ [Tagging Systems Manager Documents](sysman-ssm-docs-tagging.md)
+ [Sharing Systems Manager Documents](ssm-sharing.md)
+ [Creating Composite Documents](composite-docs.md)
+ [Running Documents from Remote Locations](run-remote-documents.md)
+ [SSM Document Plugin Reference](ssm-plugins.md)
+ [Systems Manager Automation Document Reference](automation-actions.md)

## Systems Manager Pre\-Defined Documents<a name="predefined-documents"></a>

To help you get started quickly, Systems Manager provides pre\-defined documents\. To view these documents in the AWS Systems Manager console, in the left navigation, choose **Documents**\. After you choose a document, choose **View details** to view information about the document you selected\.

To view these documents in the Amazon EC2 console, expand **Systems Manager Shared Resources**, and then choose **Documents**\. After you choose a document, use the tabs in the lower pane to view information about the document you selected\.

You can also use the AWS CLI and Tools for Windows PowerShell commands to view a list of documents and get descriptions about those documents\.

To view information about documents using the **AWS CLI**, run the following commands:

```
aws ssm list-documents
```

```
aws ssm describe-document --name "document_name"
```

To view information about documents using the **Tools for Windows PowerShell**, run the following commands:

```
Get-SSMDocumentList
```

```
Get-SSMDocumentDescription -Name "document_name"
```

## SSM Document Schemas and Features<a name="document-schemas-features"></a>

Systems Manager documents currently use the following schema versions\.

+ Documents of type `Command` can use schema version 1\.2, 2\.0, and 2\.2\. If you are currently using schema 1\.2 documents, we recommend that you create documents that use schema version 2\.2\.

+ Documents of type `Policy` must use schema version 2\.0 or later\.

+ Documents of type `Automation` must use schema version 0\.3\.

+ You can create documents in JSON or YAML\.

By using the latest schema version for `Command` and `Policy` documents, you can take advantage of the following features\.


**Schema Version 2\.2 Document Features**  

| Feature | Details | 
| --- | --- | 
|  Document editing  |  Documents can now be updated\. With version 1\.2, any update to a document required that you save it with a different name\.  | 
|  Automatic versioning  |  Any update to a document creates a new version\. This is not a schema version, but a version of the document\.  | 
|  Default version  |  If you have multiple versions of a document, you can specify which version is the default document\.  | 
|  Sequencing  |  Plugins or *steps* in a document execute in the order that you specified\.  | 
|  Cross\-platform support  |  Cross\-platform support enables you to specify different operating systems for different plugins within the same SSM document\. Cross\-platform support uses the `precondition` parameter within a step\.   | 

**Note**  
You must keep the SSM Agent on your instances updated with the latest version to use new Systems Manager features and SSM document features\. For more information, see [Example: Update the SSM Agent](rc-console.md#rc-console-agentexample)\.

The following table lists the differences between major schema versions\.


****  

| Version 1\.2 | Version 2\.2 \(lastest version\) | Details | 
| --- | --- | --- | 
|  runtimeConfig  |  mainSteps  |  In version 2\.2, the `mainSteps` section replaces `runtimeConfig`\. The `mainSteps` section enables Systems Manager to execute steps in sequence\.  | 
|  properties  |  inputs  |  In version 2\.2, the `inputs` section replaces the `properties` section\. The `inputs` section accepts parameters for steps\.  | 
|  commands  |  runCommand  |  In version 2\.2, the `inputs` section takes the `runCommand` parameter instead of the `commands` parameter\.  | 
|  id  |  action  |  In version 2\.2, `Action` replaces `ID`\. This is just a name change\.  | 
|  not applicable  |  name  |  In version 2\.2, `name` is any user\-defined name for a step\.  | 

**Using the Precondition Parameter**  
With schema version 2\.2 or higher, you can use the `precondition` parameter to specify the target operating system for each plugin\. The `precondition` parameter supports `platformType` and a value of either `Windows` or `Linux`\.

For documents that use schema version 2\.2 or higher, if `precondition` is not specified, each plugin is either executed or skipped based on the plugin’s compatibility with the operating system\. For documents that use schema 2\.0 or earlier, incompatible plugins throw an error\.

For example, in a schema version 2\.2 document, if `precondition` is not specified and the `aws:runShellScript` plugin is listed, then the step executes on Linux instances, but the system skips it on Windows instances because the `aws:runShellScript` is not compatible with Windows instances\. However, for a schema version 2\.0 document, if you specify the `aws:runShellScript` plugin, and then run the document on a Windows instances, the execution fails\. You can see an example of the the precondition parameter in an SSM document later in this section\.

## SSM Document Syntax<a name="sysman-doc-syntax"></a>

The syntax of your document is defined by the schema version used to create it\. We recommended that you use schema version 2\.2 or higher\. Documents that use this schema version include the following top\-level elements\. For information about the properties that you can specify in these elements, see [Top\-level Elements](ssm-plugins.md#top-level)\.

+ `schemaVersion`: The schema version to use\.

+ `Description`: Information you provide to describe the purpose of the document\.

+ `Parameters`: The parameters the document accepts\. For parameters that you reference often, we recommend that you store those parameters in Systems Manager Parameter Store and then reference them\. You can reference `String` and `StringList` Systems Manager parameters in this section of a document\. You can't reference Secure String Systems Manager parameters in this section of a document\. For more information, see [AWS Systems Manager Parameter Store](systems-manager-paramstore.md)\.

+ `mainSteps`: An object that can include multiple steps \(plugins\)\. Steps include one or more actions, an optional precondition, a unique name of the action, and inputs \(parameters\) for those actions\. For a list of supported plugins and plugin properties, see [SSM Document Plugin Reference](ssm-plugins.md)\.
**Important**  
The name of the action can't include a space\. If a name includes a space, you will receive an InvalidDocumentContent error\.

### Schema Version 2\.2<a name="documents-schema-twox"></a>

The following example shows the top\-level elements of a schema version 2\.2 document in JSON\.

```
{
   "schemaVersion":"2.2",
   "description":"A description of the document.",
   "parameters":{
       "parameter 1":{
           "one or more parameter properties"
        },
        "parameter 2":{
            "one or more parameter properties"
        },
        "parameter 3":{
           "one or more parameter properties"
        }
    },
   "mainSteps":[
      {
         "action":"plugin 1",
         "name":"A name for this action.",
         "inputs":{
            "name":"{{ input 1 }}",
            "name":"{{ input 2 }}",
            "name":"{{ input 3 }}",
            
         }
      }
   ]
}
```

**YAML Schema Version 2\.2 Example**  
You can use the following YAML document with Run Command to return the hostname of one or more instances\.

```
---
schemaVersion: '2.2'
description: Sample document
mainSteps:
- action: aws:runPowerShellScript
  name: runPowerShellScript
  inputs:
    runCommand:
    - hostname
```

**Schema Version 2\.2 Precondition Parameter Example**  
Schema version 2\.2 provides cross\-platform support\. This means that within a single SSM document you can specify different operating systems for different plugins\. Cross\-platform support uses the `precondition` parameter within a step, as shown in the following example\. 

```
{
   "schemaVersion":"2.2",
   "description":"cross-platform sample",
   "mainSteps":[
      {
         "action":"aws:runPowerShellScript",
         "name":"PatchWindows",
         "precondition":{
            "StringEquals":[
               "platformType",
               "Windows"
            ]
         },
         "inputs":{
            "runCommand":[
               "cmds"
            ]
         }
      },
      {
         "action":"aws:runShellScript",
         "name":"PatchLinux",
         "precondition":{
            "StringEquals":[
               "platformType",
               "Linux"
            ]
         },
         "inputs":{
            "runCommand":[
               "cmds"
            ]
         }
      }
   ]
}
```

#### Schema Version Examples 2\.2<a name="documents-schema-2"></a>

You can use the following YAML document with State Manager to download and install the ClamAV antivirus software\. State Manager enforces a specific configuration, which means that each time the State Manager association is run, the system checks to see if the ClamAV software is installed\. If not, State Manager reruns this document\.

```
---
schemaVersion: '2.2'
description: State Manager Bootstrap Example
parameters: {}
mainSteps:
- action: aws:runShellScript
  name: configureServer
  inputs:
    runCommand:
    - sudo yum install -y httpd24
    - sudo yum --enablerepo=epel install -y clamav
```

**Schema Version 2\.0 YAML Inventory Example**  
You can use the following YAML document with State Manager to collect inventory metadata about your instances\.

```
---
schemaVersion: '2.2'
description: Software Inventory Policy Document.
parameters:
  applications:
    type: String
    default: Enabled
    description: "(Optional) Collect data for installed applications."
    allowedValues:
    - Enabled
    - Disabled
  awsComponents:
    type: String
    default: Enabled
    description: "(Optional) Collect data for AWS Components like amazon-ssm-agent."
    allowedValues:
    - Enabled
    - Disabled
  networkConfig:
    type: String
    default: Enabled
    description: "(Optional) Collect data for Network configurations."
    allowedValues:
    - Enabled
    - Disabled
  windowsUpdates:
    type: String
    default: Enabled
    description: "(Optional) Collect data for all Windows Updates."
    allowedValues:
    - Enabled
    - Disabled
  instanceDetailedInformation:
    type: String
    default: Enabled
    description: "(Optional) Collect additional information about the instance, including
      the CPU model, speed, and the number of cores, to name a few."
    allowedValues:
    - Enabled
    - Disabled
  customInventory:
    type: String
    default: Enabled
    description: "(Optional) Collect data for custom inventory."
    allowedValues:
    - Enabled
    - Disabled
mainSteps:
- action: aws:softwareInventory
  name: collectSoftwareInventoryItems
  inputs:
    applications: "{{ applications }}"
    awsComponents: "{{ awsComponents }}"
    networkConfig: "{{ networkConfig }}"
    windowsUpdates: "{{ windowsUpdates }}"
    instanceDetailedInformation: "{{ instanceDetailedInformation }}"
    customInventory: "{{ customInventory }}"
```

**Schema Version 2\.2 AWS\-ConfigureAWSPackage Example**  
The following example shows the AWS\-ConfigureAWSPackage document\. The **mainSteps** section includes the **aws:configurePackage** plugin in the **action** step\.

```
{
	"schemaVersion": "2.2",
	"description": "Install or uninstall the latest version or specified version of an AWS package. 
	                Available packages include the following: AWSPVDriver, AwsEnaNetworkDriver, IntelSriovDriver, 
	                AwsVssComponents, and AmazonCloudWatchAgent.",
	"parameters": {
		"action": {
			"description": "(Required) Specify whether or not to install or uninstall the package.",
			"type": "String",
			"allowedValues": [
				"Install",
				"Uninstall"
			]
		},
		"name": {
			"description": "(Required) The package to install/uninstall.",
			"type": "String",
			"allowedPattern": "^arn:[a-z0-9][-.a-z0-9]{0,62}:[a-z0-9][-.a-z0-9]{0,62}:([a-z0-9]
			                   [-.a-z0-9]{0,62})?:([a-z0-9][-.a-z0-9]{0,62})?:package\\/[a-zA-Z][a-zA-Z0-9\\-_]{0,39}$|^
			                   [a-zA-Z][a-zA-Z0-9\\-_]{0,39}$"
		},
		"version": {
			"description": "(Optional) A specific version of the package to install or uninstall. If installing, 
			the system installs the latest published version, by default. If uninstalling, the system uninstalls 
			the currently installed version, by default. If no installed version is found, the latest published 
			version is downloaded, and the uninstall action is run.",
			     "type": "String",
			     "default": "latest"
		}
	},
	"mainSteps": [{
		"action": "aws:configurePackage",
		"name": "configurePackage",
		"inputs": {
			"name": "{{ name }}",
			"action": "{{ action }}",
			"version": "{{ version }}"
		}
	}]
}
```

### Schema Version 1\.2<a name="documents-schema-onex"></a>

The following example shows the top\-level elements of a schema version 1\.2 document\.

```
{
   "schemaVersion":"1.2",
   "description":"A description of the Systems Manager document.",
   "parameters":{
      "parameter 1":{
         "one or more parameter properties"
      },
      "parameter 2":{
         "one or more parameter properties"
      },
      "parameter 3":{
         "one or more parameter properties"
      }
   },
   "runtimeConfig":{
      "plugin 1":{
         "properties":[
            {
               "one or more plugin properties"
            }
         ]
      }
   }
}
```

**Schema Version 1\.2 Example**  
The following example shows the AWS\-RunShellScript Systems Manager document\. The **runtimeConfig** section includes the **aws:runShellScript** plugin\.

```
{
    "schemaVersion":"1.2",
    "description":"Run a shell script or specify the commands to run.",
    "parameters":{
        "commands":{
            "type":"StringList",
            "description":"(Required) Specify a shell script or a command to run.",
            "minItems":1,
            "displayType":"textarea"
        },
        "workingDirectory":{
            "type":"String",
            "default":"",
            "description":"(Optional) The path to the working directory on your instance.",
            "maxChars":4096
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
                    "workingDirectory":"{{ workingDirectory }}",
                    "timeoutSeconds":"{{ executionTimeout }}"
                }
            ]
        }
    }
}
```