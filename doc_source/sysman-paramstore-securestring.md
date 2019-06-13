# About Secure String Parameters<a name="sysman-paramstore-securestring"></a>

A secure string parameter is any sensitive data that needs to be stored and referenced in a secure manner\. If you have data that you don't want users to alter or reference in plain text, such as passwords or license keys, create those parameters using the `SecureString` datatype\. We recommend using secure string parameters for the following scenarios\.
+ You want to use data/parameters across AWS services without exposing the values as plain text in commands, functions, agent logs, or AWS CloudTrail logs\.
+ You want to control who has access to sensitive data\.
+ You want to be able to audit when sensitive data is accessed \(AWS CloudTrail\)\.
+ You want to encrypt your sensitive data and you want to bring your own encryption keys to manage access\.

If you choose the `SecureString` datatype when you create a parameter, then Parameter Store uses an AWS Key Management Service \(KMS\) customer master key \(CMK\) to encrypt the parameter value\. KMS uses either a customer managed CMK or an AWS\-managed CMK when encrypting the parameter value\. For more information about AWS managed and customer managed CMKs, see [AWS Key Management Service Concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html) in the *AWS Key Management Service Developer Guide*\. For more information about Parameter Store and AWS KMS encryption, see [How AWS Systems Manager Parameter Store Uses AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/services-parameter-store.html)\.

**Note**  
To view a CMK, use the AWS KMS DescribeKey operation\. This AWS CLI example uses DescribeKey to view an AWS\-managed CMK\.  

```
aws kms describe-key --key-id alias/aws/ssm
```

**Important**  
Only the *value* of a secure string parameter is encrypted\. Parameter names, descriptions, and other properties are not encrypted\.

## Create a Secure String Parameter Using a KMS Customer Master Key<a name="sysman-param-defaultkms"></a>

If you create a secure string parameter by using the AWS\-managed CMK in your account and Region, then you *don't* have to provide a value for the `--key-id` parameter\.

The following AWS CLI example shows the command to create a new secure string parameter in Parameter Store without the `--key-id` parameter: 

```
aws ssm put-parameter --name parameter_name --value "parameter value" --type SecureString
```

## Create a Secure String Parameter Using a Customer Managed CMK<a name="sysman-param-customkms"></a>

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

Use a command in the following format to create a secure string parameter using the key you just created\.

```
aws ssm put-parameter --name parameter_name --value "parameter value" --type SecureString --key-id arn:aws:kms:us-east-2:123456789012:key/1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d5e
```

**Note**  
You can manually create a parameter with an encrypted value\. In this case, because the value is already encrypted, you don’t have to choose the `SecureString` data type\. If you do choose `SecureString`, your parameter will be doubly encrypted\.

By default, all secure string values are displayed as cipher\-text\. To decrypt a secure string value, a user must have permission to call the KMS [Decrypt](https://docs.aws.amazon.com/kms/latest/APIReference/API_Decrypt.html) API action\. For information about configuring KMS access control, see [Authentication and Access Control for AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/control-access.html) in the *AWS Key Management Service Developer Guide*\.

## Using Secure String Parameters With Other AWS Services<a name="sysman-paramstore-securelam"></a>

You can also use secure string parameters with other AWS services\. In the following example, the AWS Lambda function retrieves a secure string parameter by using the [GetParameters](https://docs.aws.amazon.com/ssm/latest/APIReference/API_GetParameters.html) API\.

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
For an example of how to create and use a secure string parameter, see [Walkthrough: Create a Secure String Parameter and Join an Instance to a Domain \(PowerShell\)](sysman-param-securestring-walkthrough.md)\. For more information about using Systems Manager parameters with other AWS services, see the following blog posts\.
+ [Managing Secrets for Amazon ECS Applications Using Parameter Store and IAM Roles for Tasks](http://aws.amazon.com/blogs/compute/managing-secrets-for-amazon-ecs-applications-using-parameter-store-and-iam-roles-for-tasks/)
+ [Use Parameter Store to Securely Access Secrets and Config Data in CodeDeploy](http://aws.amazon.com/blogs/mt/use-parameter-store-to-securely-access-secrets-and-config-data-in-aws-codedeploy/)
+ [Interesting Articles on Amazon EC2 Systems Manager Parameter Store](http://aws.amazon.com/blogs/mt/interesting-articles-on-ec2-systems-manager-parameter-store/)