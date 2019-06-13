# Additional Sample IAM Policies for Session Manager<a name="getting-started-restrict-access-examples"></a>

Refer to the following example policies to help you create a custom IAM policy for any Session Manager user access scenarios you want to support\.

**Topics**
+ [Example 1: Restrict Access to Specific Instances](#restrict-access-example-instances)
+ [Example 2: Restrict Access Based on Instance Tags](#restrict-access-example-instance-tags)
+ [Example 3: Allow a User to Terminate Only Sessions They Started](#restrict-access-example-user-sessions)
+ [Example 4: Allow Full \(Administrative\) Access to All Sessions](#restrict-access-example-full-access)

## Example 1: Restrict Access to Specific Instances<a name="restrict-access-example-instances"></a>

You can restrict access to specific instances by creating an IAM user policy that includes the IDs of the instances\. In the following example, the user is allowed Session Manager access to three specific instances only, and allowed to terminate only their sessions on those instances\. If the user sends a command to any other instance or attempts to terminate any other session, the command result will include `AccessDenied`\.

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
                "arn:aws:ec2:us-east-2:123456789012:instance/i-1234567890EXAMPLE",
                "arn:aws:ec2:us-east-2:123456789012:instance/i-abcdefghijEXAMPLE",
                "arn:aws:ec2:us-east-2:123456789012:instance/i-0e9d8c7b6aEXAMPLE"
            ]
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

## Example 2: Restrict Access Based on Instance Tags<a name="restrict-access-example-instance-tags"></a>

You can restrict access to instances based on specific Amazon EC2 tags\. In the following example, the user is allowed to start sessions \(`Effect: Allow, Action: ssm:StartSession`\) on any instance \(`Resource: arn:aws:ec2:*:*:instance/*`\) with the condition that the instance is a Finance WebServer \(`ssm:resourceTag/Finance: WebServer`\)\. If the user sends a command to an instance that is not tagged or that has any tag other than `Finance: WebServer`, the command result will include `AccessDenied`\.

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
                "ssm:TerminateSession"
            ],
            "Resource": [
                "arn:aws:ssm:*:*:session/${aws:username}-*"
            ]
        }
    ]
}
```

You can create IAM policies that enable a user to start sessions to instances that are tagged with multiple tags\. The following policy enables the user to start sessions to instances that have of both the specified tags applied to them\. If a user sends a command to an instance that is not tagged with both of these tags, the command result will include `AccessDenied`\.

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "ssm:StartSession"
         ],
         "Resource":"*",
         "Condition":{
            "StringLike":{
               "ssm:resourceTag/tag_key1":[
                  "tag_value1"
               ],
               "ssm:resourceTag/tag_key2":[
                  "tag_value2"
               ]
            }
         }
      }
   ]
}
```

For more information about creating IAM user policies, see [Managed Policies and Inline Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html) in the *IAM User Guide*\. For more information about tagging instances, see [Tagging Your Amazon EC2 Resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) in the *Amazon EC2 User Guide for Linux Instances* \(content applies to Windows and Linux instances\)\. For more information increasing your security posture against unauthorized root\-level commands on your instances, see [Restrict Access to Root\-Level Commands Through SSM Agent](ssm-agent-restrict-root-level-commands.md)

## Example 3: Allow a User to Terminate Only Sessions They Started<a name="restrict-access-example-user-sessions"></a>

The following IAM policy lets a user view the IDs of all sessions in your account but interact with instances only through sessions they started\. A user who is assigned the following policy can't connect to or terminate other users' sessions\. The policy uses the variable `{aws:username}` to achieve this\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "ssm:DescribeSessions"
            ],
            "Effect": "Allow",
            "Resource": [
                "*"
            ]
        },
        {
            "Action": [
                "ssm:TerminateSession"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:ssm:*:*:session/${aws:username}-*"
            ]
        }
    ]
}
```

## Example 4: Allow Full \(Administrative\) Access to All Sessions<a name="restrict-access-example-full-access"></a>

The following IAM policy allows a user to fully interact with all instances and all sessions created by all users for all instances\. It should be granted only to an Administrator who needs full control over your organization's Session Manager activities\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "ssm:StartSession",
                "ssm:TerminateSession",
                "ssm:ResumeSession",
                "ssm:DescribeSessions",
                "ssm:GetConnectionStatus"
            ],
            "Effect": "Allow",
            "Resource": [
                "*"
            ]
        }
    ]
}
```