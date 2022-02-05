# Control access to maintenance windows \(console\)<a name="sysman-maintenance-perm-console"></a>

The following procedures describe how to use the AWS Systems Manager console to create the required roles and permissions for maintenance windows\.

**Topics**
+ [Task 1: \(Optional\) Create a custom service role for maintenance windows \(console\)](#sysman-maintenance-role)
+ [Task 2: Configure permissions for users who are allowed to register maintenance window tasks \(console\)](#sysman-maintenance-passrole)
+ [Task 3: Configure permissions for users who aren't allowed to register maintenance window tasks \(console\)](#mw-deny-task-registrations)

## Task 1: \(Optional\) Create a custom service role for maintenance windows \(console\)<a name="sysman-maintenance-role"></a>

Use the following procedure to create a custom service role for Maintenance Windows, a capability of Systems Manager, so that Systems Manager can run tasks on your behalf\.

**Important**  
A custom service role isn't required if you choose to use a Systems Manager service\-linked role to let maintenance windows run tasks on your behalf instead\. If you don't have a Systems Manager service\-linked role in your account, you can create it when you create or update a maintenance window task using the Systems Manager console\. For more information, see the following topics:  
[Should I use a service\-linked role or a custom service role to run maintenance window tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)
[Using service\-linked roles for Systems Manager](using-service-linked-roles.md)
[Assign tasks to a maintenance window \(console\)](sysman-maintenance-assign-tasks.md)

**To create a custom service role \(console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. Mark the following selections:

   1. ** Select type of trusted entity** area: **AWS service**

   1. **Choose a use case** area: **Systems Manager**

   1. **Select your use case** area: **Systems Manager**
**Note**  
You are choosing the option **Systems Manager** instead of **Systems Manager \- Inventory and Maintenance Windows** in order to create a custom role just for use with maintenance windows\. Choosing **Systems Manager \- Inventory and Maintenance Windows** leads you to create a service\-linked role for use with multiple Systems Manager capabilities\.

1. Choose **Next: Permissions**\. 

1. In the search box, enter **AmazonSSMMaintenanceWindowRole**, select the box next to **AmazonSSMMaintenanceWindowRole**, and then choose **Next: Tags**\.

1. \(Optional\) Add one or more tag\-key value pairs to organize, track, or control access for this role, and then choose **Next: Review**\. 

1. For **Role name**, enter a name that identifies this role as a Maintenance Windows role\. For example: **my\-maintenance\-window\-role**\.

1. \(Optional\) Change the default role description to reflect the purpose of this role\. For example: **Performs maintenance window tasks on your behalf**\.

1. Choose **Create role**\. The system returns you to the **Roles** page\.

1. Choose the name of the role you just created\.

1. Choose the **Trust relationships** tab, and then choose **Edit trust relationship**\.

1. Verify that the following policy is displayed in the **Policy Document** field\.

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

1. Choose **Update Trust Policy**, and then copy or make a note of the role name and the **Role ARN** value on the **Summary** page\. You specify this information when you create your maintenance window\.

1. \(Optional\) If you plan to configure a maintenance window to send notifications about command statuses using Amazon Simple Notification Service \(Amazon SNS\), when run through a Systems Manager Run Command command task, do the following:

   1. Choose the **Permissions** tab\.

   1. Choose **Add inline policy**, and then choose the **JSON** tab\.

   1. In **Policy Document**, replace the default contents with the following\.

      ```
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "arn:aws::iam::account-id:role/my-sns-access-role"
          }
        ]
      }
      ```

      **account\-id** represents the the 12\-digit identifier for your AWS account, in the format `123456789012`\.

      *my\-sns\-access\-role* represents the name of the existing AWS Identity and Access Management \(IAM\) role to use to send Amazon SNS notifications related to the maintenance window`.`

      For information about configuring Amazon SNS notifications for Systems Manager, including information about creating an IAM role to use for sending SNS notifications, see [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md)\.
**Note**  
In the Systems Manager console, this ARN is selected in the ** IAM Role** list on the **Register run command task** page\. For information, see [Assign tasks to a maintenance window \(console\)](sysman-maintenance-assign-tasks.md)\. In the Systems Manager API, this ARN is entered as the value of [ServiceRoleArn](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendCommand.html#EC2-SendCommand-request-ServiceRoleArn) in the [SendCommand](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendCommand.html) request\.

   1. Choose **Review policy**\.

   1. For **Name**, enter a name to identify this as a policy to allow sending Amazon SNS notifications\.

1. Choose **Create policy**\.

## Task 2: Configure permissions for users who are allowed to register maintenance window tasks \(console\)<a name="sysman-maintenance-passrole"></a>

When you register a task with a maintenance window, you specify either a custom service role or a Systems Manager service\-linked role to run the actual task operations\. This is the role that the service assumes when it runs tasks on your behalf\. Before that, to register the task itself, assign the IAM PassRole policy to an IAM user account or an IAM group\. This allows the IAM user or IAM group to specify, as part of registering those tasks with the maintenance window, the role that should be used when running tasks\. For information, see [Granting a user permissions to pass a role to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html) in the *IAM User Guide*\.

Depending on whether you're assigning the `iam:Passrole` permission to an individual user or a group, use one of the following procedures to provide the minimum permissions required to register tasks with a maintenance window\.

**To configure permissions for users who are allowed to register maintenance window tasks \(console\)**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Users**, and then choose the name of the user account you want to update\.

1. On the **Permissions** tab, in the policies list, verify that the `AmazonSSMFullAccess` policy is listed, or that there is a comparable policy that gives the IAM user permission to call the Systems Manager API\. Add the permission if it isn't included already\. For information, see [Adding and removing IAM identity permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html#add-remove-policies-console) in the *IAM User Guide*\.

1. Choose **Add inline policy**, and then choose the **JSON** tab\.

1. Replace the contents of the box with the following\.

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

1. Replace the defaut contents of the box with the following\.

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

## Task 3: Configure permissions for users who aren't allowed to register maintenance window tasks \(console\)<a name="mw-deny-task-registrations"></a>

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