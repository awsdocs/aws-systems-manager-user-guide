# Additional sample IAM policies for Session Manager<a name="getting-started-restrict-access-examples"></a>

Refer to the following example policies to help you create a custom IAM policy for any Session Manager user access scenarios you want to support\.

**Topics**
+ [Example 1: Restrict access to specific instances](#restrict-access-example-instances)
+ [Example 2: Restrict access based on instance tags](#restrict-access-example-instance-tags)
+ [Example 3: Allow a user to end only sessions they started](#restrict-access-example-user-sessions)
+ [Example 4: Allow full \(administrative\) access to all sessions](#restrict-access-example-full-access)

## Example 1: Restrict access to specific instances<a name="restrict-access-example-instances"></a>

You can restrict access to specific instances by creating an IAM user policy that includes the IDs of the instances\. In the following example, the user is allowed Session Manager access to three specific instances only, and allowed to end only their sessions on those instances\. If the user sends a command to any other instance or tries to end any other session, the command result will include `AccessDenied`\.

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

## Example 2: Restrict access based on instance tags<a name="restrict-access-example-instance-tags"></a>

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

You can create IAM policies that enable a user to start sessions to instances that are tagged with multiple tags\. The following policy enables the user to start sessions to instances that have both the specified tags applied to them\. If a user sends a command to an instance that is not tagged with both of these tags, the command result will include `AccessDenied`\.

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
               "ssm:resourceTag/tag-key1":[
                  "tag-value1"
               ],
               "ssm:resourceTag/tag-key2":[
                  "tag-value2"
               ]
            }
         }
      }
   ]
}
```

For more information about creating IAM user policies, see [Managed Policies and Inline Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html) in the *IAM User Guide*\. For more information about tagging instances, see [Tagging your Amazon EC2 resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) in the *Amazon EC2 User Guide for Linux Instances* \(content applies to Windows and Linux instances\)\. For more information increasing your security posture against unauthorized root\-level commands on your instances, see [Restricting access to root\-level commands through SSM Agent](ssm-agent-restrict-root-level-commands.md)

## Example 3: Allow a user to end only sessions they started<a name="restrict-access-example-user-sessions"></a>

Session Manager provides two methods to control which sessions a user in your AWS account is allowed to end\.
+ Use the variable `{aws:username}` in an AWS Identity and Access Management \(IAM\) permissions policy\. Users can end only sessions they started\. This method does not work for accounts that use federated IDs to grant access to AWS\. Federated IDs use the variable `{aws:userid}` instead of `{aws:username}`\.
+ Use tags supplied by AWS tags in an IAM permissions policy\. In the policy, you include a condition that allows users to end only sessions that are tagged with specific tags that have been provided by AWS\. This method works for all accounts, including those that use federated IDs to grant access to AWS\.

### Method 1: Grant TerminateSession privileges using the variable `{aws:username}`<a name="restrict-access-example-user-sessions-username"></a>

The following IAM policy lets a user view the IDs of all sessions in your account\. However, users can interact with instances only through sessions they started\. A user who is assigned the following policy can't connect to or end other users' sessions\. The policy uses the variable `{aws:username}` to achieve this\.

**Note**  
This method does not work for accounts that grant access to AWS using federated IDs\.

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

### Method 2: Grant TerminateSession privileges using tags supplied by AWS<a name="restrict-access-example-user-sessions-tags"></a>

You can control which sessions a user can end by using a condition with specific tag key variables in an IAM user policy\. The condition specifies that the user can only end sessions that are tagged with one or both of these specific tag key variables and a specified value\.

When a user in your AWS account starts a session, Session Manager applies two resource tags to the session\. The first resource tag is `aws:ssmmessages:target-id`, with which you specify the ID of the target the user is allowed to end\. The other resource tag is `aws:ssmmessages:session-id`, with a value in the format of `role-id:caller-specified-role-name`\.

**Note**  
Session Manager doesn’t support custom tags for this IAM access control policy\. You must use the resource tags supplied by AWS, described below\. 

 **aws:ssmmessages:target\-id**   
With this tag key, you include the instance ID as the value in policy\. In the following policy block, the condition statement lets a user end only the instance i\-02573cafcfEXAMPLE:  

```
{
     "Version": "2012-10-17",
     "Statement": [
         {
             "Effect": "Allow",
             "Action": [
                "ssm:TerminateSession"
             ],
             "Resource": "",
             "Condition": {
                 "StringLike": {
                     "ssm:resourceTag/aws:ssmmessages:target-id": [
                        "i-02573cafcfEXAMPLE"
                     ]
                 }
             }
         }
     ]
}
```
If the user tries to end a session for which they haven’t been granted this `TerminateSession` permission, they receive an `AccessDeniedException` error\.

 **aws:ssmmessages:session\-id**   
This tag key includes a variable for the session ID as the value in the request to start a session\.  
The following example demonstrates a policy for cases where the caller type is User\. The value you supply for `aws:ssmmessages:session-id` is the ID of the user\. In this example, `AIDIODR4TAW7CSEXAMPLE` represents the ID of a user in your AWS account\. To retrieve the ID for a user in your AWS account, use the IAM command, `get-user`\. For information, see [get\-user](https://docs.aws.amazon.com/IAM/latest/UserGuide/get-user.html) in the AWS Identity and Access Management section of the *IAM User Guide*\.   

```
{
     "Version": "2012-10-17",
     "Statement": [
         {
             "Effect": "Allow",
             "Action": [
                "ssm:TerminateSession"
             ],
             "Resource": "",
             "Condition": {
                 "StringLike": {
                     "ssm:resourceTag/aws:ssmmessages:session-id": [
                        "AIDIODR4TAW7CSEXAMPLE"
                     ]
                 }
             }
         }
     ]
}
```
The following example demonstrates a policy for cases where the caller type is `AssumedRole`\. You can use the `{aws:userid}` variable for the value you supply for `aws:ssmmessages:session-id`\. Alternatively, you can hardcode a role ID for the value you supply for `aws:ssmmessages:session-id`\. If you hardcode a role ID, you must provide the value in the format `role-id:caller-specified-role-name`\. For example, `AIDIODR4TAW7CSEXAMPLE:MyRole`\.  
In order for system tags to be applied, the role ID you supply can contain the following characters only: Unicode letters, 0\-9, space, `_`, `.`, `:`, `/`, `=`, `+`, `-`, `@`, and `\`\.
To retrieve the role ID for a role in your AWS account, use the `get-caller-identity` command\. For information, see [get\-caller\-identity](https://docs.aws.amazon.com/cli/latest/reference//sts/get-caller-identity.html) in the AWS CLI Command Reference\.   

```
{
     "Version": "2012-10-17",
     "Statement": [
         {
             "Effect": "Allow",
             "Action": [
                "ssm:TerminateSession"
             ],
             "Resource": "",
             "Condition": {
                 "StringLike": {
                     "ssm:resourceTag/aws:ssmmessages:session-id": [
                        "${aws:userid}"
                     ]
                 }
             }
         }
     ]
}
```
If a user tries to end a session for which they haven’t been granted this `TerminateSession` permission, they receive an `AccessDeniedException` error\.

**aws:ssmmessages:target\-id** and **aws:ssmmessages:session\-id**  
You can also create IAM policies that enable a user to end sessions that are tagged with both system tags, as shown in this example\.  

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "ssm:TerminateSession"
         ],
         "Resource":"*",
         "Condition":{
            "StringLike":{
               "ssm:resourceTag/aws:ssmmessages:target-id":[
                  "instance-id"
               ],
               "ssm:resourceTag/aws:ssmmessages:session-id":[
                  "${aws:username}-*"
               ]
            }
         }
      }
   ]
}
```

## Example 4: Allow full \(administrative\) access to all sessions<a name="restrict-access-example-full-access"></a>

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