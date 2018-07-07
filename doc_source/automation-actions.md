# Systems Manager Automation Document Reference<a name="automation-actions"></a>

This reference describes the actions \(or plugins\) that you can specify in an AWS Systems Manager Automation document\. For information about plugins for other types of SSM documents, see [SSM Document Plugin Reference](ssm-plugins.md)\.

Systems Manager Automation runs steps defined in Automation documents\. Each step is associated with a particular action\. The action determines the inputs, behavior, and outputs of the step\. Steps are defined in the `mainSteps` section of your Automation document\.

You don't need to specify the outputs of an action or step\. The outputs are predetermined by the action associated with the step\. When you specify step inputs in your Automation documents, you can reference one or more outputs from an earlier step\. For example, you can make the output of `aws:runInstances` available for a subsequent `aws:runCommand` action\. You can also reference outputs from earlier steps in the `Output` section of the Automation document\. 

**Topics**
+ [Common Properties In All Actions](#automation-common)
+ [aws:approve](#automation-action-approve)
+ [aws:changeInstanceState](#automation-action-changestate)
+ [aws:copyImage](#automation-action-copyimage)
+ [aws:createImage](#automation-action-create)
+ [aws:createStack](#automation-action-createstack)
+ [aws:createTags](#automation-action-createtag)
+ [aws:deleteImage](#automation-action-delete)
+ [aws:deleteStack](#automation-action-deletestack)
+ [aws:executeAutomation](#automation-action-executeAutomation)
+ [aws:executeStateMachine](#automation-action-executeStateMachine)
+ [aws:invokeLambdaFunction](#automation-action-lamb)
+ [aws:pause](#automation-action-pause)
+ [aws:runCommand](#automation-action-runcommand)
+ [aws:runInstances](#automation-action-runinstance)
+ [aws:sleep](#automation-action-sleep)

## Common Properties In All Actions<a name="automation-common"></a>

The following properties are common to all actions:

```
"mainSteps": [
    {
        "name": "name",
        "action": "action",
        "maxAttempts": value,
        "timeoutSeconds": value,
        "onFailure": "Abort",
        "inputs": {
            ...
        }
    },
    {
        "name": "name",
        "action": "action",
        "maxAttempts": value,
        "timeoutSeconds": value,
        "onFailure": "Abort",
        "inputs": {
            ...
        }
    }
]
```

name  
An identifier that must be unique across all step names in the document\.  
Type: String  
Required: Yes

action  
The name of the action the step is to execute\.  
Type: String  
Required: Yes

maxAttempts  
The number of times the step should be retried in case of failure\. If the value is greater than 1, the step is not considered to have failed until all retry attempts have failed\. The default value is 1\.  
Type: Integer  
Required: No

timeoutSeconds  
The execution timeout value for the step\. If the timeout is reached and the value of `maxAttempts` is greater than 1, then the step is not considered to have timed out until all retries have been attempted\. There is no default value for this field\.  
Type: Integer  
Required: No

onFailure  
Indicates whether the workflow should continue on failure\. The default is to abort on failure\.  
Type: String  
Valid values: Abort \| Continue  
Required: No

inputs  
The properties specific to the action\.  
Type: Map  
Required: Yes

## aws:approve<a name="automation-action-approve"></a>

Temporarily pauses an Automation execution until designated principals either approve or reject the action\. After the required number of approvals is reached, the Automation execution resumes\. You can insert the approval step any place in the mainSteps section of your Automation document\. 

In the following example, the aws:approve action temporarily pauses the Automation workflow until one approver either accepts or rejects the workflow\. Upon approval, the document runs a simple PowerShell command\. 

```
{
   "description":"RunInstancesDemo1",
   "schemaVersion":"0.3",
   "assumeRole":"{{ assumeRole }}",
   "parameters":{
      "assumeRole":{
         "type":"String"
      },
      "message":{
         "type":"String"
      }
   },
   "mainSteps":[
      {
         "name":"approve",
         "action":"aws:approve",
         "timeoutSeconds":1000,
         "onFailure":"Abort",
         "inputs":{
            "NotificationArn":"arn:aws:sns:us-east-2:12345678901:AutomationApproval",
            "Message":"{{ message }}",
            "MinRequiredApprovals":1,
            "Approvers":[
               "arn:aws:iam::12345678901:user/AWS-User-1"
            ]
         }
      },
      {
         "name":"run",
         "action":"aws:runCommand",
         "inputs":{
            "InstanceIds":[
               "i-1a2b3c4d5e6f7g"
            ],
            "DocumentName":"AWS-RunPowerShellScript",
            "Parameters":{
               "commands":[
                  "date"
               ]
            }
         }
      }
   ]
}
```

You can approve or deny Automations that are waiting for approval in the console\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To approve or deny waiting Automations \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Automation**\.

1. Choose the option beside an Automation with a status of **Waiting**\.  
![\[Accessing the Approve/Deny Automation page\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/automation-approve-action-aws.png)

1. Choose **Approve/Deny**\.

1. Review the details of the Automation\.

1. Choose either **Approve** or **Deny**, type an optional comment, and then choose **Submit**\.

**To approve or deny waiting Automations\(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Services** in the navigation pane, and then choose **Automations**\.

1. Choose an Automation with a status of **Waiting**, choose **Actions**, and then choose **Approve/Deny this request**\.  
![\[Accessing the Approve/Deny Automation page\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/automation-approve-action1.png)

1. Review the details of the Automation in the **Approve/Deny this request** page\.  
![\[A prompt to approve or reject an Automation action\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/automation-approve-action.png)

1. Choose either **Approve** or **Reject**, type an optional comment, and then choose **Submit**\.

**Input**

```
{
   "NotificationArn":"arn:aws:sns:us-west-1:12345678901:Automation-ApprovalRequest",
   "Message":"Please approve this step of the Automation.",
   "MinRequiredApprovals":3,
   "Approvers":[
      "IamUser1",
      "IamUser2",
      "arn:aws:iam::12345678901:user/IamUser3",
      "arn:aws:iam::12345678901:role/IamRole"
   ]
}
```

NotificationArn  
The ARN of an Amazon SNS topic for Automation approvals\. When you specify an `aws:approve` step in an Automation document, Automation sends a message to this topic letting principals know that they must either approve or reject an Automation step\. The title of the Amazon SNS topic must be prefixed with "Automation"\.  
Type: String  
Required: No

Message  
The information you want to include in the SNS topic when the approval request is sent\. The maximum message length is 4096 characters\.   
Type: String  
Required: No

MinRequiredApprovals  
The minimum number of approvals required to resume the Automation execution\. If you don't specify a value, the system defaults to one\. The value for this parameter must be a positive number\. The value for this parameter can't exceed the number of approvers defined by the `Approvers` parameter\.   
Type: Integer  
Required: No

Approvers  
A list of AWS authenticated principals who are able to either approve or reject the action\. The maximum number of approvers is 10\. You can specify principals by using any of the following formats:  
+ An AWS Identity and Access Management \(IAM\) user name
+ An IAM user ARN
+ An IAM role ARN
+ An IAM assume role user ARN
Type: StringList  
Required: Yes

**Output**

ApprovalStatus  
The approval status of the step\. The status can be one of the following: Approved, Rejected, or Waiting\. Waiting means that Automation is waiting for input from approvers\.  
Type: String

ApproverDecisions  
A JSON map that includes the approval decision of each approver\.  
Type: MapList

## aws:changeInstanceState<a name="automation-action-changestate"></a>

Changes or asserts the state of the instance\.

This action can be used in assert mode \(do not execute the API to change the state but verify the instance is in the desired state\.\) To use assert mode, set the CheckStateOnly parameter to true\. This mode is useful when running the Sysprep command on Windows, which is an asynchronous command that can run in the background for a long time\. You can ensure that the instance is stopped before you create an AMI\.

**Input**

```
{
    "name":"stopMyInstance",
    "action": "aws:changeInstanceState",
    "maxAttempts": 3,
    "timeoutSeconds": 3600,
    "onFailure": "Abort",
    "inputs": {
        "InstanceIds": ["i-1234567890abcdef0"],
        "CheckStateOnly": true,
        "DesiredState": "stopped"
    }
}
```

InstanceIds  
The IDs of the instances\.  
Type: StringList  
Required: Yes

CheckStateOnly  
If false, sets the instance state to the desired state\. If true, asserts the desired state using polling\.  
Type: Boolean  
Required: No

DesiredState  
The desired state\.  
Type: String  
Valid values: `running` \| `stopped` \| `terminated`  
Required: Yes

Force  
If set, forces the instances to stop\. The instances do not have an opportunity to flush file system caches or file system metadata\. If you use this option, you must perform file system check and repair procedures\. This option is not recommended for Windows instances\.  
Type: Boolean  
Required: No

AdditionalInfo  
Reserved\.  
Type: String  
Required: No

**Output**  
None

## aws:copyImage<a name="automation-action-copyimage"></a>

Copies an AMI from any region into the current region\. This action can also encrypt the new AMI\.

**Input**  
This action supports most CopyImage parameters\. For more information, see [CopyImage](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CopyImage.html)\.

The following example creates a copy of an AMI in the Seoul region \(`SourceImageID`: ami\-0fe10819\. `SourceRegion`: ap\-northeast\-2\)\. The new AMI is copied to the region where you initiated the Automation action\. The copied AMI will be encrypted because the optional `Encrypted` flag is set to `true`\.

```
{   
    "name": "createEncryptedCopy",
    "action": "aws:copyImage",
    "maxAttempts": 3,
    "onFailure": "Abort",
    "inputs": {
        "SourceImageId": "ami-0fe10819",
        "SourceRegion": "ap-northeast-2",
        "ImageName": "Encrypted Copy of LAMP base AMI in ap-northeast-2",
        "Encrypted": true
    }   
}
```

SourceRegion  
The region where the source AMI currently exists\.  
Type: String  
Required: Yes

SourceImageId  
The AMI ID to copy from the source region\.  
Type: String  
Required: Yes

ImageName  
The name for the new image\.  
Type: String  
Required: Yes

ImageDescription  
A description for the target image\.  
Type: String  
Required: No

Encrypted  
Encrypt the target AMI\.  
Type: Boolean  
Required: No

KmsKeyId  
The full Amazon Resource Name \(ARN\) of the AWS Key Management Service CMK to use when encrypting the snapshots of an image during a copy operation\. For more information, see [CopyImage](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/api_copyimage.html)\.  
Type: String  
Required: No

ClientToken  
A unique, case\-sensitive identifier that you provide to ensure request idempotency\. For more information, see [CopyImage](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/api_copyimage.html)\.  
Type: String  
Required: NoOutput

ImageId  
The ID of the copied image\.

ImageState  
The state of the copied image\.  
Valid values: `available` \| `pending` \| `failed`

## aws:createImage<a name="automation-action-create"></a>

Creates a new AMI from a stopped instance\.

**Important**  
This action does not stop the instance implicitly\. You must use the aws:changeInstanceState action to stop the instance\. If this action is used on a running instance, the resultant AMI might be defective\.

**Input**  
This action supports most CreateImage parameters\. For more information, see [CreateImage](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_CreateImage.html)\.

```
{
    "name": "createMyImage",
    "action": "aws:createImage",
    "maxAttempts": 3,
    "onFailure": "Abort",
    "inputs": {
        "InstanceId": "i-1234567890abcdef0",
        "ImageName": "AMI Created on{{global:DATE_TIME}}",
        "NoReboot": true,
        "ImageDescription": "My newly created AMI"
    }
}
```

InstanceId  
The ID of the instance\.  
Type: String  
Required: Yes

ImageName  
The name of the image\.  
Type: String  
Required: Yes

ImageDescription  
A description of the image\.  
Type: String  
Required: No

NoReboot  
A boolean literal\.  
Type: Boolean  
Required: No

BlockDeviceMappings  
The block devices for the instance\.  
Type: Map  
Required: NoOutput

ImageId  
The ID of the newly created image\.

ImageState  
An execution script provided as a string literal value\. If a literal value is entered, then it must be Base64\-encoded\.  
Required: No

## aws:createStack<a name="automation-action-createstack"></a>

Creates a new AWS CloudFormation stack from a template\.

**Input**

```
{
      "name": "makeStack",
      "action": "aws:createStack",
      "maxAttempts": 1,
      "onFailure": "Abort",
      "inputs": {
        "Capabilities": [
          "CAPABILITY_IAM"
        ],
        "StackName": "myStack",
        "TemplateURL": "http://s3.amazonaws.com/mybucket/myStackTemplate",
        "TimeoutInMinutes": 5
      }
    }
```

Capabilities  
A list of values that you specify before AWS CloudFormation can create certain stacks\. Some stack templates include resources that can affect permissions in your AWS account\. For example, creating new AWS Identity and Access Management \(IAM\) users can affect permissions in your account\. For those stacks, you must explicitly acknowledge their capabilities by specifying this parameter\.   
The only valid values are `CAPABILITY_IAM` and `CAPABILITY_NAMED_IAM`\. The following resources require you to specify this parameter\.  
+ [AWS::IAM::AccessKey](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-iam-accesskey.html)
+ [AWS::IAM::Group](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-iam-group.html)
+ [AWS::IAM::InstanceProfile](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-iam-instanceprofile.html)
+ [AWS::IAM::Policy](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-iam-policy.html)
+ [AWS::IAM::Role](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-iam-role.html)
+ [AWS::IAM::User](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-iam-user.html)
+ [AWS::IAM::UserToGroupAddition](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-iam-addusertogroup.html)
If your stack template contains these resources, we recommend that you review all permissions associated with them and edit their permissions, if necessary\.   
If you have IAM resources, you can specify either capability\. If you have IAM resources with custom names, you must specify `CAPABILITY_NAMED_IAM`\. If you don't specify this parameter, this action returns an `InsufficientCapabilities` error\.   
For more information, see [Acknowledging IAM Resources in AWS CloudFormation Templates](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-iam-template.html#capabilities)\.   
Type: array of Strings  
Valid Values: `CAPABILITY_IAM | CAPABILITY_NAMED_IAM`  
Required: No

DisableRollback  
Set to `true` to disable rollback of the stack if stack creation failed\.  
Conditional: You can specify either the `DisableRollback` parameter or the `OnFailure` parameter, but not both\.   
Default: false  
Type: Boolean  
Required: No

NotificationARNs  
The Amazon SNS topic ARNs for publishing stack\-related events\. You can find SNS topic ARNs using the Amazon SNS console, [https://console\.aws\.amazon\.com/sns/v2/home](https://console.aws.amazon.com/sns/v2/home)\.   
Type: array of Strings  
Array Members: Maximum number of 5 items\.  
Required: No

OnFailure  
Determines the action to take if stack creation failed\. You must specify `DO_NOTHING`, `ROLLBACK`, or `DELETE`\.  
Conditional: You can specify either the `OnFailure` parameter or the `DisableRollback` parameter, but not both\.   
Default: `ROLLBACK`  
Type: String  
Valid Values:` DO_NOTHING | ROLLBACK | DELETE`  
Required: No

Parameters  
A list of `Parameter` structures that specify input parameters for the stack\. For more information, see the [Parameter](http://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_Parameter.html) data type\.   
Type: array of [Parameter](http://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_Parameter.html) objects   
Required: No

ResourceTypes  
The template resource types that you have permissions to work with for this create stack action\. For example: `AWS::EC2::Instance`, `AWS::EC2::*`, or `Custom::MyCustomInstance`\. Use the following syntax to describe template resource types\.  
+ For all AWS resources:

  ```
  AWS::*
  ```
+ For all custom resources:

  ```
  Custom::*
  ```
+ For a specific custom resource:

  ```
  Custom::logical_ID
  ```
+ For all resources of a particular AWS service:

  ```
  AWS::service_name::*
  ```
+ For a specific AWS resource:

  ```
  AWS::service_name::resource_logical_ID
  ```
If the list of resource types doesn't include a resource that you're creating, the stack creation fails\. By default, AWS CloudFormation grants permissions to all resource types\. IAM uses this parameter for AWS CloudFormation\-specific condition keys in IAM policies\. For more information, see [Controlling Access with AWS Identity and Access Management](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-iam-template.html)\.   
Type: array of Strings  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Required: No

RoleARN  
The Amazon Resource Name \(ARN\) of an IAM role that AWS CloudFormation assumes to create the stack\. AWS CloudFormation uses the role's credentials to make calls on your behalf\. AWS CloudFormation always uses this role for all future operations on the stack\. As long as users have permission to operate on the stack, AWS CloudFormation uses this role even if the users don't have permission to pass it\. Ensure that the role grants the least amount of privileges\.   
If you don't specify a value, AWS CloudFormation uses the role that was previously associated with the stack\. If no role is available, AWS CloudFormation uses a temporary session that is generated from your user credentials\.   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Required: No

StackName  
The name that is associated with the stack\. The name must be unique in the region in which you are creating the stack\.  
A stack name can contain only alphanumeric characters \(case sensitive\) and hyphens\. It must start with an alphabetic character and cannot be longer than 128 characters\. 
Type: String  
Required: Yes

StackPolicyBody  
Structure containing the stack policy body\. For more information, see [Prevent Updates to Stack Resources](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/protect-stack-resources.html)\.  
Conditional: You can specify either the `StackPolicyBody` parameter or the `StackPolicyURL` parameter, but not both\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 16384\.  
Required: No

StackPolicyURL  
Location of a file containing the stack policy\. The URL must point to a policy located in an Amazon S3 bucket in the same region as the stack\. The maximum file size allowed for the stack policy is 16 KB\.  
Conditional: You can specify either the `StackPolicyBody` parameter or the `StackPolicyURL` parameter, but not both\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1350\.  
Required: No

Tags  
Key\-value pairs to associate with this stack\. AWS CloudFormation also propagates these tags to the resources created in the stack\. You can specify a maximum number of 10 tags\.   
Type: array of [Tag](http://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_Tag.html) objects   
Required: No

TemplateBody  
Structure containing the template body with a minimum length of 1 byte and a maximum length of 51,200 bytes\. For more information, see [Template Anatomy](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html)\.   
Conditional: You can specify either the `TemplateBody` parameter or the `TemplateURL` parameter, but not both\.   
Type: String  
Length Constraints: Minimum length of 1\.  
Required: No

TemplateURL  
Location of a file containing the template body\. The URL must point to a template that is located in an Amazon S3 bucket\. The maximum size allowed for the template is 460,800 bytes\. For more information, see [Template Anatomy](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html)\.   
Conditional: You can specify either the `TemplateBody` parameter or the `TemplateURL` parameter, but not both\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Required: No

TimeoutInMinutes  
The amount of time that can pass before the stack status becomes `CREATE_FAILED`\. If `DisableRollback` is not set or is set to `false`, the stack will be rolled back\.   
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: No

### Outputs<a name="automation-action-createstack-output"></a>

StackId  
Unique identifier of the stack\.  
Type: String

StackStatus  
Current status of the stack\.  
Type: String  
Valid Values: `CREATE_IN_PROGRESS | CREATE_FAILED | CREATE_COMPLETE | ROLLBACK_IN_PROGRESS | ROLLBACK_FAILED | ROLLBACK_COMPLETE | DELETE_IN_PROGRESS | DELETE_FAILED | DELETE_COMPLETE | UPDATE_IN_PROGRESS | UPDATE_COMPLETE_CLEANUP_IN_PROGRESS | UPDATE_COMPLETE | UPDATE_ROLLBACK_IN_PROGRESS | UPDATE_ROLLBACK_FAILED | UPDATE_ROLLBACK_COMPLETE_CLEANUP_IN_PROGRESS | UPDATE_ROLLBACK_COMPLETE | REVIEW_IN_PROGRESS`  
Required: Yes

StackStatusReason  
Success or failure message associated with the stack status\.  
Type: String  
Required: No  
For more information, see [CreateStack](http://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_CreateStack.html)\.

### Security Considerations<a name="automation-action-createstack-security"></a>

Before you can use the `aws:createStack` action, you must assign the following policy to the IAM Automation assume role\. For more information about the assume role, see [Task 1: Create a Service Role for Automation](automation-permissions.md#automation-role)\. 

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "sqs:*",
            "cloudformation:CreateStack",
            "cloudformation:DescribeStacks"
         ],
         "Resource":"*"
      }
   ]
}
```

## aws:createTags<a name="automation-action-createtag"></a>

Create new tags for Amazon EC2 instances or Systems Manager managed instances\.

**Input**  
This action supports most EC2 CreateTags and SSM AddTagsToResource parameters\. For more information, see [CreateTags](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/api_createtags.html) and [AddTagsToResource](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/api_addtagstoresource.html)\.

The following example shows how to tag an AMI and an instance as being production resources for a particular department\.

```
{
        "name": "createTags",
        "action": "aws:createTags",
        "maxAttempts": 3,
        "onFailure": "Abort",
        "inputs": {
            "ResourceType": "EC2",
            "ResourceIds": [
                "ami-9a3768fa",
                "i-02951acd5111a8169"
            ],
            "Tags": [
                {
                    "Key": "production",
                    "Value": ""
                },
                {
                    "Key": "department",
                    "Value": "devops"
                }
            ]
        }
    }
```

ResourceIds  
The IDs of the resource\(s\) to be tagged\. If resource type is not “EC2”, this field can contain only a single item\.  
Type: String List  
Required: Yes

Tags  
The tags to associate with the resource\(s\)\.  
Type: List of Maps  
Required: Yes

ResourceType  
The type of resource\(s\) to be tagged\. If not supplied, the default value of “EC2” is used\.  
Type: String  
Required: No  
Valid Values: `EC2` \| `ManagedInstance` \| `MaintenanceWindow` \| `Parameter`

**Output**  
None

## aws:deleteImage<a name="automation-action-delete"></a>

Deletes the specified image and all related snapshots\.

**Input**  
This action supports only one parameter\. For more information, see the documentation for [DeregisterImage](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DeregisterImage.html) and [DeleteSnapshot](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_DeleteSnapshot.html)\.

```
{
    "name": "deleteMyImage",
    "action": "aws:deleteImage",
    "maxAttempts": 3,
    "timeoutSeconds": 180,
    "onFailure": "Abort",
    "inputs": {
        "ImageId": "ami-12345678"
    }
}
```

ImageId  
The ID of the image to be deleted\.  
Type: String  
Required: Yes

**Output**  
None

## aws:deleteStack<a name="automation-action-deletestack"></a>

Deletes an AWS CloudFormation stack\.

**Input**

```
{
   "name":"deleteStack",
   "action":"aws:deleteStack",
   "maxAttempts":1,
   "onFailure":"Abort",
   "inputs":{
      "StackName":"{{stackName}}"
   }
}
```

ClientRequestToken  
A unique identifier for this `DeleteStack` request\. Specify this token if you plan to retry requests so that AWS CloudFormation knows that you're not attempting to delete a stack with the same name\. You can retry `DeleteStack` requests to verify that AWS CloudFormation received them\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: \[a\-zA\-Z\]\[\-a\-zA\-Z0\-9\]\*  
Required: No

RetainResources\.member\.N  
This input applies only to stacks that are in a `DELETE_FAILED` state\. A list of logical resource IDs for the resources you want to retain\. During deletion, AWS CloudFormation deletes the stack, but does not delete the retained resources\.  
Retaining resources is useful when you can't delete a resource, such as a non\-empty Amazon S3 bucket, but you want to delete the stack\.  
Type: array of strings  
Required: No

RoleARN  
The Amazon Resource Name \(ARN\) of an IAM role that AWS CloudFormation assumes to create the stack\. AWS CloudFormation uses the role's credentials to make calls on your behalf\. AWS CloudFormation always uses this role for all future operations on the stack\. As long as users have permission to operate on the stack, AWS CloudFormation uses this role even if the users don't have permission to pass it\. Ensure that the role grants the least amount of privileges\.   
If you don't specify a value, AWS CloudFormation uses the role that was previously associated with the stack\. If no role is available, AWS CloudFormation uses a temporary session that is generated from your user credentials\.   
Type: String  
Length Constraints: Minimum length of 20\. Maximum length of 2048\.  
Required: No

StackName  
The name or the unique stack ID that is associated with the stack\.  
Type: String  
Required: Yes

### Security Considerations<a name="automation-action-deletestack-security"></a>

Before you can use the `aws:deleteStack` action, you must assign the following policy to the IAM Automation assume role\. For more information about the assume role, see [Task 1: Create a Service Role for Automation](automation-permissions.md#automation-role)\. 

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "sqs:*",
            "cloudformation:DeleteStack",
            "cloudformation:DescribeStacks"
         ],
         "Resource":"*"
      }
   ]
}
```

## aws:executeAutomation<a name="automation-action-executeAutomation"></a>

Executes a secondary Automation workflow by calling a secondary Automation document\. With this action, you can create Automation documents for your most common workflows, and reference those documents during an Automation execution\. This action can simplify your Automation documents by removing the need to duplicate steps across similar documents\.

The secondary Automation runs in the context of the user who initiated the primary Automation\. This means that the secondary Automation uses the same IAM role or user account as the user who started the first Automation\.

**Important**  
If you specify parameters in a secondary Automation that use an assume role \(a role that uses the iam:passRole policy\), then the user or role that initiated the primary Automation must have permission to pass the assume role specified in the secondary Automation\. For more information about setting up an assume role for Automation, see [Method 2: Use IAM to Configure Roles for Automation](automation-permissions.md)\.

**Input**

```
{
   "name":"Secondary_Automation_Workflow",
   "action":"aws:executeAutomation",
   "maxAttempts":3,
   "timeoutSeconds":3600,
   "onFailure":"Abort",
   "inputs":{
      "DocumentName":"secondaryWorkflow",
      "RuntimeParameters":{
         "instanceIds":[
            "i-1234567890abcdef0"
         ]
      }
   }
}
```

DocumentName  
The name of the secondary Automation document to execute during the step\. The document must belong to the same AWS account as the primary Automation document\.  
Type: String  
Required: Yes

DocumentVersion  
The version of the secondary Automation document to execute\. If not specified, Automation runs the default document version\.  
Type: String  
Required: Yes

RuntimeParameters  
Required parameters for the secondary document execution\. The mapping uses the following format: \{"parameter1" : \["value1"\], "parameter2" : \["value2"\] \}  
Type: Map  
Required: NoOutput

Output  
The output generated by the secondary execution\. You can reference the output by using the following format: *Secondary\_Automation\_Step\_Name*\.Output  
Type: StringList

ExecutionId  
The execution ID of the secondary execution\.  
Type: String

Status  
The status of the secondary execution\.  
Type: String

## aws:executeStateMachine<a name="automation-action-executeStateMachine"></a>

Executes an AWS Step Functions state machine\.

**Input**

This action supports most parameters for the Step Functions [StartExecution](http://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html) API action\.

```
{
    "name": "executeTheStateMachine",
    "action": "aws:executeStateMachine",
    "inputs": {
        "stateMachineArn": "StateMachine_ARN",
        "input": "{\"parameters\":\"values\"}",
        "name": "name"
    }
}
```

stateMachineArn  
The ARN of the Step Functions state machine\.  
Type: String  
Required: Yes

name  
The name of the execution\.  
Type: String  
Required: No

input  
A string that contains the JSON input data for the execution\.  
Type: String  
Required: No

## aws:invokeLambdaFunction<a name="automation-action-lamb"></a>

Invokes the specified Lambda function\.

**Input**  
This action supports most invoke parameters for the Lambda service\. For more information, see [Invoke](http://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html)\.

```
{
    "name": "invokeMyLambdaFunction",
    "action": "aws:invokeLambdaFunction",
    "maxAttempts": 3,
    "timeoutSeconds": 120,
    "onFailure": "Abort",
    "inputs": {
        "FunctionName": "MyLambdaFunction"
    }
}
```

FunctionName  
The name of the Lambda function\. This function must exist\.  
Type: String  
Required: Yes

Qualifier  
The function version or alias name\.  
Type: String  
Required: No

InvocationType  
The invocation type\. The default is `RequestResponse`\.  
Type: String  
Valid values: `Event` \| `RequestResponse` \| `DryRun`  
Required: No

LogType  
If `Tail`, the invocation type must be `RequestResponse`\. AWS Lambda returns the last 4 KB of log data produced by your Lambda function, base64\-encoded\.  
Type: String  
Valid values: `None` \| `Tail`  
Required: No

ClientContext  
The client\-specific information\.  
Required: No

Payload  
The JSON input for your Lambda function\.  
Required: NoOutput

StatusCode  
The function execution status code\.

FunctionError  
Indicates whether an error occurred while executing the Lambda function\. If an error occurred, this field will show either `Handled` or `Unhandled`\. `Handled` errors are reported by the function\. `Unhandled` errors are detected and reported by AWS Lambda\.

LogResult  
The base64\-encoded logs for the Lambda function invocation\. Logs are present only if the invocation type is `RequestResponse`, and the logs were requested\.

Payload  
The JSON representation of the object returned by the Lambda function\. Payload is present only if the invocation type is `RequestResponse`\.

## aws:pause<a name="automation-action-pause"></a>

This action pauses the Automation execution\. Once paused, the execution status is *Waiting*\. To continue the Automation execution, use the [SendAutomationSignal](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendAutomationSignal.html) API action with the Resume signal type\. 

**Input**  
The input is as follows\.

```
{
    "name": "pauseThis",
    "action": "aws:pause",
    "inputs": {}
}
```Output

None  

## aws:runCommand<a name="automation-action-runcommand"></a>

Runs the specified commands\.

**Note**  
Automation only supports *output* of one Run Command action\. A document can include multiple Run Command actions and plugins, but output is supported for only one action and plugin at a time\.

**Input**  
This action supports most send command parameters\. For more information, see [SendCommand](http://docs.aws.amazon.com/ssm/latest/APIReference/API_SendCommand.html)\.

```
{
    "name": "installPowerShellModule",
    "action": "aws:runCommand",
    "inputs": {
        "DocumentName": "AWS-InstallPowerShellModule",
        "InstanceIds": ["i-1234567890abcdef0"],
        "Parameters": {
            "source": "https://my-s3-url.com/MyModule.zip ",
            "sourceHash": "ASDFWER12321WRW"
        }
    }
}
```

DocumentName  
The name of the Run Command document\.  
Type: String  
Required: Yes

InstanceIds  
The IDs of the instances\.  
Type: String  
Required: Yes

Parameters  
The required and optional parameters specified in the document\.  
Type: Map  
Required: No

Comment  
User\-defined information about the command\.  
Type: String  
Required: No

DocumentHash  
The hash for the document\.  
Type: String  
Required: No

DocumentHashType  
The type of the hash\.  
Type: String  
Valid values: `Sha256` \| `Sha1`  
Required: No

NotificationConfig  
The configurations for sending notifications\.  
Required: No

OutputS3BucketName  
The name of the S3 bucket for command execution responses\.  
Type: String  
Required: No

OutputS3KeyPrefix  
The prefix\.  
Type: String  
Required: No

ServiceRoleArn  
The ARN of the IAM role\.  
Type: String  
Required: No

TimeoutSeconds  
The run\-command timeout value, in seconds\.  
Type: Integer  
Required: NoOutput

CommandId  
The ID of the command\.

Status  
The status of the command\.

ResponseCode  
The response code of the command\.

Output  
The output of the command\.

## aws:runInstances<a name="automation-action-runinstance"></a>

Launches a new instance\.

**Input**  
The action supports most API parameters\. For more information, see the [RunInstances](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_RunInstances.html) API documentation\.

```
{
    "name": "launchInstance",
    "action": "aws:runInstances",
    "maxAttempts": 3,
    "timeoutSeconds": 1200,
    "onFailure": "Abort",
    "inputs": {
        "ImageId": "ami-12345678",
        "InstanceType": "t2.micro",
        "MinInstanceCount": 1,
        "MaxInstanceCount": 1,
        "IamInstanceProfileName": "myRunCmdRole"
    }
}
```

ImageId  
The ID of the Amazon Machine Image \(AMI\)\.  
Type: String  
Required: Yes

InstanceType  
The instance type\.  
Type: String  
Required: No

MinInstanceCount  
The minimum number of instances to be launched\.  
Type: String  
Required: No

MaxInstanceCount  
The maximum number of instances to be launched\.  
Type: String  
Required: No

AdditionalInfo  
Reserved\.  
Type: String  
Required: No

BlockDeviceMappings  
The block devices for the instance\.  
Type: MapList  
Required: No

ClientToken  
The identifier to ensure idempotency of the request\.  
Type: String  
Required: No

DisableApiTermination  
Enables or disables instance API termination  
Type: Boolean  
Required: No

EbsOptimized  
Enables or disabled EBS optimization\.  
Type: Boolean  
Required: No

IamInstanceProfileArn  
The ARN of the IAM instance profile for the instance\.  
Type: String  
Required: No

IamInstanceProfileName  
The name of the IAM instance profile for the instance\.  
Type: String  
Required: No

InstanceInitiatedShutdownBehavior  
Indicates whether the instance stops or terminates on system shutdown\.  
Type: String  
Required: No

KernelId  
The ID of the kernel\.  
Type: String  
Required: No

KeyName  
The name of the key pair\.  
Type: String  
Required: No

MaxInstanceCount  
The maximum number of instances to filter when searching for offerings\.  
Type: Integer  
Required: No

MinInstanceCount  
The minimum number of instances to filter when searching for offerings\.  
Type: Integer  
Required: No

Monitoring  
Enables or disables detailed monitoring\.  
Type: Boolean  
Required: No

NetworkInterfaces  
The network interfaces\.  
Type: MapList  
Required: No

Placement  
The placement for the instance\.  
Type: StringMap  
Required: No

PrivateIpAddress  
The primary IPv4 address\.  
Type: String  
Required: No

RamdiskId  
The ID of the RAM disk\.  
Type: String  
Required: No

SecurityGroupIds  
The IDs of the security groups for the instance\.  
Type: StringList  
Required: No

SecurityGroups  
The names of the security groups for the instance\.  
Type: StringList  
Required: No

SubnetId  
The subnet ID\.  
Type: String  
Required: No

UserData  
An execution script provided as a string literal value\. If a literal value is entered, then it must be Base64\-encoded\.  
Type: String  
Required: NoOutput

InstanceIds  
The IDs of the instances\.

## aws:sleep<a name="automation-action-sleep"></a>

Delays Automation execution for a specified amount of time\. This action uses the International Organization for Standardization \(ISO\) 8601 date and time format\. For more information about this date and time format, see [ISO 8601](https://www.iso.org/iso-8601-date-and-time-format.html)\.

**Input**  
You can delay execution for a specified duration\. 

```
{
   "name":"sleep",
   "action":"aws:sleep",
   "inputs":{
      "Duration":"PT10M"
   }
}
```

You can also delay execution until a specified date and time\. If the specified date and time has passed, the action proceeds immediately\. 

```
{
    "name": "sleep",
    "action": "aws:sleep",
    "inputs": {
        "Timestamp": "2020-01-01T01:00:00Z"
    }
}
```

**Note**  
Automation currently supports a maximum delay of 604800 seconds \(7 days\)\.

Duration  
An ISO 8601 duration\. You can't specify a negative duration\.   
Type: String  
Required: No

Timestamp  
An ISO 8601 timestamp\. If you don't specify a value for this parameter, then you must specify a value for the `Duration` parameter\.   
Type: String  
Required: NoOutput

None  