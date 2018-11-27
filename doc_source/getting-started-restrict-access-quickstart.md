# Quickstart Default IAM Policies for Session Manager<a name="getting-started-restrict-access-quickstart"></a>

Use the following samples to help you create IAM policies that provide the most commonly needed permissions for Session Manager access\. 

**Topics**
+ [Quickstart End User Policy for Session Manager](#restrict-access-quickstart-end-user)
+ [Quickstart Administrator Policy for Session Manager](#restrict-access-quickstart-admin)

## Quickstart End User Policy for Session Manager<a name="restrict-access-quickstart-end-user"></a>

Use the following example to create an IAM end user policy for Session Manager\. It provides end users the ability start a session to a particular instance and the ability to terminate only their own sessions\. Refer to [Additional Sample IAM Policies for Session Manager](getting-started-restrict-access-examples.md) for examples of customizations you might want to make to the policy\.

**Note**  
Replace *instance\-id* with the ID of the instance you want to grant access to, in the format `i-1234567890EXAMPLE`\. Replace *region* and *account\-id* with your AWS Region and AWS Account ID\. For example, `us-east-2` and `111122223333`\.

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
        }
    ]
}
```

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