# Restricting access to Systems Manager parameters using IAM policies<a name="sysman-paramstore-access"></a>

You restrict access to AWS Systems Manager parameters by using AWS Identity and Access Management \(IAM\)\. More specifically, you create IAM policies that restrict access to the following API operations:
+ [DeleteParameter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeleteParameter.html)
+ [DeleteParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeleteParameters.html)
+ [DescribeParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeParameters.html)
+ [GetParameter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameter.html)
+ [GetParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html)
+ [GetParameterHistory](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameterHistory.html)
+ [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html)
+ [PutParameter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutParameter.html)

When using IAM policies to restrict access to Systems Manager parameters, we recommend that you create and use *restrictive* IAM policies\. For example, the following policy allows a user to call the `DescribeParameters` and `GetParameters` API operations for a limited set of resources\. This means that the user can get information about and use all parameters that begin with `prod-*`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:DescribeParameters"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetParameters"
            ],
            "Resource": "arn:aws:ssm:us-east-2:123456789012:parameter/prod-*"
        }
    ]
}
```

**Important**  
If a user has access to a path, then the user can access all levels of that path\. For example, if a user has permission to access path `/a`, then the user can also access `/a/b`\. Even if a user has explicitly been denied access in IAM for parameter `/a/b`, they can still call the GetParametersByPath API operation recursively for `/a` and view `/a/b`\.

For trusted administrators, you can provide access to all Systems Manager parameter API operations by using a policy similar to the following example\. This policy gives the user full access to all production parameters that begin with `dbserver-prod-*`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "ssm:PutParameter",
                "ssm:DeleteParameter",
                "ssm:GetParameterHistory",
                "ssm:GetParametersByPath",
                "ssm:GetParameters",
                "ssm:GetParameter",
                "ssm:DeleteParameters"
            ],
            "Resource": "arn:aws:ssm:region:account-id:parameter/dbserver-prod-*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "ssm:DescribeParameters",
            "Resource": "*"
        }
    ]
}
```

## Deny permissions<a name="sysman-paramstore-deny-permissions"></a>

Each API is unique and has distinct operations and permissions that you can allow or deny individually\. An explicit deny in any policy overrides the allow\.

**Note**  
The default AWS Key Management Service \(AWS KMS\) key has `Decrypt` permission for all IAM principals within the Amazon Web Services account\. If you want to have different access levels to `SecureString` parameters in your account, we don't recommend that you use the default key\.

If you want all API operations retrieving parameter values to have the same behavior, then you can use a pattern like `GetParameter*` in a policy\. The following example shows how to deny `GetParameter`, `GetParameters`, `GetParameterHistory`, and `GetParametersByPath` for all parameters beginning with `prod-*`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "ssm:GetParameter*"
            ],
            "Resource": "arn:aws:ssm:us-east-2:123456789012:parameter/prod-*"
        }
    ]
}
```

The following example shows how to deny some commands while allowing the user to perform other commands on all parameters that begin with `prod-*`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
            "Effect": "Deny",
            "Action": [
                "ssm:PutParameter",
                "ssm:DeleteParameter",
                "ssm:DeleteParameters"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetParametersByPath",
                "ssm:GetParameters",
                "ssm:GetParameter",
                "ssm:GetParameterHistory",
                "ssm:DescribeParameters"
            ],
            "Resource": "arn:aws:ssm:region:account-id:parameter/prod-*"
        }
    ]
}
```

**Note**  
The parameter history includes all parameter versions, including the current one\. Therefore, if a user is denied permission for `GetParameter`, `GetParameters`, and `GetParameterByPath` but is allowed permission for `GetParameterHistory`, they can see the current parameter, including `SecureString` parameters, using `GetParameterHistory`\.

## Allowing only specific parameters to run on instances<a name="sysman-paramstore-access-inst"></a>

You can control access so that instances can run only parameters that you specify\.

If you choose the `SecureString` parameter type when you create your parameter, Systems Manager uses AWS KMS to encrypt the parameter value\. AWS KMS encrypts the value by using either an AWS managed key or a customer managed key\. For more information about AWS KMS and AWS KMS key, see the *[AWS Key Management Service Developer Guide](https://docs.aws.amazon.com/kms/latest/developerguide/)*\.

You can view the AWS managed key by running the following command from the AWS CLI\.

```
aws kms describe-key --key-id alias/aws/ssm
```

The following example enables instances to get a parameter value only for parameters that begin with `prod-`\. If the parameter is a `SecureString` parameter, then the instance decrypts the string using AWS KMS\.

**Note**  
Instance policies, like in the following example, are assigned to the instance role in IAM\. For more information about configuring access to Systems Manager features, including how to assign policies to users and instances, see [Setting up AWS Systems Manager](systems-manager-setting-up.md)\.

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "ssm:GetParameters"
         ],
         "Resource":[
            "arn:aws:ssm:region:account-id:parameter/prod-*"
         ]
      },
      {
         "Effect":"Allow",
         "Action":[
            "kms:Decrypt"
         ],
         "Resource":[
            "arn:aws:kms:region:account-id:key/KMSkey"
         ]
      }
   ]
}
```

## IAM permissions for using AWS default keys and customer managed keys<a name="ps-kms-permissions"></a>

Parameter Store `SecureString` parameters are encrypted and decrypted using AWS KMS keys\. You can choose to encrypt your `SecureString` parameters using either a AWS KMS key or the default KMS key provided by AWS\.

When using a customer managed key, the IAM policy that grants a user access to a parameter or parameter path must provide explicit `kms:Encrypt` permissions for the key\. For example, the following policy allows a user to create, update, and view `SecureString` parameters that begin with `prod-` in the specified AWS Region and Amazon Web Services account\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:PutParameter",
                "ssm:GetParameter",
                "ssm:GetParameters"
            ],
            "Resource": [
                "arn:aws:ssm:us-east-2:111122223333:parameter/prod-*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
               "kms:Decrypt",
               "kms:Encrypt",
               "kms:GenerateDataKey"  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout01.png)
            ],
            "Resource": [
                "arn:aws:kms:us-east-2:111122223333:key/1234abcd-12ab-34cd-56ef-12345EXAMPLE"
            ]
        }
    ]
}
```

**1**The `kms:GenerateDataKey` permission is required for creating encrypted advanced parameters using the specified customer managed key\. 

By contrast, all users within the customer account have access to the default AWS managed key\. If you use this default key to encrypt `SecureString` parameters and don't want users to work with `SecureString` parameters, their IAM policies must explicitly deny access to the default key, as demonstrated in the following policy example\.

**Note**  
You can locate the Amazon Resource Name \(ARN\) of the default key in the AWS KMS console on the [AWS managed keys](https://console.aws.amazon.com/kms/home#/kms/defaultKeys) page\. The default key is the one identified with `aws/ssm` in the **Alias** column\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "kms:Decrypt",
                "kms:GenerateDataKey "
            ],
            "Resource": [
                "default-key-ARN"
            ]
        }
    ]
}
```

If you require fine\-grained access control over the `SecureString` parameters in your account, you should use a customer managed key to protect and restrict access to these parameters\. We also recommend using AWS CloudTrail to monitor `SecureString` parameter activities\.

For more information, see the following topics:
+ [Policy Evaluation Logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html) in the *IAM User Guide*
+ [Using key policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*
+ [Viewing Events with CloudTrail Event History](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html) in the *AWS CloudTrail User Guide*