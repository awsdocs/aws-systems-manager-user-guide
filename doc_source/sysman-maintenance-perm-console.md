# Use the console to configure permissions for maintenance windows<a name="sysman-maintenance-perm-console"></a>

The following procedures describe how to use the AWS Systems Manager console to create the required roles and permissions for maintenance windows\.

**Topics**
+ [Task 1: Create a policy for your custom maintenance window service role](#sysman-maintenance-role-policy)
+ [Task 2: Create a custom service role for maintenance windows \(console\)](#sysman-maintenance-role)
+ [Task 3: Configure permissions for users who are allowed to register maintenance window tasks \(console\)](#sysman-maintenance-passrole)
+ [Task 4: Configure permissions for users who aren't allowed to register maintenance window tasks \(console\)](#mw-deny-task-registrations)

## Task 1: Create a policy for your custom maintenance window service role<a name="sysman-maintenance-role-policy"></a>

You can use the following policy in JSON format to create the policy to use with your maintenance window role\. You attach this policy to the role that you create later in [Task 2: Create a custom service role for maintenance windows \(console\)](#sysman-maintenance-role)\.

**Important**  
Depending on the tasks and types of tasks your maintenance windows run, you might not need all the permissions in this policy, and you might need to include additional permissions\. 

**To create a policy for your custom maintenance window service role**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create Policy**\.

1. Choose the **JSON** tab\.

1. Replace the default contents with the following:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "ssm:SendCommand",
                   "ssm:CancelCommand",
                   "ssm:ListCommands",
                   "ssm:ListCommandInvocations",
                   "ssm:GetCommandInvocation",
                   "ssm:GetAutomationExecution",
                   "ssm:StartAutomationExecution",
                   "ssm:ListTagsForResource",
                   "ssm:GetParameters"
               ],
               "Resource": "*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "states:DescribeExecution",
                   "states:StartExecution"
               ],
               "Resource": [
                   "arn:aws:states:*:*:execution:*:*",
                   "arn:aws:states:*:*:stateMachine:*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "lambda:InvokeFunction"
               ],
               "Resource": [
                   "arn:aws:lambda:*:*:function:*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "resource-groups:ListGroups",
                   "resource-groups:ListGroupResources"
               ],
               "Resource": [
                   "*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": [
                   "tag:GetResources"
               ],
               "Resource": [
                   "*"
               ]
           },
           {
               "Effect": "Allow",
               "Action": "iam:PassRole",
               "Resource": "*",
               "Condition": {
                   "StringEquals": {
                       "iam:PassedToService": [
                           "ssm.amazonaws.com"
                       ]
                   }
               }
           }
       ]
   }
   ```

1. Modify the JSON content as needed for the maintenance tasks that you run in your account\. The changes you make are specific to your planned operations\. 

   For example:
   + You can provide Amazon Resource Names \(ARNs\) for specific functions and state machines instead of using wildcard \(\*\) qualifiers\.
   + If you don’t plan to run AWS Step Functions tasks, you can remove the `states` permissions and \(ARNs\)\.
   + If you don’t plan to run AWS Lambda tasks, you can remove the `lambda` permissions and ARNs\.
   + If you don't plan to run Automation tasks, you can remove the `ssm:GetAutomationExecution` and `ssm:StartAutomationExecution` permissions\.
   + Add additional permissions that might be needed for the tasks to run\. For example, some Automation actions work with AWS CloudFormation stacks\. Therefore, the permissions `cloudformation:CreateStack`, `cloudformation:DescribeStacks`, and `cloudformation:DeleteStack` are required\. 

     For another example, the Automation runbook `AWS-CopySnapshot` requires permissions to create an Amazon Elastic Block Store \(Amazon EBS\) snapshot\. Therefore, the service role needs the permissions `ec2:CreateSnapshot`\. 

     For information about the role permissions needed by Automation runbooks, see the document descriptions in the *[AWS Systems Manager Automation runbook reference](https://docs.aws.amazon.com/systems-manager-automation-runbooks/latest/userguide/automation-runbook-reference.html)*\.

1. After completing the policy revisions, choose **Next: Tags**\.

1. \(Optional\) Add one or more tag\-key value pairs to organize, track, or control access for this policy, and then choose **Next: Review**\. 

1. For **Name**, enter a name that identifies this as the policy that the Maintenance Windows service role you create uses\. For example: **my\-maintenance\-window\-role\-policy**\.

1. Choose **Create policy**, and make a note of the name you specified for the policy\. You refer to it in the next procedure, [Task 2: Create a custom service role for maintenance windows \(console\)](#sysman-maintenance-role)\.

## Task 2: Create a custom service role for maintenance windows \(console\)<a name="sysman-maintenance-role"></a>

Use the following procedure to create a custom service role for Maintenance Windows, a capability of Systems Manager, so that Systems Manager can run tasks on your behalf\. In this task, you attach the policy you created in the previous task to the role you create\.

**Important**  
Previously, the Systems Manager console provided you with the ability to choose the AWS managed IAM service\-linked role `AWSServiceRoleForAmazonSSM` to use as the maintenance role for your tasks\. Using this role and its associated policy, `AmazonSSMServiceRolePolicy`, for maintenance window tasks is no longer recommended\. If you're using this role for maintenance window tasks now, we encourage you to stop using it\. Instead, create your own IAM role that enables communication between Systems Manager and other AWS services when your maintenance window tasks run\.

**To create a custom service role \(console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. For **Select trusted entity**, make the following choices:

   1. For **Trusted entity type**, choose **AWS service**

   1. For **Use cases for other AWS services**, choose **Systems Manager**

   1. Choose **Systems Manager**, as shown in the following image\.  
![\[Screenshot illustrating the Systems Manager option selected as a use case.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/iam_use_cases_for_MWs.png)

1. Choose **Next**\. 

1. In the search box, enter the name of the policy you created in [Task 1: Create a policy for your custom maintenance window service role](#sysman-maintenance-role-policy), select the box next to it sname, and then choose **Next**\.

1. For **Role name**, enter a name that identifies this role as a Maintenance Windows role\. For example: **my\-maintenance\-window\-role**\.

1. \(Optional\) Change the default role description to reflect the purpose of this role\. For example: **Performs maintenance window tasks on your behalf**\.

1. \(Optional\) Add one or more tag\-key value pairs to organize, track, or control access for this role, and then choose **Next: Review**\. 

1. Choose **Create role**\. The system returns you to the **Roles** page\.

1. Choose the name of the role you just created\.

1. Choose the **Trust relationships** tab, and then verify that the following policy is displayed in the **Trusted entities** box\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "",
         "Effect": "Allow",
         "Principal": {
           "Service": "ssm.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Copy or make a note of the role name and the **ARN** value in the **Summary** area\. Users in your account specify this information when they create maintenance windows\.

## Task 3: Configure permissions for users who are allowed to register maintenance window tasks \(console\)<a name="sysman-maintenance-passrole"></a>

When you register a task with a maintenance window, you specify either a custom service role or a Systems Manager service\-linked role to run the actual task operations\. This is the role that the service assumes when it runs tasks on your behalf\. Before that, to register the task itself, assign the IAM PassRole policy to an IAM user account or an IAM group\. This allows the IAM user or IAM group to specify, as part of registering those tasks with the maintenance window, the role that should be used when running tasks\. For information, see [Granting a user permissions to pass a role to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html) in the *IAM User Guide*\.

Depending on whether you're assigning the `iam:Passrole` permission to an individual user or a group, use one of the following procedures to provide the minimum permissions required to register tasks with a maintenance window\.

**To configure permissions for users who are allowed to register maintenance window tasks \(console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Users**, and then choose the name of the user account you want to update\.

1. On the **Permissions** tab, in the policies list, verify that the `AmazonSSMFullAccess` policy is listed, or that there is a comparable policy that gives the IAM user permission to call the Systems Manager API\. Add the permission if it isn't included already\. For information, see [Adding and removing IAM identity permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html#add-remove-policies-console) in the *IAM User Guide*\.

1. Choose **Add inline policy**, and then choose the **JSON** tab\.

1. Replace the default contents of the box with the following\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "iam:PassRole",
               "Resource": "arn:aws:iam::account-id:role/my-maintenance-window-role"
           },
           {
               "Effect": "Allow",
               "Action": "iam:ListRoles",
               "Resource": "arn:aws:iam::account-id:role/"
           },
           {
               "Effect": "Allow",
               "Action": "iam:ListRoles",
               "Resource": "arn:aws:iam::account-id:role/aws-service-role/ssm.amazonaws.com/"
           }
       ]
   }
   ```

   *my\-maintenance\-window\-role* represents the name of the custom maintenance window role you created earlier\.

   *account\-id* represents the ID of your AWS account\. Adding this permission for the resource `arn:aws:iam::account-id:role/` allows a user to view and choose from customer roles in the console when they create a maintenance window task\. Adding this permission for `arn:aws:iam::account-id:role/aws-service-role/ssm.amazonaws.com/` allows a user to choose the Systems Manager service\-linked role in the console when they create a maintenance window task\. 

1. Choose **Review policy**\.

1. On the **Review policy** page, enter a name in the **Name** box to identify this `PassRole` policy, such as **my\-iam\-passrole\-policy**, and then choose **Create policy**\.

**To configure permissions for groups that are allowed to register maintenance window tasks \(console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **User groups**\.

1. In the list of groups, select the name of the group you want to assign the `iam:PassRole` permission to\. 

1. On the **Permissions** tab, choose **Add permissions, Create inline policy**, and then choose the **JSON** tab\.

1. Replace the default contents of the box with the following\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "iam:PassRole",
               "Resource": "arn:aws:iam::account-id:role/my-maintenance-window-role"
           },
           {
               "Effect": "Allow",
               "Action": "iam:ListRoles",
               "Resource": "arn:aws:iam::account-id:role/"
           },
           {
               "Effect": "Allow",
               "Action": "iam:ListRoles",
               "Resource": "arn:aws:iam::account-id:role/aws-service-role/ssm.amazonaws.com/"
           }
       ]
   }
   ```

   *my\-maintenance\-window\-role* represents the name of the custom maintenance window role you created earlier\.

   *account\-id* represents the ID of your AWS account\. Adding this permission for the resource `arn:aws:iam::account-id:role/` allows a user to view and choose from customer roles in the console when they create a maintenance window task\. Adding this permission for `arn:aws:iam::account-id:role/aws-service-role/ssm.amazonaws.com/` allows a user to choose the Systems Manager service\-linked role in the console when they create a maintenance window task\. 

1. Choose **Review policy**\.

1. On the **Review policy** page, enter a name in the **Name** box to identify this `PassRole` policy, such as **my\-group\-iam\-passrole\-policy**, and then choose **Create policy**\.

## Task 4: Configure permissions for users who aren't allowed to register maintenance window tasks \(console\)<a name="mw-deny-task-registrations"></a>

Depending on whether you're denying the `ssm:RegisterTaskWithMaintenanceWindow` permission for an individual user or a group, use one of the following procedures to prevent users from registering tasks with a maintenance window\.

**To configure permissions for users who aren't allowed to register maintenance window tasks \(console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Users**, and then choose the name of the user account you want to update\.

1. Choose **Add inline policy**, and then choose the **JSON** tab\.

1. Replace the default contents of the box with the following\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Deny",
               "Action": "ssm:RegisterTaskWithMaintenanceWindow",
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. On the **Review policy** page, enter a name in the **Name** box to identify this policy, such as **my\-deny\-mw\-tasks\-policy**, and then choose **Create policy**\.

**To configure permissions for groups that aren't allowed to register maintenance window tasks \(console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **User groups**\.

1. In the list of groups, select the name of the group you want to deny the `ssm:RegisterTaskWithMaintenanceWindow` permission from\. 

1. On the **Permissions** tab, choose **Add permissions, Create inline policy**\.

1. Choose the **JSON** tab, and then replace the default contents of the box with the following\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Deny",
               "Action": "ssm:RegisterTaskWithMaintenanceWindow",
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter a name to identify this policy, such as **my\-groups\-deny\-mw\-tasks\-policy**, and then choose **Create policy**\.