# Control access to maintenance windows \(AWS CLI\)<a name="sysman-maintenance-perm-cli"></a>

The following procedures describe how to use the AWS CLI to create the required roles and permissions for Maintenance Windows\.

**Topics**
+ [Task 1: \(Optional\) Create a custom service role for maintenance windows \(AWS CLI\)](#sysman-maintenance-role-cli)
+ [Task 2: Configure permissions for users who are allowed to register maintenance window tasks \(AWS CLI\)](#sysman-mw-passrole-cli)
+ [Task 3: Configure permissions for users who are not allowed to register maintenance window tasks \(AWS CLI\)](#mw-deny-task-registrations-cli)

## Task 1: \(Optional\) Create a custom service role for maintenance windows \(AWS CLI\)<a name="sysman-maintenance-role-cli"></a>

**Important**  
A custom service role is not required if you choose to use a Systems Manager service\-linked role to let maintenance windows run tasks on your behalf instead\. If you do not have a Systems Manager service\-linked role in your account, you can create it when you create or update a maintenance window task using the Systems Manager console\. For more information, see the following topics:  
[Should I use a service\-linked role or a custom service role to run maintenance window tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)
[Using service\-linked roles for Systems Manager](using-service-linked-roles.md)
[Assign tasks to a maintenance window \(console\)](sysman-maintenance-assign-tasks.md)

1. Copy and paste the following trust policy into a text file\. Save the file with the following name and file extension: `mw-role-trust-policy.json`\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Principal": {
           "Service": "ssm.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
       }
     ]
   }
   ```

1. Open the AWS CLI and run the following command in the directory where you placed `mw-role-trust-policy.json` in order to create a maintenance window role called `my-maintenance-window-role`\. The command assigns the policy you created in the previous step to this role\.

------
#### [ Linux ]

   ```
   aws iam create-role \
       --role-name "my-maintenance-window-role" \
       --assume-role-policy-document file://mw-role-trust-policy.json
   ```

------
#### [ Windows ]

   ```
   aws iam create-role ^
       --role-name "my-maintenance-window-role" ^
       --assume-role-policy-document file://mw-role-trust-policy.json
   ```

------

   The system returns information like the following\.

   ```
   {
       "Role": {
           "AssumeRolePolicyDocument": {
               "Version": "2012-10-17",
               "Statement": [
                   {
                       "Action": "sts:AssumeRole",
                       "Effect": "Allow",
                       "Principal": {
                           "Service": "ssm.amazonaws.com"
                       }
                   }
               ]
           },
           "RoleId": "AROAIIZKPBKS2LEXAMPLE",
           "CreateDate": "2017-04-04T03:40:17.373Z",
           "RoleName": "my-maintenance-window-role",
           "Path": "/",
           "Arn": "arn:aws:iam::123456789012:role/my-maintenance-window-role"
       }
   }
   ```
**Note**  
Make a note of the `RoleName` and the `Arn` values\. You specify these when you create a maintenance window that uses this custom role\.

1. Run the following command to attach the `AmazonSSMMaintenanceWindowRole` managed policy to the role you created in step 2\.

------
#### [ Linux ]

   ```
   aws iam attach-role-policy \
       --role-name "my-maintenance-window-role" \
       --policy-arn "arn:aws:iam::aws:policy/service-role/AmazonSSMMaintenanceWindowRole"
   ```

------
#### [ Windows ]

   ```
   aws iam attach-role-policy ^
       --role-name "my-maintenance-window-role" ^
       --policy-arn "arn:aws:iam::aws:policy/service-role/AmazonSSMMaintenanceWindowRole"
   ```

------

## Task 2: Configure permissions for users who are allowed to register maintenance window tasks \(AWS CLI\)<a name="sysman-mw-passrole-cli"></a>

When you register a task with a maintenance window, you specify either a custom service role or a Systems Manager service\-linked role to run the actual task operations\. This is the role that the service assumes when it runs tasks on your behalf\. Before that, to register the task itself, you must assign the IAM PassRole policy to an IAM user account or an IAM group\. This allows the IAM user or IAM group to specify, as part of registering those tasks with the maintenance window, the role that should be used when running tasks\. For information, see [Granting a User Permissions to Pass a Role to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html) in the *IAM User Guide*\.

**To configure permissions for users who are allowed to register maintenance window tasks \(AWS CLI\)**

1. Copy and paste the following IAM policy into a text editor and save it with the following name and file extension: `mw-passrole-policy.json`\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "iam:PassRole",
               "Resource": "custom-role-arn"
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

   Replace *custom\-role\-arn* with the ARN of the custom maintenance window role you created earlier, such as `arn:aws:iam::123456789012:role/my-maintenance-window-role`\.

   Replace *account\-id* in the two `iam:ListRoles` permissions with the ID of your AWS account\. Adding this permission for the resource `arn:aws:iam::account-id:role/` allows users in the group to view and choose from customer roles in the console when they create a maintenance window task\. Adding this permission for `arn:aws:iam::account-id:role/aws-service-role/ssm.amazonaws.com/` allows users in the group to choose the Systems Manager service\-linked role in the console when they create a maintenance window task\. 

1. Open the AWS CLI\.

1. Depending on whether you are assigning the permission to an IAM user or group, run one of the following commands\.
   + **For an IAM user:**

------
#### [ Linux ]

     ```
     aws iam put-user-policy \
         --user-name "user-name" \
         --policy-name "policy-name" \
         --policy-document file://path-to-document
     ```

------
#### [ Windows ]

     ```
     aws iam put-user-policy ^
         --user-name "user-name" ^
         --policy-name "policy-name" ^
         --policy-document file://path-to-document
     ```

------

     For *user\-name*, specify the IAM user who assigns tasks to maintenance windows\. For *policy\-name*, specify the name you want to use to identify the policy, such as **my\-iam\-passrole\-policy**\. For *path\-to\-document*, specify the path to the file you saved in step 1\. For example: `file://C:\Temp\mw-passrole-policy.json`
**Note**  
To grant access for a user to register tasks for maintenance windows using the AWS Systems Manager console, you must also assign the `AmazonSSMFullAccess` policy to your user account \(or an IAM policy that provides a smaller set of access permissions for Systems Manager that covers maintenance window tasks\. For more information, see [Create user groups](setup-create-users-nonadmin-groups.md) and [Create users and assign permissions](setup-create-users-nonadmin-users.md)\. Run the following command to assign the `AmazonSSMFullAccess` policy to your account\.  

     ```
     aws iam attach-user-policy \
         --policy-arn "arn:aws:iam::aws:policy/AmazonSSMFullAccess" \
         --user-name "user-name"
     ```

     ```
     aws iam attach-user-policy ^
         --policy-arn "arn:aws:iam::aws:policy/AmazonSSMFullAccess" ^
         --user-name "user-name"
     ```
   + **For an IAM group:**

------
#### [ Linux ]

     ```
     aws iam put-group-policy \
         --group-name "group-name" \
         --policy-name "policy-name" \
         --policy-document file://path-to-document
     ```

------
#### [ Windows ]

     ```
     aws iam put-group-policy ^
         --group-name "group-name" ^
         --policy-name "policy-name" ^
         --policy-document file://path-to-document
     ```

------

     For *group\-name*, specify the IAM group whose members assign tasks to maintenance windows\. For *policy\-name*, specify the name you want to use to identify the policy, such as **my\-iam\-passrole\-policy**\. For *path\-to\-document*, specify the path to the file you saved in step 1\. For example: `file://C:\Temp\mw-passrole-policy.json`
**Note**  
To grant access for members of a group to register tasks for maintenance windows using the AWS Systems Manager console, you must also assign the `AmazonSSMFullAccess` policy to your group\. Run the following command to assign this policy to your group\.  

     ```
     aws iam attach-group-policy \
         --policy-arn "arn:aws:iam::aws:policy/AmazonSSMFullAccess" \
         --group-name "group-name"
     ```

     ```
     aws iam attach-group-policy ^
         --policy-arn "arn:aws:iam::aws:policy/AmazonSSMFullAccess" ^
         --group-name "group-name"
     ```

1. Run the following command to verify that the policy has been assigned to the group\.

------
#### [ Linux ]

   ```
   aws iam list-group-policies \
       --group-name "group-name"
   ```

------
#### [ Windows ]

   ```
   aws iam list-group-policies ^
       --group-name "group-name"
   ```

------

## Task 3: Configure permissions for users who are not allowed to register maintenance window tasks \(AWS CLI\)<a name="mw-deny-task-registrations-cli"></a>

1. Copy and paste the following IAM policy into a text editor and save it with the following name and file extension: `deny-mw-tasks-policy.json`\.

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

1. Open the AWS CLI\.

1. Depending on whether you are assigning the permission to an IAM user or group, run one of the following commands\.
   + **For an IAM user:**

------
#### [ Linux ]

     ```
     aws iam put-user-policy \
         --user-name "user-name" \
         --policy-name "policy-name" \
         --policy-document file://path-to-document
     ```

------
#### [ Windows ]

     ```
     aws iam put-user-policy ^
         --user-name "user-name" ^
         --policy-name "policy-name" ^
         --policy-document file://path-to-document
     ```

------

     For *user\-name*, specify the IAM user to prevent from assigning tasks to maintenance windows\. For *policy\-name*, specify the name you want to use to identify the policy, such as **my\-deny\-mw\-tasks\-policy**\. For *path\-to\-document*, specify the path to the file you saved in step 1\. For example: `file://C:\Temp\deny-mw-tasks-policy.json`
   + **For an IAM group:**

------
#### [ Linux ]

     ```
     aws iam put-group-policy \
         --group-name "group-name" \
         --policy-name "policy-name" \
         --policy-document file://path-to-document
     ```

------
#### [ Windows ]

     ```
     aws iam put-group-policy ^
         --group-name "group-name" ^
         --policy-name "policy-name" ^
         --policy-document file://path-to-document
     ```

------

     For *group\-name*, specify the IAM group whose to prevent from assigning tasks to maintenance windows\. For *policy\-name*, specify the name you want to use to identify the policy, such as **my\-deny\-mw\-tasks\-policy**\. For *path\-to\-document*, specify the path to the file you saved in step 1\. For example: `file://C:\Temp\deny-mw-tasks-policy.json`

1. Run the following command to verify that the policy has been assigned to the group\.

------
#### [ Linux ]

   ```
   aws iam list-group-policies \
       --group-name "group-name"
   ```

------
#### [ Windows ]

   ```
   aws iam list-group-policies ^
       --group-name "group-name"
   ```

------