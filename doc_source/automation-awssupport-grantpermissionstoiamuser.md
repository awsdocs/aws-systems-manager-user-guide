# AWSSupport-GrantPermissionsToIAMUser

## Description

The AWSSupport-GrantPermissionsToIAMUser Automation document assigns the specified permissions to a new or existing IAM group, and then adds an existing IAM user to the group. Policies you can choose from: Billing (<https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/job-function/Billing$serviceLevelSummary>), Support (<https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AWSSupportAccess$serviceLevelSummary>).

To enable billing access for IAM, be sure to activate **IAM user and federated user access to the Billing and Cost Management pages**: <http://docs.aws.amazon.com/console/iam/billing-enable>.

**WARNING**: If you specify an existing IAM group, all current IAM users in the group receive the new permissions.

AWSSupport-GrantPermissionsToIAMUser follows IAM best practices:

- Reminds the AWS administrator to enable access to Billing Data if the permissions are billing related
- Uses managed policies to configure access
- Assigns the managed policies to an IAM group instead of the IAM user.

## Document Type

Automation

## Owner

Amazon

## Parameters

### IAMGroupName

- **Type**: String
- **Allowed pattern**: ^[a-zA-Z0-9+=,.@_-]{1,128}$
- **Default**: ExampleSupportAndBillingGroup
- **Description**: (Required) Can be a new or existing group. Must comply with <https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_iam-limits.html#reference_iam-limits-names>.

### IAMUserName

- **Type**: String
- **Allowed pattern**: ^[a-zA-Z0-9+=,.@_-]{1,64}
- **Default**: ExampleUser
- **Description**: (Required) Must be an existing user.

### Permissions

- **Type**: String
- **Allowed values**: SupportFullAccess, BillingFullAccess, SupportAndBillingFullAccess
- **Default**: SupportAndBillingFullAccess
- **Description**: (Required) The permissions you want to apply to the IAM group/user. SupportFullAccess: Grants full access to the Support center | BillingFullAccess: Grants full access to the Billing dashboard | SupportAndBillingFullAccess: Grants full access to both Support center and the Billing dashboard | More info: <https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/job-function/Billing$serviceLevelSummary>, <https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AWSSupportAccess$serviceLevelSummary>.

### LambdaAssumeRole

- **Type**: String
- **Description**: (Optional) The ARN of the role assumed by AWS Lambda.

### AutomationAssumeRole

- **Type**: String
- **Description**: (Optional) The ARN of the role that allows Automation to perform the actions on your behalf. If no role is specified, Systems Manager Automation uses the permissions of the user that executes this document.

## Examples

### Add IAM user BillingUser to IAM group BillingGroup and grant full access to the AWS Billing and Cost Management console

```json
aws ssm start-automation-execution --document-name "AWSSupport-GrantPermissionsToIAMUser" --parameters "IAMGroupName=BillingGroup, IAMUserName=BillingUser, Permissions=BillingFullAccess"
```

### Add IAM user SupportUser to IAM group SupportGroup and grant full access to Support Center

```json
aws ssm start-automation-execution --document-name "AWSSupport-GrantPermissionsToIAMUser" --parameters "IAMGroupName=SupportGroup, IAMUserName=SupportUser, Permissions=SupportFullAccess"
```

### Add IAM user SupportAndBillingUser to IAM group SupportAndBillingGroup and grant full access both Support Center and the AWS Billing and Cost Management console

```json
aws ssm start-automation-execution --document-name "AWSSupport-GrantPermissionsToIAMUser" --parameters "IAMGroupName=SupportAndBillingGroup, IAMUserName=SupportAndBillingUser"
```

### Retrieve the execution output

```json
aws ssm get-automation-execution --automation-execution-id 'EXECUTIONID' --query 'AutomationExecution.[AutomationExecutionStatus,Parameters,Outputs]
```

## Required IAM Permissions

Least privileges depend on how AWSSupport-GrantPermissionsToIAMUser is executed.

### Direct execution

It is recommended the user has the **AmazonSSMAutomationRole** Amazon managed policy attached, and the following additional permissions to be able to create the Lambda function and the IAM Role to pass to Lambda:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "lambda:InvokeFunction",
                "lambda:CreateFunction",
                "lambda:DeleteFunction",
                "lambda:GetFunction"
            ],
            "Resource": "arn:aws:lambda:*:ACCOUNTID:function:AWSSupport-*",
            "Effect": "Allow"
        },
        {
            "Effect" : "Allow",
            "Action" : [
                "iam:CreateGroup",
                "iam:AddUserToGroup",
                "iam:ListAttachedGroupPolicies",
                "iam:GetGroup",
                "iam:GetUser"
            ],
            "Resource" : [
                "arn:aws:iam::*:user/*",
                "arn:aws:iam::*:group/*"
            ]
        },
        {
            "Effect" : "Allow",
            "Action" : [
                "iam:AttachGroupPolicy"
            ],
            "Resource": "*",
            "Condition": {
                "ArnEquals": {
                    "iam:PolicyArn": [
                        "arn:aws:iam::aws:policy/job-function/Billing",
                        "arn:aws:iam::aws:policy/AWSSupportAccess"
                    ]
                }
            }
        },
        {
            "Effect" : "Allow",
            "Action" : [
                "iam:ListAccountAliases",
                "iam:GetAccountSummary"
            ],
            "Resource" : "*"
        }
    ]
}
```

### Using AutomationAssumeRole and LambdaAssumeRole

The user must have the **ssm:ExecuteAutomation** permissions on the document, and **iam:PassRole** on the IAM roles passed as **AutomationAssumeRole** and **LambdaAssumeRole**.
Here are the permissions each IAM role needs:

```json
AutomationAssumeRole

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "lambda:InvokeFunction",
                "lambda:CreateFunction",
                "lambda:DeleteFunction",
                "lambda:GetFunction"
            ],
            "Resource": "arn:aws:lambda:*:ACCOUNTID:function:AWSSupport-*",
            "Effect": "Allow"
        }
    ]
}
```

```json
LambdaAssumeRole

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect" : "Allow",
            "Action" : [
                "iam:CreateGroup",
                "iam:AddUserToGroup",
                "iam:ListAttachedGroupPolicies",
                "iam:GetGroup",
                "iam:GetUser"
            ],
            "Resource" : [
                "arn:aws:iam::*:user/*",
                "arn:aws:iam::*:group/*"
            ]
        },
        {
            "Effect" : "Allow",
            "Action" : [
                "iam:AttachGroupPolicy"
            ],
            "Resource": "*",
            "Condition": {
                "ArnEquals": {
                    "iam:PolicyArn": [
                        "arn:aws:iam::aws:policy/job-function/Billing",
                        "arn:aws:iam::aws:policy/AWSSupportAccess"
                    ]
                }
            }
        },
        {
            "Effect" : "Allow",
            "Action" : [
                "iam:ListAccountAliases",
                "iam:GetAccountSummary"
            ],
            "Resource" : "*"
        }
    ]
}
```

## Document Version

Document Steps:

1. aws:createStack - Execute CloudFormation Template to create lambda.
1. aws:invokeLambdaFunction - Execute Lambda to set IAM permissions.
1. aws:deleteStack - Delete CloudFormation Template.
