# Additional sample IAM policies for Session Manager<a name="getting-started-restrict-access-examples"></a>

Refer to the following example policies to help you create a custom AWS Identity and Access Management \(IAM\) policy for any Session Manager user access scenarios you want to support\.

**Topics**
+ [Example 1: Grant access to documents in the console](#grant-access-documents-console-example)
+ [Example 2: Restrict access to specific managed nodes](#restrict-access-example-instances)
+ [Example 3: Restrict access based on tags](#restrict-access-example-instance-tags)
+ [Example 4: Allow a user to end only sessions they started](#restrict-access-example-user-sessions)
+ [Example 5: Allow full \(administrative\) access to all sessions](#restrict-access-example-full-access)

## Example 1: Grant access to documents in the console<a name="grant-access-documents-console-example"></a>

You can allow users to specify a custom document when they launch a session using the Session Manager console\. The following example IAM policy grants permission to access documents with names that begin with **SessionDocument\-** in the specified AWS Region and AWS account\.

To use this policy, replace each *example resource placeholder* with your own information\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetDocument",
                "ssm:ListDocuments"
            ],
            "Resource": [
                "arn:aws:ssm:region:account-id:document/SessionDocument-*"
                
            ]
        }
    ]
}
```

**Note**  
The Session Manager console only supports Session documents that have a `sessionType` of `Standard_Stream` which are used to define session preferences\. For more information, see [Session document schema](session-manager-schema.md)\.

## Example 2: Restrict access to specific managed nodes<a name="restrict-access-example-instances"></a>

You can create an IAM policy that defines which managed nodes that a user is allowed to connect to using Session Manager\. For example, the following policy grants a user the permission to start, end, and resume their sessions on three specific nodes\. The policy restricts the user from connecting to nodes other than those specified\.

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
                "ssm:TerminateSession",
                "ssm:ResumeSession"
            ],
            "Resource": [
                "arn:aws:ssm:*:*:session/${aws:username}-*"
            ]
        }
    ]
}
```

## Example 3: Restrict access based on tags<a name="restrict-access-example-instance-tags"></a>

You can restrict access to managed nodes based on specific tags\. In the following example, the user is allowed to start and resume sessions \(`Effect: Allow, Action: ssm:StartSession, ssm:ResumeSession`\) on any managed node \(`Resource: arn:aws:ec2:region:987654321098:instance/*`\) with the condition that the node is a Finance WebServer \(`ssm:resourceTag/Finance: WebServer`\)\. If the user sends a command to a managed node that isn't tagged or that has any tag other than `Finance: WebServer`, the command result will include `AccessDenied`\.

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
                "arn:aws:ec2:us-east-2:123456789012:instance/*"
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
                "ssm:TerminateSession",
                "ssm:ResumeSession"
            ],
            "Resource": [
                "arn:aws:ssm:*:*:session/${aws:username}-*"
            ]
        }
    ]
}
```

You can create IAM policies that allow a user to start sessions to managed nodes that are tagged with multiple tags\. The following policy allows the user to start sessions to managed nodes that have both the specified tags applied to them\. If a user sends a command to a managed node that isn't tagged with both of these tags, the command result will include `AccessDenied`\.

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

For more information about creating IAM policies, see [Managed Policies and Inline Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html) in the *IAM User Guide*\. For more information about tagging managed nodes, see [Tagging managed nodes](tagging-managed-instances.md) and [Tagging your Amazon EC2 resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) in the *Amazon EC2 User Guide for Linux Instances* \(content applies to Windows and Linux managed nodes\)\. For more information about increasing your security posture against unauthorized root\-level commands on your managed nodes, see [Restricting access to root\-level commands through SSM Agent](ssm-agent-restrict-root-level-commands.md)

## Example 4: Allow a user to end only sessions they started<a name="restrict-access-example-user-sessions"></a>

Session Manager provides two methods to control which sessions a user in your AWS account is allowed to end\.
+ Use the variable `{aws:username}` in an AWS Identity and Access Management \(IAM\) permissions policy\. Users can end only sessions they started\. This method doesn't work for accounts that use federated IDs to grant access to AWS\. Federated IDs use the variable `{aws:userid}` instead of `{aws:username}`\.
+ Use tags supplied by AWS tags in an IAM permissions policy\. In the policy, you include a condition that allows users to end only sessions that are tagged with specific tags that have been provided by AWS\. This method works for all accounts, including those that use federated IDs to grant access to AWS\.

### Method 1: Grant TerminateSession privileges using the variable `{aws:username}`<a name="restrict-access-example-user-sessions-username"></a>

The following IAM policy allows a user to view the IDs of all sessions in your account\. However, users can interact with managed nodes only through sessions they started\. A user who is assigned the following policy can't connect to or end other users' sessions\. The policy uses the variable `{aws:username}` to achieve this\.

**Note**  
This method doesn't work for accounts that grant access to AWS using federated IDs\.

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

You can control which sessions that a user can end by including conditional tag key variables in an IAM policy\. The condition specifies that the user can only end sessions that are tagged with one or both of these specific tag key variables and a specified value\.

When a user in your AWS account starts a session, Session Manager applies two resource tags to the session\. The first resource tag is `aws:ssmmessages:target-id`, with which you specify the ID of the target the user is allowed to end\. The other resource tag is `aws:ssmmessages:session-id`, with a value in the format of `role-id:caller-specified-role-name`\.

**Note**  
Session Manager doesn’t support custom tags for this IAM access control policy\. You must use the resource tags supplied by AWS, described below\. 

 **`aws:ssmmessages:target-id`**   
With this tag key, you include the managed node ID as the value in policy\. In the following policy block, the condition statement allows a user to end only the node i\-02573cafcfEXAMPLE\.  

```
{
     "Version": "2012-10-17",
     "Statement": [
         {
             "Effect": "Allow",
             "Action": [
                "ssm:TerminateSession"
             ],
             "Resource": "*",
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

 **`aws:ssmmessages:session-id`**   
This tag key includes a variable for the session ID as the value in the request to start a session\.  
The following example demonstrates a policy for cases where the caller type is `User`\. The value you supply for `aws:ssmmessages:session-id` is the ID of the user\. In this example, `AIDIODR4TAW7CSEXAMPLE` represents the ID of a user in your AWS account\. To retrieve the ID for a user in your AWS account, use the IAM command, `get-user`\. For information, see [get\-user](https://docs.aws.amazon.com/IAM/latest/UserGuide/get-user.html) in the AWS Identity and Access Management section of the *IAM User Guide*\.   

```
{
     "Version": "2012-10-17",
     "Statement": [
         {
             "Effect": "Allow",
             "Action": [
                "ssm:TerminateSession"
             ],
             "Resource": "*",
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
To retrieve the role ID for a role in your AWS account, use the `get-caller-identity` command\. For information, see [get\-caller\-identity](https://docs.aws.amazon.com/cli/latest/reference/sts/get-caller-identity.html) in the AWS CLI Command Reference\.   

```
{
     "Version": "2012-10-17",
     "Statement": [
         {
             "Effect": "Allow",
             "Action": [
                "ssm:TerminateSession"
             ],
             "Resource": "*",
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

**`aws:ssmmessages:target-id`** and **`aws:ssmmessages:session-id`**  
You can also create IAM policies that allow a user to end sessions that are tagged with both system tags, as shown in this example\.  

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
                  "i-02573cafcfEXAMPLE"
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

## Example 5: Allow full \(administrative\) access to all sessions<a name="restrict-access-example-full-access"></a>

The following IAM policy allows a user to fully interact with all managed nodes and all sessions created by all users for all nodes\. It should be granted only to an Administrator who needs full control over your organization's Session Manager activities\.

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