# SecureString parameters<a name="sysman-paramstore-securestring"></a>

A `SecureString` parameter is any sensitive data that needs to be stored and referenced in a secure manner\. If you have data that you don't want users to alter or reference in plain text, such as passwords or license keys, create those parameters using the `SecureString` datatype\.

**Important**  
Do not store sensitive data in a `String` or `StringList` parameter\. For all sensitive data that must remain encrypted, use only the `SecureString` parameter type\.  
For more information, see [Parameter types and examples](parameter-store-about-examples.md)\.

We recommend using `SecureString` parameters for the following scenarios\.
+ You want to use data/parameters across AWS services without exposing the values as plain text in commands, functions, agent logs, or AWS CloudTrail logs\.
+ You want to control who has access to sensitive data\.
+ You want to be able to audit when sensitive data is accessed \(AWS CloudTrail\)\.
+ You want to encrypt your sensitive data and you want to bring your own encryption keys to manage access\.

**Important**  
Only the *value* of a `SecureString` parameter is encrypted\. Parameter names, descriptions, and other properties are not encrypted\.

**AWS KMS encryption and pricing**  
If you choose the `SecureString` type when you create a parameter, then Parameter Store uses an AWS Key Management Service \(KMS\) customer master key \(CMK\) to encrypt the parameter value\. KMS uses either a customer managed CMK or an AWS\-managed CMK when encrypting the parameter value\. 

There is no charge from Parameter Store to create a `SecureString` parameter, but charges for use of AWS Key Management Service encryption do apply\. For information, see [AWS Key Management Service pricing](https://aws.amazon.com/kms/pricing)\.

For more information about AWS managed and customer managed CMKs, see [AWS Key Management Service Concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html) in the *AWS Key Management Service Developer Guide*\. For more information about Parameter Store and AWS KMS encryption, see [How AWS Systems Manager Parameter Store Uses AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/services-parameter-store.html)\.

**Note**  
To view a CMK, use the AWS KMS DescribeKey operation\. This AWS CLI example uses DescribeKey to view an AWS\-managed CMK\.  

```
aws kms describe-key --key-id alias/aws/ssm
```

## IAM permissions for using AWS default keys and customer managed keys<a name="ps-kms-key-permissions"></a>

Parameter Store `SecureString` parameters are encrypted and decrypted using AWS Key Management Service \(AWS KMS\) keys\. You can choose to encrypt your `SecureString` parameters using either a customer master key \(CMK\) or the default KMS key provided by AWS\.

When using a customer managed key, the IAM policy that grants a user access to a parameter or parameter path must provide explicit `kms:Encrypt` permissions for the key\. For example, the following policy allows a user to create, update, and view `SecureString` parameters that begin with "prod\-" in the specified Region and account\.

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

By contrast, all users within the customer account have access to the default AWS managed key\. If you use this default key to encrypt `SecureString` parameters and do not want users to work with `SecureString` parameters, their IAM policies must explicitly deny access to the default key, as demonstrated in the following policy example\.

**Note**  
You can locate the ARN of the default key in the AWS KMS console on the [AWS managed keys](https://console.aws.amazon.com/kms/home#/kms/defaultKeys) page\. The default key is the one identified with `aws/ssm` in the **Alias** column\.

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

If you require fine\-grained access control over the `SecureString` parameters in your account, you should use a customer managed CMK to protect and restrict access to these parameters\. We also recommend using AWS CloudTrail to monitor `SecureString` parameter activities\.

For more information, see the following topics\.
+ [Policy Evaluation Logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html) in the *IAM User Guide*
+ [Using key policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*
+ [Viewing Events with CloudTrail Event History](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html) in the *AWS CloudTrail User Guide*

## Create a SecureString parameter using the default AWS managed key<a name="sysman-param-defaultkms"></a>

If you create a `SecureString` parameter by using the AWS\-managed AWS KMS key in your account and Region, then you *don't* have to provide a value for the `--key-id` parameter\.

The following AWS CLI example shows the command to create a new `SecureString` parameter in Parameter Store without the `--key-id` parameter: 

------
#### [ Linux ]

```
aws ssm put-parameter \
    --name parameter-name \
    --value "parameter-value" \
    --type SecureString
```

------
#### [ Windows ]

```
aws ssm put-parameter ^
    --name parameter-name ^
    --value "parameter-value" ^
    --type SecureString
```

------

## Create a SecureString parameter using a customer managed CMK<a name="sysman-param-customkms"></a>

To use a customer managed CMK instead of the AWS\-managed CMK assigned to your account, you must specify the key by using the `--key-id` parameter\. The parameter supports the following KMS parameter formats\.
+ Key ARN example:

   `arn:aws:kms:us-east-2:123456789012:key/12345678-1234-1234-1234-123456789012`
+ Alias ARN example:

  `arn:aws:kms:us-east-2:123456789012:alias/MyAliasName`
+ Key ID example:

  `12345678-1234-1234-1234-123456789012`
+ Alias Name example:

  `alias/MyAliasName`

You can create a customer managed CMK by using the AWS Management Console or the AWS KMS API\. The following AWS CLI commands create a customer managed key in the current Region of your AWS account\.

```
aws kms [create\-key](https://docs.aws.amazon.com/kms/latest/APIReference/API_CreateKey.html)
```

Use a command in the following format to create a `SecureString` parameter using the key you just created\.

------
#### [ Linux ]

```
aws ssm put-parameter \
    --name parameter-name \
    --value "parameter-value" \
    --type SecureString \
    --key-id arn:aws:kms:us-east-2:123456789012:key/1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d5e
```

------
#### [ Windows ]

```
aws ssm put-parameter ^
    --name parameter-name ^
    --value "parameter-value" ^
    --type SecureString ^
    --key-id arn:aws:kms:us-east-2:123456789012:key/1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d5e
```

------

**Note**  
You can manually create a parameter with an encrypted value\. In this case, because the value is already encrypted, you don’t have to choose the `SecureString` parameter type\. If you do choose `SecureString`, your parameter will be doubly encrypted\.

By default, all `SecureString` values are displayed as cipher\-text\. To decrypt a `SecureString` value, a user must have permission to call the KMS [Decrypt](https://docs.aws.amazon.com/kms/latest/APIReference/API_Decrypt.html) API action\. For information about configuring KMS access control, see [Authentication and Access Control for AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/control-access.html) in the *AWS Key Management Service Developer Guide*\.

## Using SecureString parameters with other AWS services<a name="sysman-paramstore-securelam"></a>

You can also use `SecureString` parameters with other AWS services\. In the following example, the AWS Lambda function retrieves a `SecureString` parameter by using the [GetParameters](https://docs.aws.amazon.com/ssm/latest/APIReference/API_GetParameters.html) API\.

```
from __future__ import print_function
 
import json
import boto3
ssm = boto3.client('ssm', 'us-east-2')
 def get_parameters():
    response = ssm.get_parameters(
        Names=['LambdaSecureString'],WithDecryption=True
    )
    for parameter in response['Parameters']:
        return parameter['Value']
        
def lambda_handler(event, context):
    value = get_parameters()
    print("value1 = " + value)
    return value  # Echo back the first key value
```

**Related topics**  
For an example of how to create and use a `SecureString` parameter, see [Walkthrough: Create a SecureString parameter and join an instance to a Domain \(PowerShell\)](sysman-param-securestring-walkthrough.md)\. For more information about using Systems Manager parameters with other AWS services, see the following blog posts\.
+ [Managing Secrets for Amazon ECS Applications Using Parameter Store and IAM Roles for Tasks](http://aws.amazon.com/blogs/compute/managing-secrets-for-amazon-ecs-applications-using-parameter-store-and-iam-roles-for-tasks/)
+ [Use Parameter Store to Securely Access Secrets and Config Data in CodeDeploy](http://aws.amazon.com/blogs/mt/use-parameter-store-to-securely-access-secrets-and-config-data-in-aws-codedeploy/)
+ [Interesting Articles on Amazon EC2 Systems Manager Parameter Store](http://aws.amazon.com/blogs/mt/interesting-articles-on-ec2-systems-manager-parameter-store/)