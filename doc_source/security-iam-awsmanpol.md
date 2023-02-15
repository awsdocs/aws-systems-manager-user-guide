# AWS managed policies for AWS Systems Manager<a name="security-iam-awsmanpol"></a>





To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the `ViewOnlyAccess` AWS managed policy provides read\-only access to many AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.









## AWS managed policy: AmazonSSMServiceRolePolicy<a name="security-iam-awsmanpol-AmazonSSMServiceRolePolicy"></a>

You can't attach `AmazonSSMServiceRolePolicy` to your AWS Identity and Access Management \(IAM\) entities\. This policy is attached to a service\-linked role that allows AWS Systems Manager to perform actions on your behalf\. For more information, see [Using roles to collect inventory and view OpsData: `AWSServiceRoleForAmazonSSM`](using-service-linked-roles-service-action-1.md)\.

Two Systems Manager capabilities use the service\-linked role: 
+ The Inventory capability requires a service\-linked role\. The role allows the system to collect Inventory metadata from tags and resource groups\.
+ The Explorer capability uses the service\-linked role to allow viewing OpsData and OpsItems from multiple accounts\. This service\-linked role also allows Explorer to create a managed rule when you turn on Security Hub as a data source from Explorer or OpsCenter\.
**Important**  
Previously, the Systems Manager console provided you with the ability to choose the AWS managed IAM service\-linked role `AWSServiceRoleForAmazonSSM` to use as the maintenance role for your tasks\. Using this role and its associated policy, `AmazonSSMServiceRolePolicy`, for maintenance window tasks is no longer recommended\. If you're using this role for maintenance window tasks now, we encourage you to stop using it\. Instead, create your own IAM role that enables communication between Systems Manager and other AWS services when your maintenance window tasks run\.  
For more information, see [Setting up Maintenance Windows](sysman-maintenance-permissions.md)\.

**Permissions details**

The `AWSServiceRoleForAmazonSSM` service\-linked role permissions policy allows Systems Manager to complete the following actions on all related resources \(`"Resource": "*"`\), except where indicated:
+ `ssm:CancelCommand`
+ `ssm:GetCommandInvocation`
+ `ssm:ListCommandInvocations`
+ `ssm:ListCommands`
+ `ssm:SendCommand`
+ `ssm:GetAutomationExecution`
+ `ssm:GetParameters`
+ `ssm:StartAutomationExecution`
+ `ssm:StopAutomationExecution`
+ `ssm:ListTagsForResource`
+ `ssm:GetCalendarState`
+ `ssm:UpdateServiceSetting` \[1\]
+ `ssm:GetServiceSetting` \[1\]
+ `ec2:DescribeInstanceAttribute`
+ `ec2:DescribeInstanceStatus`
+ `ec2:DescribeInstances`
+ `lambda:InvokeFunction` \[2\]
+ `states:DescribeExecution` \[3\]
+ `states:StartExecution` \[3\]
+ `resource-groups:ListGroups`
+ `resource-groups:ListGroupResources`
+ `resource-groups:GetGroupQuery`
+ `tag:GetResources`
+ `config:SelectResourceConfig`
+ `config:DescribeComplianceByConfigRule`
+ `config:DescribeComplianceByResource`
+ `config:DescribeRemediationConfigurations`
+ `config:DescribeConfigurationRecorders`
+ `cloudwatch:DescribeAlarms`
+ `compute-optimizer:GetEC2InstanceRecommendations`
+ `compute-optimizer:GetEnrollmentStatus`
+ `support:DescribeTrustedAdvisorChecks`
+ `support:DescribeTrustedAdvisorCheckSummaries`
+ `support:DescribeTrustedAdvisorCheckResult`
+ `support:DescribeCases`
+ `iam:PassRole` \[4\]
+ `cloudformation:DescribeStacks`
+ `cloudformation:ListStackResources`
+ `cloudformation:ListStackInstances` \[5\]
+ `cloudformation:DescribeStackSetOperation` \[5\]
+ `cloudformation:DeleteStackSet` \[5\]
+ `cloudformation:DeleteStackInstances` \[6\]
+ `events:PutRule` \[7\]
+ `events:PutTargets` \[7\]
+ `events:RemoveTargets` \[8\]
+ `events:DeleteRule` \[8\]
+ `events:DescribeRule`
+ `securityhub:DescribeHub`

\[1\] The `ssm:UpdateServiceSetting` and `ssm:GetServiceSetting` actions are allowed permissions for the following resources only\.

```
arn:aws:ssm:*:*:servicesetting/ssm/opsitem/*
arn:aws:ssm:*:*:servicesetting/ssm/opsdata/*
```

\[2\] The `lambda:InvokeFunction` action is allowed permissions for the following resources only\.

```
arn:aws:lambda:*:*:function:SSM*
arn:aws:lambda:*:*:function:*:SSM*
```

\[3\] The `states:` actions are allowed permissions on the following resources only\.

```
arn:aws:states:*:*:stateMachine:SSM*
arn:aws:states:*:*:execution:SSM*
```

 \[4\] The `iam:PassRole` action is allowed permissions by the following condition for the Systems Manager service only\.

```
"Condition": {
   "StringEquals": {
      "iam:PassedToService": [
         "ssm.amazonaws.com"
      ]
   }
}
```

\[5\] The `cloudformation:ListStackInstances`, `cloudformation:DescribeStackSetOperation`, and `cloudformation:DeleteStackSet` actions are allowed permissions on the following resource only\.

```
arn:aws:cloudformation:*:*:stackset/AWS-QuickSetup-SSM*:*
```

\[6\] The `cloudformation:DeleteStackInstances` action is allowed permissions on the following resources only\.

```
arn:aws:cloudformation:*:*:stackset/AWS-QuickSetup-SSM*:*
arn:aws:cloudformation:*:*:stackset-target/AWS-QuickSetup-SSM*:*
arn:aws:cloudformation:*:*:type/resource/*
```

\[7\] The `events:PutRule` and `events:PutTargets` actions are allowed permissions by the following condition for the Systems Manager service only\.

```
"Condition": {
    "StringEquals": {
        "events:ManagedBy": "ssm.amazonaws.com"
    }
}
```

\[8\] The `events:RemoveTargets` and `events:DeleteRule` actions are allowed permissions on the following resource only\.

```
arn:aws:events:*:*:rule/SSMExplorerManagedRule
```

**Full `AmazonSSMServiceRolePolicy` policy**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:CancelCommand",
                "ssm:GetCommandInvocation",
                "ssm:ListCommandInvocations",
                "ssm:ListCommands",
                "ssm:SendCommand",
                "ssm:GetAutomationExecution",
                "ssm:GetParameters",
                "ssm:StartAutomationExecution",
                "ssm:StopAutomationExecution",
                "ssm:ListTagsForResource",
                "ssm:GetCalendarState"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:UpdateServiceSetting",
                "ssm:GetServiceSetting"
            ],
            "Resource": [
                "arn:aws:ssm:*:*:servicesetting/ssm/opsitem/*",
                "arn:aws:ssm:*:*:servicesetting/ssm/opsdata/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstanceAttribute",
                "ec2:DescribeInstanceStatus",
                "ec2:DescribeInstances"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "lambda:InvokeFunction"
            ],
            "Resource": [
                "arn:aws:lambda:*:*:function:SSM*",
                "arn:aws:lambda:*:*:function:*:SSM*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "states:DescribeExecution",
                "states:StartExecution"
            ],
            "Resource": [
                "arn:aws:states:*:*:stateMachine:SSM*",
                "arn:aws:states:*:*:execution:SSM*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "resource-groups:ListGroups",
                "resource-groups:ListGroupResources",
                "resource-groups:GetGroupQuery"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudformation:DescribeStacks",
                "cloudformation:ListStackResources"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "tag:GetResources"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "config:SelectResourceConfig"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "compute-optimizer:GetEC2InstanceRecommendations",
                "compute-optimizer:GetEnrollmentStatus"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "support:DescribeTrustedAdvisorChecks",
                "support:DescribeTrustedAdvisorCheckSummaries",
                "support:DescribeTrustedAdvisorCheckResult",
                "support:DescribeCases"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "config:DescribeComplianceByConfigRule",
                "config:DescribeComplianceByResource",
                "config:DescribeRemediationConfigurations",
                "config:DescribeConfigurationRecorders"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": "cloudwatch:DescribeAlarms",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": [
                        "ssm.amazonaws.com"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": "organizations:DescribeOrganization",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "cloudformation:ListStackSets",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "cloudformation:ListStackInstances",
                "cloudformation:DescribeStackSetOperation",
                "cloudformation:DeleteStackSet"
            ],
            "Resource": "arn:aws:cloudformation:*:*:stackset/AWS-QuickSetup-SSM*:*"
        },
        {
            "Effect": "Allow",
            "Action": "cloudformation:DeleteStackInstances",
            "Resource": [
                "arn:aws:cloudformation:*:*:stackset/AWS-QuickSetup-SSM*:*",
                "arn:aws:cloudformation:*:*:stackset-target/AWS-QuickSetup-SSM*:*",
                "arn:aws:cloudformation:*:*:type/resource/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "events:PutRule",
                "events:PutTargets"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "events:ManagedBy": "ssm.amazonaws.com"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "events:RemoveTargets",
                "events:DeleteRule"
            ],
            "Resource": [
                "arn:aws:events:*:*:rule/SSMExplorerManagedRule"
            ]
        },
        {
            "Effect": "Allow",
            "Action": "events:DescribeRule",
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "securityhub:DescribeHub",
            "Resource": "*"
        }
    ]
}
```

## `AmazonSSMReadOnlyAccess`<a name="security-iam-awsmanpol-AmazonSSMReadOnlyAccess"></a>

Use the `AmazonSSMReadOnlyAccess` AWS managed policy to allow read\-only access to AWS Systems Manager\. Assigning this policy to an IAM entity grants permission to read\-only API operations, such as `Describe*`, `Get*`, and `List*`\. View the [policy](https://console.aws.amazon.com/iam/home#policies/arn:aws:iam::aws:policy/AmazonSSMReadOnlyAccess) for the full list of actions supported by this policy\. 

### Service\-level permissions<a name="AmazonSSMReadOnlyAccess-service-level-permissions"></a>

This policy provides read\-only access to Systems Manager\. No other service permissions are included in this policy\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:Describe*",
                "ssm:Get*",
                "ssm:List*"
            ],
            "Resource": "*"
        }
    ]
}
```

## AWS managed policy: `AWSSystemsManagerOpsDataSyncServiceRolePolicy`<a name="security-iam-awsmanpol-AWSSystemsManagerOpsDataSyncServiceRolePolicy"></a>

You can't attach `AWSSystemsManagerOpsDataSyncServiceRolePolicy` to your IAM entities\. This policy is attached to a service\-linked role that allows Systems Manager to perform actions on your behalf\. For more information, see [Using roles to create OpsData and OpsItems for Explorer: `AWSServiceRoleForSystemsManagerOpsDataSync`](using-service-linked-roles-service-action-3.md)\.

 `AWSSystemsManagerOpsDataSyncServiceRolePolicy` allows the [``](using-service-linked-roles-service-action-3.md) service\-linked role to create and update OpsItems and OpsData from AWS Security Hub findings\. 

**Permissions details**

The `AWSServiceRoleForSystemsManagerOpsDataSync` service\-linked role permissions policy allows Systems Manager to complete the following actions on all related resources \(`"Resource": "*"`\), except where indicated:
+ `ssm:GetOpsItem` \[1\]
+ `ssm:UpdateOpsItem` \[1\]
+ `ssm:CreateOpsItem`
+ `ssm:AddTagsToResource` \[2\]
+ `ssm:UpdateServiceSetting` \[3\]
+ `ssm:GetServiceSetting` \[3\]
+ `securityhub:GetFindings`
+ `securityhub:GetFindings`
+ `securityhub:BatchUpdateFindings` \[4\]

\[1\] The `ssm:GetOpsItem` and `ssm:UpdateOpsItem` actions are allowed permissions by the following condition for the Systems Manager service only\.

```
"Condition": {
    "StringEquals": {
        "aws:ResourceTag/ExplorerSecurityHubOpsItem": "true"
    }
}
```

\[2\] The `ssm:AddTagsToResource` action is allowed permissions for the following resource only\.

```
arn:aws:ssm:*:*:opsitem/*
```

\[3\] The `ssm:UpdateServiceSetting` and `ssm:GetServiceSetting` actions are allowed permissions for the following resources only\.

```
arn:aws:ssm:*:*:servicesetting/ssm/opsitem/*
arn:aws:ssm:*:*:servicesetting/ssm/opsdata/*
```

\[4\] The `securityhub:BatchUpdateFindings` are denied permissions by the following condition for the Systems Manager service only\.

```
"Condition": {
    "StringEquals": {
        "securityhub:ASFFSyntaxPath/Workflow.Status": "SUPPRESSED"
    },
    "Null": {
        "securityhub:ASFFSyntaxPath/Confidence": false,
        "securityhub:ASFFSyntaxPath/Criticality": false,
        "securityhub:ASFFSyntaxPath/Note": false,
        "securityhub:ASFFSyntaxPath/RelatedFindings": false,
        "securityhub:ASFFSyntaxPath/Types": false,
        "securityhub:ASFFSyntaxPath/UserDefinedFields": false,
        "securityhub:ASFFSyntaxPath/VerificationState": false
    }
}
```

**Full `AWSSystemsManagerOpsDataSyncServiceRolePolicy` policy**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetOpsItem",
                "ssm:UpdateOpsItem"
            ],
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:ResourceTag/ExplorerSecurityHubOpsItem": "true"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:CreateOpsItem"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:AddTagsToResource"
            ],
            "Resource": "arn:aws:ssm:*:*:opsitem/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:UpdateServiceSetting",
                "ssm:GetServiceSetting"
            ],
            "Resource": [
                "arn:aws:ssm:*:*:servicesetting/ssm/opsitem/*",
                "arn:aws:ssm:*:*:servicesetting/ssm/opsdata/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "securityhub:GetFindings",
                "securityhub:BatchUpdateFindings"
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Effect": "Deny",
            "Action": "securityhub:BatchUpdateFindings",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "securityhub:ASFFSyntaxPath/Workflow.Status": "SUPPRESSED"
                },
                "Null": {
                    "securityhub:ASFFSyntaxPath/Confidence": false,
                    "securityhub:ASFFSyntaxPath/Criticality": false,
                    "securityhub:ASFFSyntaxPath/Note": false,
                    "securityhub:ASFFSyntaxPath/RelatedFindings": false,
                    "securityhub:ASFFSyntaxPath/Types": false,
                    "securityhub:ASFFSyntaxPath/UserDefinedFields": false,
                    "securityhub:ASFFSyntaxPath/VerificationState": false
                }
            }
        }
    ]
}
```

## AWS managed policy: `AmazonSSMManagedEC2InstanceDefaultPolicy`<a name="security-iam-awsmanpol-AmazonSSMManagedEC2InstanceDefaultPolicy"></a>

You shouldn't attach `AmazonSSMManagedEC2InstanceDefaultPolicy` to your IAM entities\. This policy should be attached to a role that grants permissions to your Amazon EC2 instances to allow Systems Manager functionality\. For more information, see [Default Host Management Configuration](managed-instances-default-host-management.md)\.

This policy grants permissions that allow the SSM Agent on your Amazon EC2 instance to retrieve Documents, execute commands using Run Command, establish sessions using Session Manager, collect an inventory of the instance, and scan for patches and patch compliance using Patch Manager\.

Systems Manager uses a personalized authorization token for each instance to ensure that the SSM agent performs the API operations on the correct instance\. Systems Manager validates the personalized authorization token against the ARN of the instance, provided in the API operation\.

**Permissions details**

The `AmazonSSMManagedEC2InstanceDefaultPolicy` role permissions policy allows Systems Manager to complete the following actions on all related resources:
+ `ssm:DescribeAssociation`
+ `ssm:GetDeployablePatchSnapshotForInstance`
+ `ssm:GetDocument`
+ `ssm:DescribeDocument`
+ `ssm:GetManifest`
+ `ssm:ListAssociations`
+ `ssm:ListInstanceAssociations`
+ `ssm:PutInventory`
+ `ssm:PutComplianceItems`
+ `ssm:PutConfigurePackageResult`
+ `ssm:UpdateAssociationStatus`
+ `ssm:UpdateInstanceAssociationStatus`
+ `ssm:UpdateInstanceInformation`
+ `ssmmessages:CreateControlChannel`
+ `ssmmessages:CreateDataChannel`
+ `ssmmessages:OpenControlChannel`
+ `ssmmessages:OpenDataChannel`
+ `ec2messages:AcknowledgeMessage`
+ `ec2messages:DeleteMessage`
+ `ec2messages:FailMessage`
+ `ec2messages:GetEndpoint`
+ `ec2messages:GetMessages`
+ `ec2messages:SendReply`

The following API operations do not allow the SSM Agent to make calls for other instances:
+ `ec2messages:GetMessages`
+ `ec2messages:SendReply`
+ `ssm:PutComplianceItems`
+ `ssm:UpdateAssociationStatus`
+ `ssm:UpdateInstanceAssociationStatus`
+ `ssmmessages:CreateControlChannel`

**Full `AmazonSSMManagedEC2InstanceDefaultPolicy` policy**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:DescribeAssociation",
                "ssm:GetDeployablePatchSnapshotForInstance",
                "ssm:GetDocument",
                "ssm:DescribeDocument",
                "ssm:GetManifest",
                "ssm:ListAssociations",
                "ssm:ListInstanceAssociations",
                "ssm:PutInventory",
                "ssm:PutComplianceItems",
                "ssm:PutConfigurePackageResult",
                "ssm:UpdateAssociationStatus",
                "ssm:UpdateInstanceAssociationStatus",
                "ssm:UpdateInstanceInformation"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssmmessages:CreateControlChannel",
                "ssmmessages:CreateDataChannel",
                "ssmmessages:OpenControlChannel",
                "ssmmessages:OpenDataChannel"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ec2messages:AcknowledgeMessage",
                "ec2messages:DeleteMessage",
                "ec2messages:FailMessage",
                "ec2messages:GetEndpoint",
                "ec2messages:GetMessages",
                "ec2messages:SendReply"
            ],
            "Resource": "*"
        }
    ]
}
```





## Systems Manager updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>



View details about updates to AWS managed policies for Systems Manager since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the Systems Manager [Document history](systems-manager-release-history.md) page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  [`AmazonSSMManagedEC2InstanceDefaultPolicy`](#security-iam-awsmanpol-AmazonSSMManagedEC2InstanceDefaultPolicy) – New policy  |  Systems Manager added a new policy to allow Systems Manager functionality on Amazon EC2 instances without the use of an IAM instance profile\.  | August 18, 2022 | 
|  [AmazonSSMServiceRolePolicy](#security-iam-awsmanpol-AmazonSSMServiceRolePolicy) – Update to an existing policy\.  |  Systems Manager added new permissions to allow Explorer to create a managed rule when you turn on Security Hub from Explorer or OpsCenter\. New permissions were added to check that config and the compute\-optimizer meet the necessary requirements before allowing OpsData\.  | April 27, 2021 | 
|  [`AWSSystemsManagerOpsDataSyncServiceRolePolicy`](#security-iam-awsmanpol-AWSSystemsManagerOpsDataSyncServiceRolePolicy) – New policy\.  |  Systems Manager added a new policy to create and update OpsItems and OpsData from Security Hub findings in Explorer and OpsCenter\.  | April 27, 2021 | 
|  `AmazonSSMServiceRolePolicy` – Update to an existing policy\.  |  Systems Manager added new permissions to allow viewing aggregate OpsData and OpsItems details from multiple accounts and AWS Regions in Explorer\.  | March 24, 2021 | 
|  Systems Manager started tracking changes  |  Systems Manager started tracking changes for its AWS managed policies\.  | March 12, 2021 | 