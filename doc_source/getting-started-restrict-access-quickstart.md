# Quickstart default IAM policies for Session Manager<a name="getting-started-restrict-access-quickstart"></a>

Use the samples in this section to help you create IAM policies that provide the most commonly needed permissions for Session Manager access\. 

**Note**  
You can also use an AWS KMS key policy to control which IAM users, IAM roles, and AWS accounts are given access to your CMK\. For information, see [Overview of Managing Access to Your AWS KMS Resources](https://docs.aws.amazon.com/kms/latest/developerguide/control-access-overview.html) and [Using Key Policies in AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html) in the *AWS Key Management Service Developer Guide*\.

**Topics**
+ [Quickstart end user policies for Session Manager](#restrict-access-quickstart-end-user)
+ [Quickstart administrator policy for Session Manager](#restrict-access-quickstart-admin)

## Quickstart end user policies for Session Manager<a name="restrict-access-quickstart-end-user"></a>

Use the following examples to create IAM end user policies for Session Manager\. 

You can create a policy that allows users to start sessions from only the Session Manager console and AWS CLI, from only the Amazon EC2 console, or from all three\.

These policies provide end users the ability start a session to a particular instance and the ability to end only their own sessions\. Refer to [Additional sample IAM policies for Session Manager](getting-started-restrict-access-examples.md) for examples of customizations you might want to make to the policy\.

**Note**  
In all the following sample policies, replace *instance\-id* with the ID of the instance you want to grant access to, in the format `i-02573cafcfEXAMPLE`\. Replace *region* and *account\-id* with your AWS Region and AWS Account ID, such as `us-east-2` and `111122223333`\. 

Choose from the following tabs to view the sample policy for the range of session access you want to provide\.

------
#### [ Session Manager and CLI ]

Use this sample policy to provider users with the ability to start sessions from only the Session Manager console and the AWS CLI\. This policy doesn't provide all the permissions needed to start sessions from the Amazon EC2 console\.

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
                "arn:aws:ec2:*:*:instance/instance-id",
                "arn:aws:ssm:region:account-id:document/SSM-SessionManagerRunShell" ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout01.png)
            ],
            "Condition": {
                "BoolIfExists": {
                    "ssm:SessionDocumentAccessCheck": "true" ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout02.png)
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
                "ssm:TerminateSession"
            ],
            "Resource": [
                "arn:aws:ssm:*:*:session/${aws:username}-*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "kms:GenerateDataKey" ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout03.png)
            ],
            "Resource": "key-name"
        }
    ]
}
```

------
#### [ Amazon EC2 ]

Use this sample policy to provider users with the ability to start sessions from only the Amazon EC2 console\. This policy doesn't provide all the permissions needed to start sessions from the Session Manager console and the AWS CLI\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:StartSession",
                "ssm:SendCommand" ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout04.png)
            ],
            "Resource": [
                "arn:aws:ec2:*:*:instance/instance-id"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetConnectionStatus",
                "ssm:DescribeInstanceInformation"
            ],
            "Resource": "*"
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

------
#### [ Session Manager, CLI, and Amazon EC2 ]

Use this sample policy to provider users with the ability to start sessions from the Session Manager console, the AWS CLI, and the Amazon EC2 console\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:StartSession",
                "ssm:SendCommand" ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout04.png)
            ],
            "Resource": [
                "arn:aws:ec2:*:*:instance/instance-id",
                "arn:aws:ssm:region:account-id:document/SSM-SessionManagerRunShell" ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout01.png)
            ],
            "Condition": {
                "BoolIfExists": {
                    "ssm:SessionDocumentAccessCheck": "true" ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout02.png)
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:DescribeSessions",
                "ssm:GetConnectionStatus",
                "ssm:DescribeInstanceInformation",
                "ssm:DescribeInstanceProperties",
                "ec2:DescribeInstances"
            ],
            "Resource": "*"
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
                "kms:GenerateDataKey" ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout03.png)
            ],
            "Resource": "key-name"
        }
    ]
}
```

------

**1** `SSM-SessionManagerRunShell` is the default name of the SSM document that Session Manager creates to store your session configuration preferences\. You can create a custom session document and specify it in this policy instead\. You can also specify the AWS\-provided document, `AWS-StartSSHSession`, for users who are starting sessions using SSH\. For information about configuration steps needed to support sessions using SSH, see [\(Optional\) Enable SSH connections through Session Manager](session-manager-getting-started-enable-ssh-connections.md)\.

**2** If you specify the condition element, `ssm:SessionDocumentAccessCheck`, as `true`, the system checks that a user has explicit access to the defined session document, in this example `SSM-SessionManagerRunShell`, before a session is established\. For more information, see [Enforce a session document permission check for the AWS CLI](getting-started-sessiondocumentaccesscheck.md)\.

**3** The `kms:GenerateDataKey` permission enables the creation of a data encryption key that will be used to encrypt session data\. If you will use AWS Key Management Service \(AWS KMS\) encryption for your session data, replace *key\-name* with the ARN of the customer master key \(CMK\) you want to use, in the format `arn:aws:kms:us-west-2:111122223333:key/1234abcd-12ab-34cd-56ef-12345EXAMPLE`\. If you won't use AWS KMS key encryption for your session data, remove the following content from the policy:

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

For information about AWS KMS and CMKs for encrypting session data, see [Enable AWS KMS key encryption of session data \(console\)](session-preferences-enable-encryption.md)\.

**4** The permission for [SendCommand](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendCommand.html) is needed for cases where a user attempts to start a session from the Amazon EC2 console, but a command must be sent to update SSM Agent first\.

## Quickstart administrator policy for Session Manager<a name="restrict-access-quickstart-admin"></a>

Use the following examples to create IAM administrator policies for Session Manager\. 

These policies provide administrators the ability to start a session to instances that are tagged with `Key=Finance,Value=WebServers`, permission to create, update and delete preferences, and permission to end only their own sessions\. Refer to [Additional sample IAM policies for Session Manager](getting-started-restrict-access-examples.md) for examples of customizations you might want to make to the policy\.

You can create a policy that allows administrators to perform these tasks from only the Session Manager console and AWS CLI, from only the Amazon EC2 console, or from all three\.

**Note**  
Replace *region* and *account\-id* with your AWS Region and AWS Account ID, such as `us-east-2` and `111122223333`\.

Choose from the following tabs to view the sample policy for the access scenario you want to support\.

------
#### [ Session Manager and CLI ]

Use this sample policy to provider administrators with the ability to perform session\-related tasks from only the Session Manager console and the AWS CLI\. This policy doesn't provide all the permissions needed to perform session\-related tasks from the Amazon EC2 console\.

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

------
#### [ Amazon EC2 ]

Use this sample policy to provider administrators with the ability to perform session\-related tasks from only the Amazon EC2\. This policy doesn't provide all the permissions needed to perform session\-related tasks from the Session Manager console and the AWS CLI\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:StartSession",
                "ssm:SendCommand" ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout01.png)
            ],
            "Resource": [
                "arn:aws:ec2:*:*:instance/*"
            ],
            "Condition": {
                "StringLike": {
                    "ssm:resourceTag/tag-key": [
                        "tag-value"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetConnectionStatus",
                "ssm:DescribeInstanceInformation"
            ],
            "Resource": "*"
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

------
#### [ Session Manager, CLI, and Amazon EC2 ]

Use this sample policy to provider administrators with the ability to perform session\-related tasks from the Session Manager console, the AWS CLI, and the Amazon EC2 console\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:StartSession",
                "ssm:SendCommand" ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout01.png)
            ],
            "Resource": [
                "arn:aws:ec2:*:*:instance/*"
            ],
            "Condition": {
                "StringLike": {
                    "ssm:resourceTag/tag-key": [
                        "tag-value"
                    ]
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:DescribeSessions",
                "ssm:GetConnectionStatus",
                "ssm:DescribeInstanceInformation",
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

------

**1** The permission for [SendCommand](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendCommand.html) is needed for cases where a user attempts to start a session from the Amazon EC2 console, but a command must be sent to update SSM Agent first\.