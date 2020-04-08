# Control access to maintenance windows \(Tools for Windows PowerShell\)<a name="sysman-maintenance-perm-ps"></a>

The following procedures describe how to use the Tools for Windows PowerShell to create the required roles and permissions for the Maintenance Windows capability\.

**Topics**
+ [Task 1: \(Optional\) create a custom service role for Maintenance Windows \(AWS CLI\)](#sysman-maintenance-role-ps)
+ [Task 2: Assign the IAM PassRole policy to an IAM user or group \(AWS CLI\)](#sysman-mw-passrole-ps)

## Task 1: \(Optional\) create a custom service role for Maintenance Windows \(AWS CLI\)<a name="sysman-maintenance-role-ps"></a>

****
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

1. \(Optional\) Modify the policy to restrict access or provide additional permissions as needed\. 

   For information about the types of customizations you might choose to make, see [Should I use a service\-linked role or a custom service role to run maintenance window tasks?](sysman-maintenance-permissions.md#maintenance-window-tasks-service-role)\.

1. Open Tools for Windows PowerShell and run the following command to create a role with a name that identifies this role as a maintenance window role; for example `my-maintenance-window-role`\. The role uses the policy that you created in the previous step:

   ```
   New-IAMRole -RoleName "mw-task-role" -AssumeRolePolicyDocument (Get-Content -raw .\mw-role-trust-policy.json)
   ```

   The system returns information like the following\.

   ```
   Arn : arn:aws:iam::123456789012:role/mw-task-role
   AssumeRolePolicyDocument : ExampleDoc12345678
   CreateDate : 4/4/2017 11:24:43
   Path : /
   RoleId : AROAIIZKPBKS2LEXAMPLE
   RoleName : mw-task-role
   ```

1. Run the following command to attach the `AmazonSSMMaintenanceWindowRole` managed policy to the role you created in the previous step:

   ```
   Register-IAMRolePolicy -RoleName mw-task-role -PolicyArn
                   arn:aws:iam::aws:policy/service-role/AmazonSSMMaintenanceWindowRole
   ```

## Task 2: Assign the IAM PassRole policy to an IAM user or group \(AWS CLI\)<a name="sysman-mw-passrole-ps"></a>

When you register a task with a maintenance window, you specify either a custom service role or a Systems Manager service\-linked role to run the actual task operations\. This is the role that the service assumes when it runs tasks on your behalf\. Before that, to register the task itself, you must assign the IAM PassRole policy to an IAM user account or an IAM group\. This allows the IAM user or IAM group to specify, as part of registering those tasks with the maintenance window, the role that should be used when running tasks\. For information, see [Granting a User Permissions to Pass a Role to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html) in the *IAM User Guide*\.

1. Copy and paste the following IAM policy into a text editor and save it with the `.json` file extension\.

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

1. Open Tools for Windows PowerShell\.

1. Depending on whether you are assigning the permission to an IAM user or group, run one of the following commands\.
   + **For an IAM user**:

     ```
     Write-IAMUserPolicy -UserName user-name -PolicyDocument (Get-Content -raw path-to-document) -PolicyName policy-name
     ```

     For *user\-name*, specify the IAM user who assigns tasks to maintenance windows\. For *policy\-name*, specify the name you want to use to identify the policy\. For *path\-to\-document*, specify the path to the file you saved in step 1\. For example: `C:\temp\passrole-policy.json`
**Note**  
If you plan to register tasks for maintenance windows using the AWS Systems Manager console, you must also assign the `AmazonSSMFullAccess` policy to your user account\. Run the following command to assign this policy to your account:  

     ```
     Register-IAMUserPolicy -UserName user-name -PolicyArn arn:aws:iam::aws:policy/AmazonSSMFullAccess
     ```
   + **For an IAM group**:

     ```
     Write-IAMGroupPolicy -GroupName group-name -PolicyDocument (Get-Content -raw path-to-document) -PolicyName policy-name
     ```

     For *group\-name*, specify the IAM group that assigns tasks to maintenance windows\. For *policy\-name*, specify the name you want to use to identify the policy\. For *path\-to\-document*, specify the path to the file you saved in step 1\. For example: `C:\temp\passrole-policy.json`
**Note**  
If you plan to register tasks for maintenance windows using the AWS Systems Manager console, you must also assign the `AmazonSSMFullAccess` policy to your user account\. Run the following command to assign this policy to your group:  

   ```
   Register-IAMGroupPolicy -GroupName group-name -PolicyArn arn:aws:iam::aws:policy/AmazonSSMFullAccess
   ```

1. Run the following command to verify that the policy has been assigned to the group:

   ```
   Get-IAMGroupPolicies -GroupName group-name
   ```