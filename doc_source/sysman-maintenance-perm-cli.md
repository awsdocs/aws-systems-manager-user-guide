# Control Access to Maintenance Windows \(AWS CLI\)<a name="sysman-maintenance-perm-cli"></a>

The following procedures describe how to use the AWS CLI to create the required roles and permissions for Maintenance Windows\.

**Topics**
+ [Task 1: \(Optional\) Create a Custom Service Role for Maintenance Windows \(AWS CLI\)](#sysman-maintenance-role-cli)
+ [Task 2: Assign the IAM PassRole Policy to an IAM User or Group](#sysman-mw-passrole-cli)

## Task 1: \(Optional\) Create a Custom Service Role for Maintenance Windows \(AWS CLI\)<a name="sysman-maintenance-role-cli"></a>

**Important**  
A custom service role is not required if you choose to use a Systems Manager service\-linked role to let maintenance windows run tasks on your behalf instead\. If you do not have a Systems Manager service\-linked role in your account, you can create it when you create or update a maintenance window task using the Systems Manager console\. For more information, see the following topics:  
[Should I Use a Service\-Linked Role or a Custom Service Role to Run Maintenance Window Tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)
[Using Service\-Linked Roles for Systems Manager](using-service-linked-roles.md)
[Assign Tasks to a Maintenance Window \(Console\)](sysman-maintenance-assign-tasks.md)

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

1. \(Optional\) Modify the policy to restrict access or provide additional permissions as needed\. 

   For information about the types of customizations you might choose to make, see [Should I Use a Service\-Linked Role or a Custom Service Role to Run Maintenance Window Tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)\.

1. Open the AWS CLI and run the following command in the directory where you placed `mw-role-trust-policy.json` in order to create a maintenance window role called `mw-task-role`\. The command assigns the policy you created in the previous step to this role:

   ```
   aws iam create-role --role-name mw-task-role --assume-role-policy-document file://mw-role-trust-policy.json
   ```

   The system returns information like the following:

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
           "RoleName": "mw-task-role",
           "Path": "/",
           "Arn": "arn:aws:iam::123456789012:role/mw-task-role"
       }
   }
   ```
**Note**  
Make a note of the `RoleName` and the `Arn`\. You specify these when you create a maintenance window\.

1. Run the following command to attach the `AmazonSSMMaintenanceWindowRole` managed policy to the role you created in step 2:

   ```
   aws iam attach-role-policy --role-name mw-task-role --policy-arn arn:aws:iam::aws:policy/service-role/AmazonSSMMaintenanceWindowRole
   ```

## Task 2: Assign the IAM PassRole Policy to an IAM User or Group<a name="sysman-mw-passrole-cli"></a>

When you register a task with a maintenance window, you specify either a custom service role or a Systems Manager service\-linked role to run the actual task operations\. This is the role that the service assumes when it runs tasks on your behalf\. Before that, to register the task itself, you must assign the IAM PassRole policy to an IAM user account or an IAM group\. This allows the IAM user or IAM group to specify, as part of registering those tasks with the maintenance window, the role that should be used when running tasks\. For information, see [Granting a User Permissions to Pass a Role to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html) in the *IAM User Guide*\.

**To assign the IAM PassRole policy to an IAM user account or group \(AWS CLI\)**

1. Copy and paste the following IAM policy into a text editor and save it with the following name and file extension: `mw-passrole-policy.json`\.

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
            "Sid":"Stmt1491345526000",
            "Effect":"Allow",
            "Action":[
               "iam:GetRole",
               "iam:PassRole",
               "ssm:RegisterTaskWithMaintenanceWindow"
            ],
            "Resource":[
               "*"
            ]
         }
      ]
   }
   ```

1. Open the AWS CLI\.

1. Depending on whether you are assigning the permission to an IAM user or group, run one of the following commands\.
   + **For an IAM user:**

     ```
     aws iam put-user-policy --user-name user-name --policy-name "policy-name" --policy-document path-to-document
     ```

     For *user\-name*, specify the IAM user who assigns tasks to maintenance windows\. For *policy\-name*, specify the name you want to use to identify the policy\. For *path\-to\-document*, specify the path to the file you saved in step 1\. For example: `file://C:\Temp\mw-passrole-policy.json`
**Note**  
If you plan to register tasks for maintenance windows using the AWS Systems Manager console, you must also assign the `AmazonSSMFullAccess` policy to your user account\. Run the following command to assign this policy to your account:  

     ```
     aws iam attach-user-policy --policy-arn arn:aws:iam::aws:policy/AmazonSSMFullAccess --user-name user-name
     ```
   + **For an IAM group:**

     ```
     aws iam put-group-policy --group-name group-name --policy-name "policy-name" --policy-document path-to-document
     ```

     For *group\-name*, specify the IAM group whose members assign tasks to maintenance windows\. For *policy\-name*, specify the name you want to use to identify the policy\. For *path\-to\-document*, specify the path to the file you saved in step 1\. For example: `file://C:\Temp\mw-passrole-policy.json`
**Note**  
If you plan to register tasks for maintenance windows using the AWS Systems Manager console, you must also assign the `AmazonSSMFullAccess` policy to your group\. Run the following command to assign this policy to your group:  

     ```
     aws iam attach-group-policy --policy-arn arn:aws:iam::aws:policy/AmazonSSMFullAccess --group-name group-name
     ```

1. Run the following command to verify that the policy has been assigned to the group:

   ```
   aws iam list-group-policies --group-name group-name
   ```