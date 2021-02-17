# Setting up Run Command<a name="sysman-rc-setting-up"></a>

Before you can manage instances by using Run Command, you must configure an AWS Identity and Access Management \(IAM\) user policy for any user who will run commands\. For more information, see [ Create non\-Admin IAM users and groups for Systems Manager](systems-manager-setting-up.md)\.

You must also create an IAM instance profile role for any instance that will process commands and attach it to those instances\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md) and [Attach an IAM instance profile to an EC2 instance](setup-launch-managed-instance.md)\. 

We also strongly recommend completing the following optional setup tasks to help minimize the security posture and day\-to\-day management of your instances\.

Monitor command executions using Amazon EventBridge  
You can use Amazon EventBridge to log command execution status changes\. You can create a rule that runs whenever there is a state transition, or when there is a transition to one or more states that are of interest\. You can also specify Run Command as a target action when an EventBridge event occurs\. For more information, see [Configuring EventBridge for Systems Manager events](monitoring-systems-manager-events.md)\.

Monitor command executions using Amazon CloudWatch Logs  
You can configure Run Command to periodically send all command output and error logs to a CloudWatch Logs log group\. You can monitor these output logs in near real\-time, search for specific phrases, values, or patterns, and create alarms based on the search\. For more information, see [Configuring Amazon CloudWatch Logs for Run Command](sysman-rc-setting-up-cwlogs.md)\.

Restrict Run Command access to specific instances  
You can restrict which of your managed instances commands can be run on by creating an IAM user policy that includes a condition that the user can only run commands on instances that are tagged with specific Amazon EC2 tags\. For more information, see the following topic, [Restricting Run Command access based on instance tags](#sysman-rc-setting-up-cmdsec)\.

## Restricting Run Command access based on instance tags<a name="sysman-rc-setting-up-cmdsec"></a>

You can restrict command execution to specific instances by creating an IAM user policy that includes a condition that the user can only run commands on instances that are tagged with specific Amazon EC2 tags\. In the following example, the user is allowed to use Run Command \(`Effect: Allow, Action: ssm:SendCommand`\) by using any SSM document \(`Resource: arn:aws:ssm:*:*:document/*`\) on any instance \(`Resource: arn:aws:ec2:*:*:instance/*`\) with the condition that the instance is a Finance WebServer \(`ssm:resourceTag/Finance: WebServer`\)\. If the user sends a command to an instance that is not tagged or that has any tag other than `Finance: WebServer`, the execution results show `AccessDenied`\.

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "ssm:SendCommand"
         ],
         "Resource":[
            "arn:aws:ssm:*:*:document/*"
         ]
      },
      {
         "Effect":"Allow",
         "Action":[
            "ssm:SendCommand"
         ],
         "Resource":[
            "arn:aws:ec2:*:*:instance/*"
         ],
         "Condition":{
            "StringLike":{
               "ssm:resourceTag/Finance":[
                  "WebServers"
               ]
            }
         }
      }
   ]
}
```

You can create IAM policies that enable a user to run commands on instances that are tagged with multiple tags\. The following policy enables the user to run commands on instances that have two tags\. If a user sends a command to an instance that is not tagged with both of these tags, the execution results show `AccessDenied`\.

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "ssm:SendCommand"
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
      },
      {
         "Effect":"Allow",
         "Action":[
            "ssm:SendCommand"
         ],
         "Resource":[
            "arn:aws:ssm:us-west-1::document/AWS-*",
            "arn:aws:ssm:us-east-2::document/AWS-*"
         ]
      },
      {
         "Effect":"Allow",
         "Action":[
            "ssm:UpdateInstanceInformation",
            "ssm:ListCommands",
            "ssm:ListCommandInvocations",
            "ssm:GetDocument"
         ],
         "Resource":"*"
      }
   ]
}
```

You can also create IAM policies that enable a user to run commands on multiple groups of tagged instances\. The following policy enables the user to run commands on either group of tagged instances, or both groups\.

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "ssm:SendCommand"
         ],
         "Resource":"*",
         "Condition":{
            "StringLike":{
               "ssm:resourceTag/tag_key1":[
                  "tag_value1"
               ]
            }
         }
      },
      {
         "Effect":"Allow",
         "Action":[
            "ssm:SendCommand"
         ],
         "Resource":"*",
         "Condition":{
            "StringLike":{
               "ssm:resourceTag/tag_key2":[
                  "tag_value2"
               ]
            }
         }
      },
      {
         "Effect":"Allow",
         "Action":[
            "ssm:SendCommand"
         ],
         "Resource":[
            "arn:aws:ssm:us-west-1::document/AWS-*",
            "arn:aws:ssm:us-east-2::document/AWS-*"
         ]
      },
      {
         "Effect":"Allow",
         "Action":[
            "ssm:UpdateInstanceInformation",
            "ssm:ListCommands",
            "ssm:ListCommandInvocations",
            "ssm:GetDocument"
         ],
         "Resource":"*"
      }
   ]
}
```

For more information about creating IAM user policies, see [Managed Policies and Inline Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html) in the *IAM User Guide*\. For more information about tagging instances, see [Tagging your Amazon EC2 resources](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) in the *Amazon EC2 User Guide for Linux Instances* \(content applies to Windows and Linux instances\)\. 