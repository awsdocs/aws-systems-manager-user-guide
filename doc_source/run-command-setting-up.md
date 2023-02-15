# Setting up Run Command<a name="run-command-setting-up"></a>

Before you can manage nodes by using Run Command, a capability of AWS Systems Manager, configure an AWS Identity and Access Management \(IAM\) policy for any user who will run commands\.

You must also configure your nodes for Systems Manager\. For more information, see [Setting up AWS Systems Manager](systems-manager-setting-up.md)\.

We recommend completing the following optional setup tasks to help minimize the security posture and day\-to\-day management of your managed nodes\.

Monitor command executions using Amazon EventBridge  
You can use EventBridge to log command execution status changes\. You can create a rule that runs whenever there is a state transition, or when there is a transition to one or more states that are of interest\. You can also specify Run Command as a target action when an EventBridge event occurs\. For more information, see [Configuring EventBridge for Systems Manager events](monitoring-systems-manager-events.md)\.

Monitor command executions using Amazon CloudWatch Logs  
You can configure Run Command to periodically send all command output and error logs to an Amazon CloudWatch log group\. You can monitor these output logs in near real\-time, search for specific phrases, values, or patterns, and create alarms based on the search\. For more information, see [Configuring Amazon CloudWatch Logs for Run Command](sysman-rc-setting-up-cwlogs.md)\.

Restrict Run Command access to specific managed nodes  
You can restrict a user's ability to run commands on managed nodes by using AWS Identity and Access Management \(IAM\)\. Specifically, you can create an IAM policy with a condition that the user can only run commands on managed nodes that are tagged with specific tags\. For more information, see [Restricting Run Command access based on tags](#tag-based-access)\.

## Restricting Run Command access based on tags<a name="tag-based-access"></a>

This section describes how to restrict a user's ability to run commands on managed instances by specifying a tag condition in an IAM policy\. Managed instances include Amazon EC2 instances and on\-premises servers, edge devices, and VMs in a hybrid environment that are configured for Systems Manager\. Though the information is not explicitly presented, you can also restrict access to managed AWS IoT Greengrass core devices\. To get started, you must tag your AWS IoT Greengrass devices\. For more information, see [Tag your AWS IoT Greengrass Version 2 resources](https://docs.aws.amazon.com/greengrass/v2/developerguide/tag-resources.html) in the *AWS IoT Greengrass Version 2 Developer Guide*\.

You can restrict command execution to specific managed nodes by creating an IAM policy that includes a condition that the user can only run commands on nodes with specific tags\. In the following example, the user is allowed to use Run Command \(`Effect: Allow, Action: ssm:SendCommand`\) by using any SSM document \(`Resource: arn:aws:ssm:*:*:document/*`\) on any node \(`Resource: arn:aws:ec2:*:*:instance/*`\) with the condition that the node is a Finance WebServer \(`ssm:resourceTag/Finance: WebServer`\)\. If the user sends a command to a node that isn't tagged or that has any tag other than `Finance: WebServer`, the execution results show `AccessDenied`\.

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

You can create IAM policies that allow a user to run commands on managed nodes that are tagged with multiple tags\. The following policy allows the user to run commands on managed nodes that have two tags\. If a user sends a command to a node that isn't tagged with both of these tags, the execution results show `AccessDenied`\.

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

You can also create IAM policies that allows a user to run commands on multiple groups of tagged managed nodes\. The following example policy allows the user to run commands on either group of tagged nodes, or both groups\.

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

For more information about creating IAM policies, see [Managed policies and inline policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html) in the *IAM User Guide*\. For more information about tagging managed nodes, see [Tag Editor](https://docs.aws.amazon.com/ARG/latest/userguide/tag-editor.html) in the *AWS Resource Groups User Guide*\. 