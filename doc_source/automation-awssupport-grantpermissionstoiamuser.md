# AWSSupport\-GrantPermissionsToIAMUser<a name="automation-awssupport-grantpermissionstoiamuser"></a>

 **Description** 

This document grants the specified permissions to an IAM group \(new or existing\), and adds the existing IAM user to it\. Policies you can choose from: [Billing](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/job-function/Billing$serviceLevelSummary) or [Support](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AWSSupportAccess$serviceLevelSummary)\. To enable billing access for IAM, remember to also activate [IAM user and federated user access to the Billing and Cost Management pages](https://docs.aws.amazon.com/console/iam/billing-enable)\.

**Important**  
If you provide an existing IAM group, all current IAM users in the group receive the new permissions\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-GrantPermissionsToIAMUser)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ IAMGroupName

  Type: String

  Default: ExampleSupportAndBillingGroup

  Description: \(Required\) Can be a new or existing group\. Must comply with [IAM Entity Name Limits](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_iam-limits.html#reference_iam-limits-names)\.
+ IAMUserName

  Type: String

  Default: ExampleUser

  Description: \(Required\) Must be an existing user\.
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role assumed by lambda\.
+ Permissions

  Type: String

  Valid values: SupportFullAccess \| BillingFullAccess \| SupportAndBillingFullAccess

  Default: SupportAndBillingFullAccess

  Description: \(Required\) Choose one of: `SupportFullAccess` grants full access to the Support center\. `BillingFullAccess` grants full access to the Billing dashboard\. `SupportAndBillingFullAccess` grants full access to both Support center and the Billing dashboard\. More info on policies under Document details\.

**Required IAM Permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.

The permissions required depend on how AWSSupport\-GrantPermissionsToIAMUser is run\. 

 **Running as the currently logged in user or role** 

It is recommended you have the \*\*AmazonSSMAutomationRole\*\* Amazon managed policy attached, and the following additional permissions to be able to create the Lambda function and the IAM Role to pass to Lambda: 

```
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

 **Using AutomationAssumeRole and LambdaAssumeRole** 

The user must have the **ssm:ExecuteAutomation** permissions on the document, and **iam:PassRole** on the IAM roles passed as **AutomationAssumeRole** and **LambdaAssumeRole**\. Here are the permissions each IAM role needs: 

```
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

```
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

 **Document Steps** 

1. aws:createStack \- Run AWS CloudFormation Template to create a Lambda function\.

1. aws:invokeLambdaFunction \- Run Lambda to set IAM permissions\.

1. aws:deleteStack \- Delete CloudFormation Template\.

 **Outputs** 

configureIAM\.Payload