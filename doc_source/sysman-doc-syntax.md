# SSM Document Syntax<a name="sysman-doc-syntax"></a>

The syntax of your document is defined by the schema version used to create it\. We recommended that you use schema version 2\.2 or higher\. Documents that use this schema version include the following top\-level elements\. For information about the properties that you can specify in these elements, see [Top\-level Elements](ssm-plugins.md#top-level)\.
+ `schemaVersion`: The schema version to use\.
+ `Description`: Information you provide to describe the purpose of the document\.
+ `Parameters`: The parameters the document accepts\. For parameters that you reference often, we recommend that you store those parameters in Systems Manager Parameter Store and then reference them\. You can reference `String` and `StringList` Systems Manager parameters in this section of a document\. You can't reference Secure String Systems Manager parameters in this section of a document\. For more information, see [AWS Systems Manager Parameter Store](systems-manager-paramstore.md)\.
+ `mainSteps`: An object that can include multiple steps \(plugins\)\. Steps include one or more actions, an optional precondition, a unique name of the action, and inputs \(parameters\) for those actions\. For a list of supported plugins and plugin properties, see [SSM Document Plugin Reference](ssm-plugins.md)\.
**Important**  
The name of the action can't include a space\. If a name includes a space, you will receive an InvalidDocumentContent error\.

**Topics**
+ [Schema Version 2\.2](#documents-schema-twox)
+ [Schema Version 1\.2](#documents-schema-onex)

## Schema Version 2\.2<a name="documents-schema-twox"></a>

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

### Schema Version Examples 2\.2<a name="documents-schema-2"></a>

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
	                AwsVssComponents, and AmazonCloudWatchAgent, and AWSSupport-EC2Rescue.",
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

## Schema Version 1\.2<a name="documents-schema-onex"></a>

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