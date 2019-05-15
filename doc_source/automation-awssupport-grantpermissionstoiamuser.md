# AWSSupport\-GrantPermissionsToIAMUser<a name="automation-awssupport-grantpermissionstoiamuser"></a>

 **Description** 

This document grants the specified permissions to an IAM group \(new or existing\), and adds the existing IAM user to it\. Policies you can choose from: [Billing](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/job-function/Billing$serviceLevelSummary) or [Support](https://console.aws.amazon.com/iam/home?#/policies/arn:aws:iam::aws:policy/AWSSupportAccess$serviceLevelSummary)\. To enable billing access for IAM, remember to also activate [IAM user and federated user access to the Billing and Cost Management pages](http://docs.aws.amazon.com/console/iam/billing-enable)\.

**Important**  
If you provide an existing IAM group, all current IAM users in the group receive the new permissions\.

 **Document Type** 

Automation

 **Owner** 

Amazon

 **Parameters** 
+ IAMUserName

  Type: String

  Default: ExampleUser

  Description: \(Required\) Must be an existing user\.
+ IAMGroupName

  Type: String

  Default: ExampleSupportAndBillingGroup

  Description: \(Required\) Can be a new or existing group\. Must comply with [IAM Entity Name Limits](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_iam-limits.html#reference_iam-limits-names)\.
+ Permissions

  Type: String

  Allowed values: SupportFullAccess,BillingFullAccess,SupportAndBillingFullAccess

  Default: SupportAndBillingFullAccess

  Description: \(Required\) Choose one of: SupportFullAccess \- Grants full access to the Support center \| BillingFullAccess \- Grants full access to the Billing dashboard \| SupportAndBillingFullAccess \- Grants full access to both Support center and the Billing dashboard\. More info on policies under Document details\.
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role that allows Automation to perform the actions on your behalf\. If no role is specified, AWS Systems Manager Automation will use the permissions of the user that runs this document\. 
+ LambdaAssumeRole

  Type: String

  Description: \(Optional\) The ARN of the role assumed by lambda\.

 **Examples** 

Add IAM user BillingUser to IAM group BillingGroup and grant full access to the AWS Billing and Cost Management console

```
aws ssm start-automation-execution --document-name "AWSSupport-GrantPermissionsToIAMUser" --parameters "IAMGroupName=BillingGroup, IAMUserName=BillingUser, Permissions=BillingFullAccess"
```

Add IAM user SupportUser to IAM group SupportGroup and grant full access to Support Center

```
aws ssm start-automation-execution --document-name "AWSSupport-GrantPermissionsToIAMUser" --parameters "IAMGroupName=SupportGroup, IAMUserName=SupportUser, Permissions=SupportFullAccess"
```

Add IAM user SupportAndBillingUser to IAM group SupportAndBillingGroup and grant full access both Support Center and the AWS Billing and Cost Management console

```
aws ssm start-automation-execution --document-name "AWSSupport-GrantPermissionsToIAMUser" --parameters "IAMGroupName=SupportAndBillingGroup, IAMUserName=SupportAndBillingUser"
```

Retrieve the execution output

```
aws ssm get-automation-execution --automation-execution-id EXECUTIONID --output text --query 'AutomationExecution.Output'
```

 **Required IAM Permissions** 

Least privileges depend on how AWSSupport\-GrantPermissionsToIAMUser is run\. 

 **Direct execution** 

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