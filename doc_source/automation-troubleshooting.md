# Troubleshooting Systems Manager Automation<a name="automation-troubleshooting"></a>

Use the following information to help you troubleshoot problems with the Automation service\. This topic includes specific tasks to resolve issues based on Automation error messages\.

**Topics**
+ [Common Automation Errors](#automation-trbl-common)
+ [Automation Execution Failed to Start](#automation-trbl-access)
+ [Execution Started, but Status is Failed](#automation-trbl-exstrt)
+ [Execution Started, but Timed Out](#automation-trbl-to)

## Common Automation Errors<a name="automation-trbl-common"></a>

This section includes information about common Automation errors\.

### VPC not defined 400<a name="automation-trbl-common-vpc"></a>

By default, when Automation runs either the AWS\-UpdateLinuxAmi document or the AWS\-UpdateWindowsAmi document, the system creates a temporary instance in the default VPC \(172\.30\.0\.0/16\)\. If you deleted the default VPC, you will receive the following error:

VPC not defined 400

To solve this problem, you must create a new Automation document that includes the subnet ID\. Copy a sample document below that includes the subnet ID parameter and create a new document\. For information about creating a document, see [Walkthrough: Create an Automation Document](automation-createdoc.md)\.

**AWS\-UpdateLinuxAmi**

```
{
   "schemaVersion":"0.3",
   "description":"Updates AMI with Linux distribution packages and Amazon software. For details,see https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/sysman-ami-walkthrough.html",
   "assumeRole":"{{AutomationAssumeRole}}",
   "parameters":{
      "SourceAmiId":{
         "type":"String",
         "description":"(Required) The source Amazon Machine Image ID."
      },
      "InstanceIamRole":{
         "type":"String",
         "description":"(Required) The name of the role that enables Systems Manager (SSM) to manage the instance.",
         "default":"ManagedInstanceProfile"
      },
      "AutomationAssumeRole":{
         "type":"String",
         "description":"(Required) The ARN of the role that allows Automation to perform the actions on your behalf.",
         "default":"arn:aws:iam::{{global:ACCOUNT_ID}}:role/AutomationServiceRole"
      },
      "SubnetId":{
         "type":"String",
         "description":"(Required) The subnet that the created instance will be placed into."
      },
      "TargetAmiName":{
         "type":"String",
         "description":"(Optional) The name of the new AMI that will be created. Default is a system-generated string including the source AMI id, and the creation time and date.",
         "default":"UpdateLinuxAmi_from_{{SourceAmiId}}_on_{{global:DATE_TIME}}"
      },
      "InstanceType":{
         "type":"String",
         "description":"(Optional) Type of instance to launch as the workspace host. Instance types vary by region. Default is t2.micro.",
         "default":"t2.micro"
      },
      "PreUpdateScript":{
         "type":"String",
         "description":"(Optional) URL of a script to run before updates are applied. Default (\"none\") is to not run a script.",
         "default":"none"
      },
      "PostUpdateScript":{
         "type":"String",
         "description":"(Optional) URL of a script to run after package updates are applied. Default (\"none\") is to not run a script.",
         "default":"none"
      },
      "IncludePackages":{
         "type":"String",
         "description":"(Optional) Only update these named packages. By default (\"all\"), all available updates are applied.",
         "default":"all"
      },
      "ExcludePackages":{
         "type":"String",
         "description":"(Optional) Names of packages to hold back from updates, under all conditions. By default (\"none\"), no package is excluded.",
         "default":"none"
      }
   },
   "mainSteps":[
      {
         "name":"launchInstance",
         "action":"aws:runInstances",
         "maxAttempts":3,
         "timeoutSeconds":1200,
         "onFailure":"Abort",
         "inputs":{
            "ImageId":"{{SourceAmiId}}",
            "InstanceType":"{{InstanceType}}",
            "SubnetId":"{{ SubnetId }}",
            "UserData":"IyEvYmluL2Jhc2gNCg0KZnVuY3Rpb24gZ2V0X2NvbnRlbnRzKCkgew0KICAgIGlmIFsgLXggIiQod2hpY2ggY3VybCkiIF07IHRoZW4NCiAgICAgICAgY3VybCAtcyAtZiAiJDEiDQogICAgZWxpZiBbIC14ICIkKHdoaWNoIHdnZXQpIiBdOyB0aGVuDQogICAgICAgIHdnZXQgIiQxIiAtTyAtDQogICAgZWxzZQ0KICAgICAgICBkaWUgIk5vIGRvd25sb2FkIHV0aWxpdHkgKGN1cmwsIHdnZXQpIg0KICAgIGZpDQp9DQoNCnJlYWRvbmx5IElERU5USVRZX1VSTD0iaHR0cDovLzE2OS4yNTQuMTY5LjI1NC8yMDE2LTA2LTMwL2R5bmFtaWMvaW5zdGFuY2UtaWRlbnRpdHkvZG9jdW1lbnQvIg0KcmVhZG9ubHkgVFJVRV9SRUdJT049JChnZXRfY29udGVudHMgIiRJREVOVElUWV9VUkwiIHwgYXdrIC1GXCIgJy9yZWdpb24vIHsgcHJpbnQgJDQgfScpDQpyZWFkb25seSBERUZBVUxUX1JFR0lPTj0idXMtZWFzdC0xIg0KcmVhZG9ubHkgUkVHSU9OPSIke1RSVUVfUkVHSU9OOi0kREVGQVVMVF9SRUdJT059Ig0KDQpyZWFkb25seSBTQ1JJUFRfTkFNRT0iYXdzLWluc3RhbGwtc3NtLWFnZW50Ig0KIFNDUklQVF9VUkw9Imh0dHBzOi8vYXdzLXNzbS1kb3dubG9hZHMtJFJFR0lPTi5zMy5hbWF6b25hd3MuY29tL3NjcmlwdHMvJFNDUklQVF9OQU1FIg0KDQppZiBbICIkUkVHSU9OIiA9ICJjbi1ub3J0aC0xIiBdOyB0aGVuDQogIFNDUklQVF9VUkw9Imh0dHBzOi8vYXdzLXNzbS1kb3dubG9hZHMtJFJFR0lPTi5zMy5jbi1ub3J0aC0xLmFtYXpvbmF3cy5jb20uY24vc2NyaXB0cy8kU0NSSVBUX05BTUUiDQpmaQ0KDQppZiBbICIkUkVHSU9OIiA9ICJ1cy1nb3Ytd2VzdC0xIiBdOyB0aGVuDQogIFNDUklQVF9VUkw9Imh0dHBzOi8vYXdzLXNzbS1kb3dubG9hZHMtJFJFR0lPTi5zMy11cy1nb3Ytd2VzdC0xLmFtYXpvbmF3cy5jb20vc2NyaXB0cy8kU0NSSVBUX05BTUUiDQpmaQ0KDQpjZCAvdG1wDQpGSUxFX1NJWkU9MA0KTUFYX1JFVFJZX0NPVU5UPTMNClJFVFJZX0NPVU5UPTANCg0Kd2hpbGUgWyAkUkVUUllfQ09VTlQgLWx0ICRNQVhfUkVUUllfQ09VTlQgXSA7IGRvDQogIGVjaG8gQVdTLVVwZGF0ZUxpbnV4QW1pOiBEb3dubG9hZGluZyBzY3JpcHQgZnJvbSAkU0NSSVBUX1VSTA0KICBnZXRfY29udGVudHMgIiRTQ1JJUFRfVVJMIiA+ICIkU0NSSVBUX05BTUUiDQogIEZJTEVfU0laRT0kKGR1IC1rIC90bXAvJFNDUklQVF9OQU1FIHwgY3V0IC1mMSkNCiAgZWNobyBBV1MtVXBkYXRlTGludXhBbWk6IEZpbmlzaGVkIGRvd25sb2FkaW5nIHNjcmlwdCwgc2l6ZTogJEZJTEVfU0laRQ0KICBpZiBbICRGSUxFX1NJWkUgLWd0IDAgXTsgdGhlbg0KICAgIGJyZWFrDQogIGVsc2UNCiAgICBpZiBbWyAkUkVUUllfQ09VTlQgLWx0IE1BWF9SRVRSWV9DT1VOVCBdXTsgdGhlbg0KICAgICAgUkVUUllfQ09VTlQ9JCgoUkVUUllfQ09VTlQrMSkpOw0KICAgICAgZWNobyBBV1MtVXBkYXRlTGludXhBbWk6IEZpbGVTaXplIGlzIDAsIHJldHJ5Q291bnQ6ICRSRVRSWV9DT1VOVA0KICAgIGZpDQogIGZpIA0KZG9uZQ0KDQppZiBbICRGSUxFX1NJWkUgLWd0IDAgXTsgdGhlbg0KICBjaG1vZCAreCAiJFNDUklQVF9OQU1FIg0KICBlY2hvIEFXUy1VcGRhdGVMaW51eEFtaTogUnVubmluZyBVcGRhdGVTU01BZ2VudCBzY3JpcHQgbm93IC4uLi4NCiAgLi8iJFNDUklQVF9OQU1FIiAtLXJlZ2lvbiAiJFJFR0lPTiINCmVsc2UNCiAgZWNobyBBV1MtVXBkYXRlTGludXhBbWk6IFVuYWJsZSB0byBkb3dubG9hZCBzY3JpcHQsIHF1aXR0aW5nIC4uLi4NCmZp",
            "MinInstanceCount":1,
            "MaxInstanceCount":1,
            "IamInstanceProfileName":"{{InstanceIamRole}}"
         }
      },
      {
         "name":"updateOSSoftware",
         "action":"aws:runCommand",
         "maxAttempts":3,
         "timeoutSeconds":3600,
         "onFailure":"Abort",
         "inputs":{
            "DocumentName":"AWS-RunShellScript",
            "InstanceIds":[
               "{{launchInstance.InstanceIds}}"
            ],
            "Parameters":{
               "commands":[
                  "set -e",
                  "[ -x \"$(which wget)\" ] && get_contents='wget $1 -O -'",
                  "[ -x \"$(which curl)\" ] && get_contents='curl -s -f $1'",
                  "eval $get_contents https://aws-ssm-downloads-{{global:REGION}}.s3.amazonaws.com/scripts/aws-update-linux-instance > /tmp/aws-update-linux-instance",
                  "chmod +x /tmp/aws-update-linux-instance",
                  "/tmp/aws-update-linux-instance --pre-update-script '{{PreUpdateScript}}' --post-update-script '{{PostUpdateScript}}' --include-packages '{{IncludePackages}}' --exclude-packages '{{ExcludePackages}}' 2>&1 | tee /tmp/aws-update-linux-instance.log"
               ]
            }
         }
      },
      {
         "name":"stopInstance",
         "action":"aws:changeInstanceState",
         "maxAttempts":3,
         "timeoutSeconds":1200,
         "onFailure":"Abort",
         "inputs":{
            "InstanceIds":[
               "{{launchInstance.InstanceIds}}"
            ],
            "DesiredState":"stopped"
         }
      },
      {
         "name":"createImage",
         "action":"aws:createImage",
         "maxAttempts":3,
         "onFailure":"Abort",
         "inputs":{
            "InstanceId":"{{launchInstance.InstanceIds}}",
            "ImageName":"{{TargetAmiName}}",
            "NoReboot":true,
            "ImageDescription":"AMI Generated by EC2 Automation on {{global:DATE_TIME}} from {{SourceAmiId}}"
         }
      },
      {
         "name":"terminateInstance",
         "action":"aws:changeInstanceState",
         "maxAttempts":3,
         "onFailure":"Continue",
         "inputs":{
            "InstanceIds":[
               "{{launchInstance.InstanceIds}}"
            ],
            "DesiredState":"terminated"
         }
      }
   ],
   "outputs":[
      "createImage.ImageId"
   ]
}
```

**AWS\-UpdateWindowsAmi**

```
{
   "schemaVersion":"0.3",
   "description":"Updates a Microsoft Windows AMI. By default it will install all Windows updates, Amazon software, and Amazon drivers. It will then sysprep and create a new AMI. Supports Windows Server 2008 R2 and greater.",
   "assumeRole":"{{ AutomationAssumeRole }}",
   "parameters":{
      "SourceAmiId":{
         "type":"String",
         "description":"(Required) The source Amazon Machine Image ID."
      },
      "IamInstanceProfileName":{
         "type":"String",
         "description":"(Required) The name of the role that enables Systems Manager to manage the instance.",
         "default":"ManagedInstanceProfile"
      },
      "AutomationAssumeRole":{
         "type":"String",
         "description":"(Required) The ARN of the role that allows Automation to perform the actions on your behalf.",
         "default":"arn:aws:iam::{{global:ACCOUNT_ID}}:role/AutomationServiceRole"
      },
      "SubnetId": {
        "type": "String",
        "description": "(Required) The subnet that the created instance will be placed into."
    },
      "TargetAmiName":{
         "type":"String",
         "description":"(Optional) The name of the new AMI that will be created. Default is a system-generated string including the source AMI id, and the creation time and date.",
         "default":"UpdateWindowsAmi_from_{{SourceAmiId}}_on_{{global:DATE_TIME}}"
      },
      "InstanceType":{
         "type":"String",
         "description":"(Optional) Type of instance to launch as the workspace host. Instance types vary by region. Default is t2.medium.",
         "default":"t2.medium"
      },
      "IncludeKbs":{
         "type":"String",
         "description":"(Optional) Specify one or more Microsoft Knowledge Base (KB) article IDs to include. You can install multiple IDs using comma-separated values. When specified, the categories and security level values are ignored. Valid formats: KB9876543 or 9876543.",
         "default":""
      },
      "ExcludeKbs":{
         "type":"String",
         "description":"(Optional) Specify one or more Microsoft Knowledge Base (KB) article IDs to exclude. You can exclude multiple IDs using comma-separated values. When specified, all these KBs are excluded from install process. Valid formats: KB9876543 or 9876543.",
         "default":""
      },
      "Categories":{
         "type":"String",
         "description":"(Optional) Specify one or more update categories. You can filter categories using comma-separated values. By default patches for all categories are selected. If value supplied, the update list is filtered by those values. Options: Critical Update, Security Update, Definition Update, Update Rollup, Service Pack, Tool, Update or Driver. Valid formats include a single entry, for example: Critical Update. Or, you can specify a comma separated list: Critical Update,Security Update,Definition Update. NOTE: There cannot be any spaces around the commas.",
         "default":""
      },
      "SeverityLevels":{
         "type":"String",
         "description":"(Optional) Specify one or more MSRC severity levels associated with an update. You can filter severity levels using comma-separated values. By default patches for all security levels are selected. If value supplied, the update list is filtered by those values. Options: Critical, Important, Low, Moderate or Unspecified. Valid formats include a single entry, for example: Critical. Or, you can specify a comma separated list: Critical,Important,Low.",
         "default":""
      },
      "PreUpdateScript":{
         "type":"String",
         "description":"(Optional) A script provided as a string. It will run prior to installing OS updates.",
         "default":""
      },
      "PostUpdateScript":{
         "type":"String",
         "description":"(Optional) A script provided as a string. It will run after installing OS updates.",
         "default":""
      }
   },
   "mainSteps":[
      {
         "name":"LaunchInstance",
         "action":"aws:runInstances",
         "timeoutSeconds":1800,
         "maxAttempts":3,
         "onFailure":"Abort",
         "inputs":{
            "ImageId":"{{ SourceAmiId  }}",
            "InstanceType":"{{ InstanceType }}",
            "SubnetId":"{{ SubnetId }}",
            "MinInstanceCount":1,
            "MaxInstanceCount":1,
            "IamInstanceProfileName":"{{ IamInstanceProfileName }}"
         }
      },
      {
         "name":"RunPreUpdateScript",
         "action":"aws:runCommand",
         "maxAttempts":3,
         "onFailure":"Abort",
         "timeoutSeconds":1800,
         "inputs":{
            "DocumentName":"AWS-RunPowerShellScript",
            "InstanceIds":[
               "{{ LaunchInstance.InstanceIds }}"
            ],
            "Parameters":{
               "commands":"{{ PreUpdateScript }}"
            }
         }
      },
      {
         "name":"UpdateSSMAgent",
         "action":"aws:runCommand",
         "maxAttempts":3,
         "onFailure":"Abort",
         "timeoutSeconds":600,
         "inputs":{
            "DocumentName":"AWS-UpdateSSMAgent",
            "InstanceIds":[
               "{{ LaunchInstance.InstanceIds }}"
            ]
         }
      },
      {
         "name":"UpdateEC2Config",
         "action":"aws:runCommand",
         "maxAttempts":3,
         "onFailure":"Abort",
         "timeoutSeconds":7200,
         "inputs":{
            "DocumentName":"AWS-InstallPowerShellModule",
            "InstanceIds":[
               "{{ LaunchInstance.InstanceIds }}"
            ],
            "Parameters":{
               "executionTimeout":"7200",
               "source":"https://aws-ssm-downloads-{{global:REGION}}.s3.amazonaws.com/PSModules/AWSUpdateWindowsInstance/Latest/AWSUpdateWindowsInstance.zip",
               "sourceHash":"14CAD416F4A054894EBD2091EA4B99E542368BE5895BDD466B567C1ABA87C87C",
               "commands":[
                  "Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force",
                  "Import-Module AWSUpdateWindowsInstance",
                  "if ([Environment]::OSVersion.Version.Major -ge 10) {",
                  "  Install-AwsUwiEC2Launch -Id {{ automation:EXECUTION_ID }}",
                  "} else {",
                  "  Install-AwsUwiEC2Config -Id {{ automation:EXECUTION_ID }}",
                  "}"
               ]
            }
         }
      },
      {
         "name":"UpdateAWSPVDriver",
         "action":"aws:runCommand",
         "maxAttempts":3,
         "onFailure":"Abort",
         "timeoutSeconds":600,
         "inputs":{
            "DocumentName":"AWS-ConfigureAWSPackage",
            "InstanceIds":[
               "{{ LaunchInstance.InstanceIds }}"
            ],
            "Parameters":{
               "name":"AWSPVDriver",
               "action":"Install"
            }
         }
      },
      {
         "name":"InstallWindowsUpdates",
         "action":"aws:runCommand",
         "maxAttempts":3,
         "onFailure":"Abort",
         "timeoutSeconds":14400,
         "inputs":{
            "DocumentName":"AWS-InstallWindowsUpdates",
            "InstanceIds":[
               "{{ LaunchInstance.InstanceIds }}"
            ],
            "Parameters":{
               "Action":"Install",
               "IncludeKbs":"{{ IncludeKbs }}",
               "ExcludeKbs":"{{ ExcludeKbs }}",
               "Categories":"{{ Categories }}",
               "SeverityLevels":"{{ SeverityLevels }}"
            }
         }
      },
      {
         "name":"RunPostUpdateScript",
         "action":"aws:runCommand",
         "maxAttempts":3,
         "onFailure":"Abort",
         "timeoutSeconds":1800,
         "inputs":{
            "DocumentName":"AWS-RunPowerShellScript",
            "InstanceIds":[
               "{{ LaunchInstance.InstanceIds }}"
            ],
            "Parameters":{
               "commands":"{{ PostUpdateScript }}"
            }
         }
      },
      {
         "name":"RunSysprepGeneralize",
         "action":"aws:runCommand",
         "maxAttempts":3,
         "onFailure":"Abort",
         "timeoutSeconds":7200,
         "inputs":{
            "DocumentName":"AWS-InstallPowerShellModule",
            "InstanceIds":[
               "{{ LaunchInstance.InstanceIds }}"
            ],
            "Parameters":{
               "executionTimeout":"7200",
               "source":"https://aws-ssm-downloads-{{global:REGION}}.s3.amazonaws.com/PSModules/AWSUpdateWindowsInstance/Latest/AWSUpdateWindowsInstance.zip",
               "sourceHash":"14CAD416F4A054894EBD2091EA4B99E542368BE5895BDD466B567C1ABA87C87C",
               "commands":[
                  "Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force",
                  "Import-Module AWSUpdateWindowsInstance",
                  "Start-AwsUwiSysprep -Id {{ automation:EXECUTION_ID }}"
               ]
            }
         }
      },
      {
         "name":"StopInstance",
         "action":"aws:changeInstanceState",
         "maxAttempts":3,
         "timeoutSeconds":7200,
         "onFailure":"Abort",
         "inputs":{
            "InstanceIds":[
               "{{ LaunchInstance.InstanceIds }}"
            ],
            "CheckStateOnly":false,
            "DesiredState":"stopped"
         }
      },
      {
         "name":"CreateImage",
         "action":"aws:createImage",
         "maxAttempts":3,
         "onFailure":"Abort",
         "inputs":{
            "InstanceId":"{{ LaunchInstance.InstanceIds }}",
            "ImageName":"{{ TargetAmiName }}",
            "NoReboot":true,
            "ImageDescription":"Test CreateImage Description"
         }
      },
      {
         "name":"CreateTags",
         "action":"aws:createTags",
         "maxAttempts":3,
         "onFailure":"Abort",
         "inputs":{
            "ResourceType":"EC2",
            "ResourceIds":[
               "{{ CreateImage.ImageId }}"
            ],
            "Tags":[
               {
                  "Key":"Original_AMI_ID",
                  "Value":"Created from {{ SourceAmiId }}"
               }
            ]
         }
      },
      {
         "name":"TerminateInstance",
         "action":"aws:changeInstanceState",
         "maxAttempts":3,
         "onFailure":"Abort",
         "inputs":{
            "InstanceIds":[
               "{{ LaunchInstance.InstanceIds }}"
            ],
            "DesiredState":"terminated"
         }
      }
   ],
   "outputs":[
      "CreateImage.ImageId"
   ]
}
```

## Automation Execution Failed to Start<a name="automation-trbl-access"></a>

An Automation execution can fail with an access denied error or an invalid assume role error if you have not properly configured IAM users, roles, and policies for Automation\.

### Access Denied<a name="automation-trbl-access-denied"></a>

The following examples describe situations when an Automation execution failed to start with an access denied error\.

**Access Denied to Systems Manager API**  
**Error message**: `User: user arn is not authorized to perform: ssm:StartAutomationExecution on resource: document arn (Service: AWSSimpleSystemsManagement; Status Code: 400; Error Code: AccessDeniedException; Request ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)`
+ Possible cause 1: The IAM user attempting to start the Automation execution does not have permission to invoke the `StartAutomationExecution` API\. To resolve this issue, attach the required IAM policy to the user account that was used to start the execution\. For more information, see [Task 4: Configure User Access to Automation](automation-permissions.md#automation-passrole)\. 
+ Possible cause 2: The IAM user attempting to start the Automation execution has permission to invoke the `StartAutomationExecution` API, but does not have permission to invoke the API by using the specific Automation document\. To resolve this issue, attach the required IAM policy to the user account that was used to start the execution\. For more information, see [Task 4: Configure User Access to Automation](automation-permissions.md#automation-passrole)\.

**Access Denied Because of Missing PassRole Permissions**  
**Error message**: `User: user arn is not authorized to perform: iam:PassRole on resource: automation assume role arn (Service: AWSSimpleSystemsManagement; Status Code: 400; Error Code: AccessDeniedException; Request ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)`

The IAM user attempting to start the Automation execution does not have PassRole permission for the assume role\. To resolve this issue, attach the iam:PassRole policy to the role of the IAM user attempting to start the Automation execution\. For more information, see [Task 3: Attach the iam:PassRole Policy to Your Automation Role](automation-permissions.md#automation-passpolicy)\.

### Invalid Assume Role<a name="automation-trbl-ar"></a>

When you run an Automation, an assume role is either provided in the document or passed as a parameter value for the document\. Different types of errors can occur if the assume role is not specified or configured properly\.

**Malformed Assume Role**  
**Error message**: `The format of the supplied assume role ARN is invalid.` The assume role is improperly formatted\. To resolve this issue, verify that a valid assume role is specified in your Automation document or as a runtime parameter when executing the Automation\.

**Assume Role Can't Be Assumed**  
**Error message**: `The defined assume role is unable to be assumed. (Service: AWSSimpleSystemsManagement; Status Code: 400; Error Code: InvalidAutomationExecutionParametersException; Request ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)`
+ Possible cause 1: The assume role does not exist\. To resolve this issue, create the role\. For more information, see [Setting Up Automation](automation-setup.md)\. Specific details for creating this role are described in the following topic, [Task 1: Create a Service Role for Automation](automation-permissions.md#automation-role)\.
+ Possible cause 2: The assume role does not have a trust relationship with the Systems Manager service\. To resolve this issue, create the trust relationship\. For more information, see [Task 2: Add a Trust Relationship for Automation](automation-permissions.md#automation-trust2)\. 

## Execution Started, but Status is Failed<a name="automation-trbl-exstrt"></a>

### Action\-Specific Failures<a name="automation-trbl-actspec"></a>

Automation documents contain steps and steps run in order\. Each step invokes one or more AWS service APIs\. The APIs determine the inputs, behavior, and outputs of the step\. There are multiple places where an error can cause a step to fail\. Failure messages indicate when and where an error occurred\.

To see a failure message in the EC2 console, choose the **View Outputs** link of the failed step\. To see a failure message from the CLI, call `get-automation-execution` and look for the `FailureMessage` attribute in a failed `StepExecution`\.

In the following examples, a step associated with the `aws:runInstance` action failed\. Each example explores a different type of error\.

**Missing Image**  
**Error message**: `Automation Step Execution fails when it is launching the instance(s). Get Exception from RunInstances API of ec2 Service. Exception Message from RunInstances API: [The image id '[ami id]' does not exist (Service: AmazonEC2; Status Code: 400; Error Code: InvalidAMIID.NotFound; Request ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)]. Please refer to Automation Service Troubleshooting Guide for more diagnosis details.`

The `aws:runInstances` action received input for an `ImageId` that doesn't exist\. To resolve this problem, update the automation document or parameter values with the correct AMI ID\.

**Assume Role Policy Doesn't Have Sufficient Permissions**  
**Error message**: `Automation Step Execution fails when it is launching the instance(s). Get Exception from RunInstances API of ec2 Service. Exception Message from RunInstances API: [You are not authorized to perform this operation. Encoded authorization failure message: xxxxxxx (Service: AmazonEC2; Status Code: 403; Error Code: UnauthorizedOperation; Request ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)]. Please refer to Automation Service Troubleshooting Guide for more diagnosis details.`

The assume role doesn't have sufficient permission to invoke the `RunInstances` API on Amazon EC2 instances\. To resolve this problem, attach an IAM policy to the assume role that has permission to invoke the `RunInstances` API\. For more information, see the [Method 2: Use IAM to Configure Roles for Automation](automation-permissions.md)\.

**Unexpected State**  
**Error message**: `Step fails when it is verifying launched instance(s) are ready to be used. Instance i-xxxxxxxxx entered unexpected state: shutting-down. Please refer to Automation Service Troubleshooting Guide for more diagnosis details.`
+ Possible cause 1: There is a problem with the instance or the Amazon EC2 service\. To resolve this problem, login to the instance or review the instance system log to understand why the instance started shutting down\.
+ Possible cause 2: The user data script specified for the `aws:runInstances` action has a problem or incorrect syntax\. Verify the syntax of the user data script\. Also, verify that the user data scripts doesn't shut down the instance, or invoke other scripts that shut down the instance\.

**Action\-Specific Failures Reference**  
When a step fails, the failure message might indicate which service was being invoked when the failure occurred\. The following table lists the services invoked by each action\. The table also provides links to information about each service\.


****  

| Action | AWS Service\(s\) Invoked by This Action | For Information About This Service | Troubleshooting Content | 
| --- | --- | --- | --- | 
| aws:runInstances | Amazon EC2 | [Amazon EC2 User Guide](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/) | [Troubleshooting EC2 Instances](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-troubleshoot.html) | 
| aws:changeInstanceState | Amazon EC2 | [Amazon EC2 User Guide](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/) | [Troubleshooting EC2 Instances](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-troubleshoot.html) | 
| aws:runCommand | Systems Manager |  [Systems Manager Run Command](http://docs.aws.amazon.com/systems-manager/latest/userguide/execute-remote-commands.html) | [Troubleshooting Run Command](http://docs.aws.amazon.com/systems-manager/latest/userguide/troubleshooting-remote-commands.html) | 
| aws:createImage | Amazon EC2 | [Amazon Machine Images](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) |  | 
| aws:createStack | AWS CloudFormation | [AWS CloudFormation User Guide](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) | [Troubleshooting AWS CloudFormation](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html) | 
| aws:deleteStack | AWS CloudFormation | [AWS CloudFormation User Guide](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) | [Troubleshooting AWS CloudFormation](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html) | 
| aws:deleteImage | Amazon EC2 | [Amazon Machine Images](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) |  | 
| aws:copyImage | Amazon EC2 | [Amazon Machine Images](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) |  | 
| aws:createTag | Amazon EC2, Systems Manager | [EC2 Resource and Tags](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_Resources.html) |  | 
| aws:invokeLambdaFunction | AWS Lambda | [AWS Lambda Developer Guide](http://docs.aws.amazon.com/lambda/latest/dg/) | [Troublshooting Lambda](http://docs.aws.amazon.com/lambda/latest/dg/monitoring-functions.html) | 

### Automation Service Internal Error<a name="automation-trbl-err"></a>

**Error message**: `Internal Server Error. Please refer to Automation Service Troubleshooting Guide for more diagnosis details.`

A problem with the Automation service is preventing the specified Automation document from executing correctly\. To resolve this issue, contact AWS Support\. Provide the execution ID and customer ID, if available\.

## Execution Started, but Timed Out<a name="automation-trbl-to"></a>

**Error message**: `Step timed out while step is verifying launched instance(s) are ready to be used. Please refer to Automation Service Troubleshooting Guide for more diagnosis details.`

A step in the `aws:runInstances` action timed out\. This can happen if the step action takes longer to run than the value specified for `timeoutSeconds` in the step\. To resolve this issue, specify a longer value for `timeoutSeconds`\. If that does not solve the problem, investigate why the step takes longer to run than expected\.