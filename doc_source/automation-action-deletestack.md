# aws:deleteStack – Delete an AWS CloudFormation stack<a name="automation-action-deletestack"></a>

Deletes an AWS CloudFormation stack\.

**Input**

------
#### [ YAML ]

```
name: deleteStack
action: aws:deleteStack
maxAttempts: 1
onFailure: Abort
inputs:
  StackName: "{{stackName}}"
```

------
#### [ JSON ]

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

------

ClientRequestToken  
A unique identifier for this `DeleteStack` request\. Specify this token if you plan to retry requests so that AWS CloudFormation knows that you're not attempting to delete a stack with the same name\. You can retry `DeleteStack` requests to verify that AWS CloudFormation received them\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: \[a\-zA\-Z\]\[\-a\-zA\-Z0\-9\]\*  
Required: No

RetainResources\.member\.N  
This input applies only to stacks that are in a `DELETE_FAILED` state\. A list of logical resource IDs for the resources you want to retain\. During deletion, AWS CloudFormation deletes the stack, but does not delete the retained resources\.  
Retaining resources is useful when you can't delete a resource, such as a non\-empty S3 bucket, but you want to delete the stack\.  
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

## Security considerations<a name="automation-action-deletestack-security"></a>

Before you can use the `aws:deleteStack` action, you must assign the following policy to the IAM Automation assume role\. For more information about the assume role, see [Task 1: Create a service role for Automation](automation-permissions.md#automation-role)\. 

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