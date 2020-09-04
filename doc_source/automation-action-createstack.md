# aws:createStack – Create an AWS CloudFormation stack<a name="automation-action-createstack"></a>

Creates a new AWS CloudFormation stack from a template\.

For supplemental information about creating AWS CloudFormation stacks, see [CreateStack](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_CreateStack.html) in the *AWS CloudFormation API Reference*\. 

**Input**

------
#### [ YAML ]

```
name: makeStack
action: aws:createStack
maxAttempts: 1
onFailure: Abort
inputs:
  Capabilities:
  - CAPABILITY_IAM
  StackName: myStack
  TemplateURL: http://s3.amazonaws.com/doc-example-bucket/myStackTemplate
  TimeoutInMinutes: 5
```

------
#### [ JSON ]

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
        "TemplateURL": "http://s3.amazonaws.com/doc-example-bucket/myStackTemplate",
        "TimeoutInMinutes": 5
    }
}
```

------

Capabilities  
A list of values that you specify before AWS CloudFormation can create certain stacks\. Some stack templates include resources that can affect permissions in your AWS account\. For example, creating new AWS Identity and Access Management \(IAM\) users can affect permissions in your account\. For those stacks, you must explicitly acknowledge their capabilities by specifying this parameter\.   
Valid values include `CAPABILITY_IAM`, `CAPABILITY_NAMED_IAM`, and `CAPABILITY_AUTO_EXPAND`\.   
**CAPABILITY\_IAM and CAPABILITY\_NAMED\_IAM**  
If you have IAM resources, you can specify either capability\. If you have IAM resources with custom names, you must specify `CAPABILITY_NAMED_IAM`\. If you don't specify this parameter, this action returns an `InsufficientCapabilities` error\. The following resources require you to specify either `CAPABILITY_IAM` or `CAPABILITY_NAMED_IAM`\.
+ [AWS::IAM::AccessKey](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-iam-accesskey.html)
+ [AWS::IAM::Group](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-iam-group.html)
+ [AWS::IAM::InstanceProfile](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-iam-instanceprofile.html)
+ [AWS::IAM::Policy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-iam-policy.html)
+ [AWS::IAM::Role](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-iam-role.html)
+ [AWS::IAM::User](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-iam-user.html)
+ [AWS::IAM::UserToGroupAddition](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-iam-addusertogroup.html)
If your stack template contains these resources, we recommend that you review all permissions associated with them and edit their permissions, if necessary\.   
For more information, see [Acknowledging IAM Resources in AWS CloudFormation Templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-iam-template.html#capabilities)\.   
**CAPABILITY\_AUTO\_EXPAND**  
Some template contain macros\. Macros perform custom processing on templates; this can include simple actions like find\-and\-replace operations, all the way to extensive transformations of entire templates\. Because of this, users typically create a change set from the processed template, so that they can review the changes resulting from the macros before actually creating the stack\. If your stack template contains one or more macros, and you choose to create a stack directly from the processed template, without first reviewing the resulting changes in a change set, you must acknowledge this capability\. 
For more information, see [Using AWS CloudFormation Macros to Perform Custom Processing on Templates](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-macros.html) in the *AWS CloudFormation User Guide*\.  
Type: array of Strings  
Valid Values: `CAPABILITY_IAM | CAPABILITY_NAMED_IAM | CAPABILITY_AUTO_EXPAND`  
Required: No

ClientRequestToken  
A unique identifier for this CreateStack request\. Specify this token if you set maxAttempts in this step to a value greater than 1\. By specifying this token, AWS CloudFormation knows that you're not attempting to create a new stack with the same name\.  
Type: String  
Required: No  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: \[a\-zA\-Z0\-9\]\[\-a\-zA\-Z0\-9\]\*

DisableRollback  
Set to `true` to disable rollback of the stack if stack creation failed\.  
Conditional: You can specify either the `DisableRollback` parameter or the `OnFailure` parameter, but not both\.   
Default: `false`  
Type: Boolean  
Required: No

NotificationARNs  
The Amazon SNS topic ARNs for publishing stack\-related events\. You can find SNS topic ARNs using the Amazon SNS console, [https://console\.aws\.amazon\.com/sns/v3/home](https://console.aws.amazon.com/sns/v3/home)\.   
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
A list of `Parameter` structures that specify input parameters for the stack\. For more information, see the [Parameter](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_Parameter.html) data type\.   
Type: array of [Parameter](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_Parameter.html) objects   
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
If the list of resource types doesn't include a resource that you're creating, the stack creation fails\. By default, AWS CloudFormation grants permissions to all resource types\. IAM uses this parameter for AWS CloudFormation\-specific condition keys in IAM policies\. For more information, see [Controlling Access with AWS Identity and Access Management](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-iam-template.html)\.   
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
Structure containing the stack policy body\. For more information, see [Prevent Updates to Stack Resources](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/protect-stack-resources.html)\.  
Conditional: You can specify either the `StackPolicyBody` parameter or the `StackPolicyURL` parameter, but not both\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 16384\.  
Required: No

StackPolicyURL  
Location of a file containing the stack policy\. The URL must point to a policy located in an S3 bucket in the same region as the stack\. The maximum file size allowed for the stack policy is 16 KB\.  
Conditional: You can specify either the `StackPolicyBody` parameter or the `StackPolicyURL` parameter, but not both\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1350\.  
Required: No

Tags  
Key\-value pairs to associate with this stack\. AWS CloudFormation also propagates these tags to the resources created in the stack\. You can specify a maximum number of 10 tags\.   
Type: array of [Tag](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_Tag.html) objects   
Required: No

TemplateBody  
Structure containing the template body with a minimum length of 1 byte and a maximum length of 51,200 bytes\. For more information, see [Template Anatomy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html)\.   
Conditional: You can specify either the `TemplateBody` parameter or the `TemplateURL` parameter, but not both\.   
Type: String  
Length Constraints: Minimum length of 1\.  
Required: No

TemplateURL  
Location of a file containing the template body\. The URL must point to a template that is located in an S3 bucket\. The maximum size allowed for the template is 460,800 bytes\. For more information, see [Template Anatomy](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html)\.   
Conditional: You can specify either the `TemplateBody` parameter or the `TemplateURL` parameter, but not both\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 1024\.  
Required: No

TimeoutInMinutes  
The amount of time that can pass before the stack status becomes `CREATE_FAILED`\. If `DisableRollback` is not set or is set to `false`, the stack will be rolled back\.   
Type: Integer  
Valid Range: Minimum value of 1\.  
Required: No

## Outputs<a name="automation-action-createstack-output"></a>

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
For more information, see [CreateStack](https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_CreateStack.html)\.

## Security considerations<a name="automation-action-createstack-security"></a>

Before you can use the `aws:createStack` action, you must assign the following policy to the IAM Automation assume role\. For more information about the assume role, see [Task 1: Create a service role for Automation](automation-permissions.md#automation-role)\. 

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