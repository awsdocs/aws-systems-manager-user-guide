# AWS managed policies for AWS Systems Manager<a name="security-iam-awsmanpol"></a>





To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services occasionally add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ReadOnlyAccess** AWS managed policy provides read\-only access to all AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list and descriptions of job function policies, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.









## AWS managed policy: AmazonSSMServiceRolePolicy<a name="security-iam-awsmanpol-AmazonSSMServiceRolePolicy"></a>



You can't attach `AmazonSSMServiceRolePolicy` to your IAM entities\. This policy is attached to a service\-linked role that allows Systems Manager to perform actions on your behalf\. For more information, see [Using roles to collect inventory, run maintenance window tasks, and view OpsData: AWSServiceRoleForAmazonSSM](using-service-linked-roles-service-action-1.md)\.



Currently, three Systems Manager capabilities use the service\-linked role: 
+ Inventory requires a service\-linked role\. The role enables the system to collect Inventory metadata from tags and resource groups\.
+ The Maintenance Windows capability can optionally use the service\-linked role\. The role enables the Maintenance Windows service to run maintenance tasks on target instances\. Note that the service\-linked role for Systems Manager doesn't provide the permissions needed for all scenarios\. For more information, see [Should I use a service\-linked role or a custom service role to run maintenance window tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)\.
+ The Explorer capability uses the service\-linked role to enable viewing OpsData and OpsItems from multiple accounts\.



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

\[1\] The `ssm:UpdateServiceSetting` and `ssm:GetServiceSetting` actions are allowed permissions for the following resources only:

```
arn:aws:ssm:*:*:servicesetting/ssm/opsitem/*
arn:aws:ssm:*:*:servicesetting/ssm/opsdata/*
```

\[2\] The `lambda:InvokeFunction` action is allowed permissions for the following resources only:

```
arn:aws:lambda:*:*:function:SSM*
arn:aws:lambda:*:*:function:*:SSM*
```

\[3\] The `states:` actions are allowed permissions on the following resources only:

```
arn:aws:states:*:*:stateMachine:SSM*
arn:aws:states:*:*:execution:SSM*
```

 \[4\] The `iam:PassRole` action is allowed permissions by the following condition for the Systems Manager service only:

```
"Condition": {
   "StringEquals": {
      "iam:PassedToService": [
         "ssm.amazonaws.com"
      ]
   }
}
```

\[5\] The `cloudformation:DeleteStackInstances` action is allowed permissions on the following resource only:

```
arn:aws:cloudformation:*:*:stackset/AWS-QuickSetup-SSM*:*
```

\[6\] The `cloudformation:ListStackInstances`, `cloudformation:DescribeStackSetOperation`, and `cloudformation:DeleteStackSet` actions are allowed permissions on the following resources only:

```
arn:aws:cloudformation:*:*:stackset/AWS-QuickSetup-SSM*:*
arn:aws:cloudformation:*:*:stackset-target/AWS-QuickSetup-SSM*:*
arn:aws:cloudformation:*:*:type/resource/*
```

**Full AmazonSSMServiceRolePolicy policy**



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
        }
    ]
}
```





## Systems Manager updates to AWS managed policies<a name="security-iam-awsmanpol-updates"></a>



View details about updates to AWS managed policies for Systems Manager since this service began tracking these changes\. For automatic alerts about changes to this page, subscribe to the RSS feed on the Systems Manager [Document history](systems-manager-release-history.md) page\.




| Change | Description | Date | 
| --- | --- | --- | 
|  [AmazonSSMServiceRolePolicy](#security-iam-awsmanpol-AmazonSSMServiceRolePolicy) – Update to an existing policy\.  |  Systems Manager added new permissions to allow viewing aggregate OpsData and OpsItems details from multiple accounts and Regions in AWS Systems Manager Explorer\.  | March 24, 2021 | 
|  Systems Manager started tracking changes  |  Systems Manager started tracking changes for its AWS managed policies\.  | March 12, 2021 | 