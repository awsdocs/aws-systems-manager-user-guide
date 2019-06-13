# Quickstart Default IAM Policies for Session Manager<a name="getting-started-restrict-access-quickstart"></a>

Use the following samples to help you create IAM policies that provide the most commonly needed permissions for Session Manager access\. 

**Note**  
You can also use an AWS KMS key policy to control which IAM users, IAM roles, and AWS accounts are given access to your CMK\. For information, see [Overview of Managing Access to Your AWS KMS Resources](https://docs.aws.amazon.com/kms/latest/developerguide/control-access-overview.html) and [Using Key Policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\.

**Topics**
+ [Quickstart End User Policy for Session Manager](#restrict-access-quickstart-end-user)
+ [Quickstart Administrator Policy for Session Manager](#restrict-access-quickstart-admin)

## Quickstart End User Policy for Session Manager<a name="restrict-access-quickstart-end-user"></a>

Use the following example to create an IAM end user policy for Session Manager\. It provides end users the ability start a session to a particular instance and the ability to terminate only their own sessions\. Refer to [Additional Sample IAM Policies for Session Manager](getting-started-restrict-access-examples.md) for examples of customizations you might want to make to the policy\.

Replace *instance\-id* with the ID of the instance you want to grant access to, in the format `i-02573cafcfEXAMPLE`\. Replace *region* and *account\-id* with your AWS Region and AWS Account ID\. For example, `us-east-2` and `111122223333`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:StartSession"
            ],
            "Resource": [
                "arn:aws:ec2:*:*:instance/instance-id"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:DescribeSessions",
                "ssm:GetConnectionStatus",
                "ssm:DescribeInstanceProperties",
                "ec2:DescribeInstances"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetDocument"
            ],
            "Resource": "arn:aws:ssm:region:account-id:document/SSM-SessionManagerRunShell"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:TerminateSession"
            ],
            "Resource": [
                "arn:aws:ssm:*:*:session/${aws:username}-*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "kms:GenerateDataKey"
            ],
            "Resource": "key-name"
        }
    ]
}
```

**About 'kms:GenerateDataKey**  
The `kms:GenerateDataKey` permission enables the creation of a data encryption key that will be used to encrypt session data\. If you will use AWS Key Management Service \(AWS KMS\) encryption for your session data, replace *key\-name* with the ARN of the customer master key \(CMK\) you want to use, in the format `arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-12345EXAMPLE`\. 

 If you will not use AWS KMS key encryption for your session data, remove the following content from the policy:

```
,
        {
            "Effect": "Allow",
            "Action": [
                "kms:GenerateDataKey"
            ],
            "Resource": "key-name"
        }
```

For information about AWS KMS and CMKs for encrypting session data, see [Enable AWS KMS Key Encryption of Session Data \(Console\)](session-preferences-enable-encryption.md)\.

## Quickstart Administrator Policy for Session Manager<a name="restrict-access-quickstart-admin"></a>

Use the following example to create an IAM administrator policy for Session Manager\. It provides administrators the ability to start a session to instances that are tagged with `Key=Finance,Value=WebServers`, permission to create, update and delete preferences, and permission to terminate only their own sessions\. Refer to [Additional Sample IAM Policies for Session Manager](getting-started-restrict-access-examples.md) for examples of customizations you might want to make to the policy\.

**Note**  
Update the tag/value pair `Key=Finance,Value=WebServers` with the tags applied to your instances\. Replace *region* and *account\-id* with your AWS Region and AWS Account ID\. For example, `us-east-2` and `111122223333`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:StartSession"
            ],
            "Resource": [
                "arn:aws:ec2:*:*:instance/*"
            ],
            "Condition": {
                "StringLike": {
                    "ssm:resourceTag/Finance": [
                        "WebServers"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:DescribeSessions",
                "ssm:GetConnectionStatus",
                "ssm:DescribeInstanceProperties",
                "ec2:DescribeInstances"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:CreateDocument",
                "ssm:UpdateDocument",
                "ssm:GetDocument"
            ],
            "Resource": "arn:aws:ssm:region:account-id:document/SSM-SessionManagerRunShell"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:TerminateSession"
            ],
            "Resource": [
                "arn:aws:ssm:*:*:session/${aws:username}-*"
            ]
        }
    ]
}
```