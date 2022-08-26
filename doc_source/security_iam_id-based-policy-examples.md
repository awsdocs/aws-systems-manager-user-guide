# AWS Systems Manager identity\-based policy examples<a name="security_iam_id-based-policy-examples"></a>

By default, AWS Identity and Access Management \(IAM\) users and roles don't have permission to create or modify AWS Systems Manager resources\. They also can't perform tasks using the Systems Manager console, AWS Command Line Interface \(AWS CLI\), or AWS API\. An IAM administrator must create IAM policies that grant users and roles permission to perform specific API operations on the specified resources they need\. The administrator must then attach those policies to the IAM users or groups that require those permissions\.

The following is an example of a permissions policy that allows a user to delete documents with names that begin with **MyDocument\-** in the US East \(Ohio\) \(us\-east\-2\) AWS Region\.

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "ssm:DeleteDocument"
      ],
      "Resource" : [
        "arn:aws:ssm:us-east-2:111122223333:document/MyDocument-*"
      ]
    }
  ]
}
```

To learn how to create an IAM identity\-based policy using these example JSON Policy documents, see [Creating IAM policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-json-editor) in the *IAM User Guide*\.

**Topics**
+ [Policy best practices](#security_iam_service-with-iam-policy-best-practices)
+ [Using the Systems Manager console](#security_iam_id-based-policy-examples-console)
+ [Allow users to view their own permissions](#security_iam_id-based-policy-examples-view-own-permissions)
+ [Cross\-service confused deputy prevention](cross-service-confused-deputy-prevention.md)
+ [Customer managed policy examples](#customer-managed-policies)
+ [Viewing Systems Manager documents based on tags](#security_iam_id-based-policy-examples-view-documents-tags)

## Policy best practices<a name="security_iam_service-with-iam-policy-best-practices"></a>

Identity\-based policies determine whether someone can create, access, or delete Systems Manager resources in your account\. These actions can incur costs for your AWS account\. When you create or edit identity\-based policies, follow these guidelines and recommendations:
+ **Get started with AWS managed policies and move toward least\-privilege permissions** – To get started granting permissions to your users and workloads, use the *AWS managed policies* that grant permissions for many common use cases\. They are available in your AWS account\. We recommend that you reduce permissions further by defining AWS customer managed policies that are specific to your use cases\. For more information, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) or [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.
+ **Apply least\-privilege permissions** – When you set permissions with IAM policies, grant only the permissions required to perform a task\. You do this by defining the actions that can be taken on specific resources under specific conditions, also known as *least\-privilege permissions*\. For more information about using IAM to apply permissions, see [ Policies and permissions in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) in the *IAM User Guide*\.
+ **Use conditions in IAM policies to further restrict access** – You can add a condition to your policies to limit access to actions and resources\. For example, you can write a policy condition to specify that all requests must be sent using SSL\. You can also use conditions to grant access to service actions if they are used through a specific AWS service, such as AWS CloudFormation\. For more information, see [ IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.
+ **Use IAM Access Analyzer to validate your IAM policies to ensure secure and functional permissions** – IAM Access Analyzer validates new and existing policies so that the policies adhere to the IAM policy language \(JSON\) and IAM best practices\. IAM Access Analyzer provides more than 100 policy checks and actionable recommendations to help you author secure and functional policies\. For more information, see [IAM Access Analyzer policy validation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access-analyzer-policy-validation.html) in the *IAM User Guide*\.
+ **Require multi\-factor authentication \(MFA\)** – If you have a scenario that requires IAM users or root users in your account, turn on MFA for additional security\. To require MFA when API operations are called, add MFA conditions to your policies\. For more information, see [ Configuring MFA\-protected API access](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_configure-api-require.html) in the *IAM User Guide*\.

For more information about best practices in IAM, see [Security best practices in IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

## Using the Systems Manager console<a name="security_iam_id-based-policy-examples-console"></a>

To access the Systems Manager console, you must have a minimum set of permissions\. These permissions must allow you to list and view details about the Systems Manager resources and other resources in your AWS account\. 

To fully use Systems Manager in the Systems Manager console, you must have permissions from the following services:
+ AWS Systems Manager
+ Amazon Elastic Compute Cloud \(Amazon EC2\)
+ AWS Identity and Access Management \(IAM\)

You can grant the required permissions with the following policy statement\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:*",
                "ec2:describeInstances",
                "iam:ListRoles"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": "ssm.amazonaws.com"
                }
            }
        }
    ]
}
```

If you create an identity\-based policy that is more restrictive than the minimum required permissions, the console won't function as intended for entities \(IAM users or roles\) with that policy\.

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the AWS API\. Instead, allow access to only the actions that match the API operation that you're trying to perform\.

## Allow users to view their own permissions<a name="security_iam_id-based-policy-examples-view-own-permissions"></a>

This example shows how you might create a policy that allows IAM users to view the inline and managed policies that are attached to their user identity\. This policy includes permissions to complete this action on the console or programmatically using the AWS CLI or AWS API\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ViewOwnUserInfo",
            "Effect": "Allow",
            "Action": [
                "iam:GetUserPolicy",
                "iam:ListGroupsForUser",
                "iam:ListAttachedUserPolicies",
                "iam:ListUserPolicies",
                "iam:GetUser"
            ],
            "Resource": ["arn:aws:iam::*:user/${aws:username}"]
        },
        {
            "Sid": "NavigateInConsole",
            "Effect": "Allow",
            "Action": [
                "iam:GetGroupPolicy",
                "iam:GetPolicyVersion",
                "iam:GetPolicy",
                "iam:ListAttachedGroupPolicies",
                "iam:ListGroupPolicies",
                "iam:ListPolicyVersions",
                "iam:ListPolicies",
                "iam:ListUsers"
            ],
            "Resource": "*"
        }
    ]
}
```

## Customer managed policy examples<a name="customer-managed-policies"></a>

You can create standalone policies that you administer in your own AWS account\. We refer to these as *customer managed policies*\. You can attach these policies to multiple principal entities in your AWS account\. When you attach a policy to a principal entity, you give the entity the permissions that are defined in the policy\. For more information, see [Customer managed policy examples](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#customer-managed-policies) in the *[IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)*\.

The following examples of user policies grant permissions for various Systems Manager actions\. Use them to limit the Systems Manager access for your IAM users and roles\. These policies work when performing actions in the Systems Manager API, AWS SDKs, or the AWS CLI\. For users who use the console, you need to grant additional permissions specific to the console\. For more information, see [Using the Systems Manager console](#security_iam_id-based-policy-examples-console)\.

**Note**  
All examples use the US West \(Oregon\) Region \(us\-west\-2\) and contain fictitious account IDs\. The account ID shouldn't be specified in the Amazon Resource Name \(ARN\) for AWS public documents \(documents that begin with `AWS-*`\)\.

 **Examples** 
+  [Example 1: Allow a user to perform Systems Manager operations in a single Region](#identity-based-policies-example-1) 
+  [Example 2: Allow a user to list documents for a single Region](#identity-based-policies-example-2) 

### Example 1: Allow a user to perform Systems Manager operations in a single Region<a name="identity-based-policies-example-1"></a>

The following example grants permissions to perform Systems Manager operations only in the US East \(Ohio\) Region \(us\-east\-2\)\.

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "ssm:*"
      ],
      "Resource" : [
        "arn:aws:ssm:us-east-2:aws-account-ID:*"
      ]
    }
  ]
}
```

### Example 2: Allow a user to list documents for a single Region<a name="identity-based-policies-example-2"></a>

The following example grants permissions to list all document names that begin with **Update** in the US East \(Ohio\) Region \(us\-east\-2\)\.

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "ssm:ListDocuments"
      ],
      "Resource" : [
        "arn:aws:ssm:us-east-2:aws-account-ID:document/Update*"
      ]
    }
  ]
}
```

### Example 3: Allow a user to use a specific SSM document to run commands on specific nodes<a name="identity-based-policies-example-3"></a>

The following example IAM policy allows a user to do the following in the US East \(Ohio\) Region \(us\-east\-2\):
+ List Systems Manager documents \(SSM documents\) and document versions\.
+ View details about documents\.
+ Send a command using the document specified in the policy\. The name of the document is determined by the following entry\.

  ```
  arn:aws:ssm:us-east-2:aws-account-ID:document/Systems-Manager-document-name
  ```
+ Send a command to three nodes\. The nodes are determined by the following entries in the second `Resource` section\.

  ```
  "arn:aws:ssm:us-east-2:aws-account-ID:instance/i-02573cafcfEXAMPLE",
  "arn:aws:ssm:us-east-2:aws-account-ID:instance/i-0471e04240EXAMPLE",
  "arn:aws:ssm:us-east-2:aws-account-ID:instance/i-07782c72faEXAMPLE"
  ```
+ View details about a command after it has been sent\.
+ Start and stop workflows in Automation, a capability of AWS Systems Manager\.
+ Get information about Automation workflows\.

If you want to give a user permission to use this document to send commands on any node for which the user has access \(as determined by their user account\), you could specify an entry similar to the following in the `Resource` section and remove the other node entries\. The following example uses the US East \(Ohio\) Region \(us\-east\-2\)\.

```
"arn:aws:ssm:us-east-2:*:instance/*"
```

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "ssm:ListDocuments",
                "ssm:ListDocumentVersions",
                "ssm:DescribeDocument",
                "ssm:GetDocument",
                "ssm:DescribeInstanceInformation",
                "ssm:DescribeDocumentParameters",
                "ssm:DescribeInstanceProperties"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Action": "ssm:SendCommand",
            "Effect": "Allow",
            "Resource": [
                "arn:aws:ssm:us-east-2:aws-account-ID:instance/i-02573cafcfEXAMPLE",
                "arn:aws:ssm:us-east-2:aws-account-ID:instance/i-0471e04240EXAMPLE",
                "arn:aws:ssm:us-east-2:aws-account-ID:instance/i-07782c72faEXAMPLE",
                
                "arn:aws:ssm:us-east-2:aws-account-ID:document/Systems-Manager-document-name"
            ]
        },
        {
            "Action": [
                "ssm:CancelCommand",
                "ssm:ListCommands",
                "ssm:ListCommandInvocations"
            ],
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Action": "ec2:DescribeInstanceStatus",
            "Effect": "Allow",
            "Resource": "*"
        },
        {
            "Action": "ssm:StartAutomationExecution",
            "Effect": "Allow",
            "Resource": [
                "arn:aws:ssm:us-east-2:aws-account-ID:automation-definition/*"
            ]
        },
        {
            "Action": "ssm:DescribeAutomationExecutions",
            "Effect": "Allow",
            "Resource": [
                "*"
            ]
        },
        {
            "Action": [
                "ssm:StopAutomationExecution",
                "ssm:GetAutomationExecution"
            ],
            "Effect": "Allow",
            "Resource": [
                "*"
            ]
        }
    ]
}
```

## Viewing Systems Manager documents based on tags<a name="security_iam_id-based-policy-examples-view-documents-tags"></a>

You can use conditions in your identity\-based policy to control access to Systems Manager resources based on tags\. This example shows how you might create a policy that allows viewing an SSM document\. However, permission is granted only if the document tag `Owner` has the value of that user's user name\. This policy also grants the permissions necessary to complete this action on the console\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ListDocumentsInConsole",
            "Effect": "Allow",
            "Action": "ssm:ListDocuments",
            "Resource": "*"
        },
        {
            "Sid": "ViewDocumentIfOwner",
            "Effect": "Allow",
            "Action": "ssm:GetDocument",
            "Resource": "arn:aws:ssm:*:*:document/*",
            "Condition": {
                "StringEquals": {"ssm:ResourceTag/Owner": "${aws:username}"}
            }
        }
    ]
}
```

You can attach this policy to the IAM users in your account\. If a user named `richard-roe` attempts to view an Systems Manager document, the document must be tagged `Owner=richard-roe` or `owner=richard-roe`\. Otherwise they're denied access\. The condition tag key `Owner` matches both `Owner` and `owner` because condition key names aren't case\-sensitive\. For more information, see [IAM JSON policy elements: Condition](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) in the *IAM User Guide*\.