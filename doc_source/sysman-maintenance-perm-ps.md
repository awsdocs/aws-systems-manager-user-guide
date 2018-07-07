# Control Access to Maintenance Windows \(Tools for Windows PowerShell\)<a name="sysman-maintenance-perm-ps"></a>

The following procedures describe how to use the Tools for Windows PowerShell to create the required roles and permissions for Maintenance Windows\.

**Topics**
+ [Task 1: Create a Custom Service Role for Maintenance Windows](#sysman-maintenance-role-ps)
+ [Task 2: Assign the IAM PassRole Policy to an IAM User or Group](#sysman-mw-passrole-ps)

## Task 1: Create a Custom Service Role for Maintenance Windows<a name="sysman-maintenance-role-ps"></a>

****

1. Copy and paste the following trust policy into a text file\. Save the file with the following name and file extension: `mw-role-trust-policy.json`\.
**Note**  
`"sns.amazonaws.com"` is required only if you will use Amazon SNS to send notifications related to Maintenance Window tasks run through the [SendCommand](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendCommand.html) API\.

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
            "Effect":"Allow",
            "Principal":{
               "Service":[
                  "ssm.amazonaws.com",
                  "sns.amazonaws.com"
               ]
            },
            "Action":"sts:AssumeRole"
         }
      ]
   }
   ```

1. Open Tools for Windows PowerShell and run the following command to create a role with a name that identifies this role as a Maintenance Window role; for example `my-maintenance-window-role`\. The role uses the policy that you created in the previous step\.

   ```
   New-IAMRole -RoleName "mw-task-role" -AssumeRolePolicyDocument (Get-Content -raw .\mw-role-trust-policy.json)
   ```

   The systems returns information like the following\.

   ```
   Arn : arn:aws:iam::123456789012:role/mw-task-role
   AssumeRolePolicyDocument : ExampleDoc12345678
   CreateDate : 4/4/2017 11:24:43
   Path : /
   RoleId : AROAIIZKPBKS2LEXAMPLE
   RoleName : mw-task-role
   ```

1. Run the following command to attach the `AmazonSSMMaintenanceWindowRole` managed policy to the role you created in the previous step\.

   ```
   Register-IAMRolePolicy -RoleName mw-task-role -PolicyArn
                   arn:aws:iam::aws:policy/service-role/AmazonSSMMaintenanceWindowRole
   ```

## Task 2: Assign the IAM PassRole Policy to an IAM User or Group<a name="sysman-mw-passrole-ps"></a>

When you register a task with a Maintenance Window, you specify a custom service role to run the actual task operations\. This is the role that the service will assume when it runs tasks on your behalf\. Before that, to register the task itself, you must assign the IAM [PassRole](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html) policy to an IAM user account or an IAM group\. This allows the IAM user or IAM group to specify, as part of registering those tasks with the Maintenance Window, the role that should be used when running tasks\.

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

     For *user\-name*, specify the IAM user who will assign tasks to Maintenance Windows\. For *policy\-name*, specify the name you want to use to identify the policy\. For *path\-to\-document*, specify the path to the file you saved in step 1\. For example: `C:\temp\passrole-policy.json`
**Note**  
If you plan to register tasks for Maintenance Windows using the AWS Systems Manager console, you must also assign the `AmazonSSMFullAccess` policy to your user account\. Run the following command to assign this policy to your account\.  

     ```
     Register-IAMUserPolicy -UserName user-name -PolicyArn arn:aws:iam::aws:policy/AmazonSSMFullAccess
     ```
   + **For an IAM group**:

     ```
     Write-IAMGroupPolicy -GroupName group-name -PolicyDocument (Get-Content -raw path-to-document) -PolicyName policy-name
     ```

     For *group\-name*, specify the IAM group that will assign tasks to Maintenance Windows\. For *policy\-name*, specify the name you want to use to identify the policy\. For *path\-to\-document*, specify the path to the file you saved in step 1\. For example: `C:\temp\passrole-policy.json`
**Note**  
If you plan to register tasks for Maintenance Windows using the AWS Systems Manager console, you must also assign the `AmazonSSMFullAccess` policy to your user account\. Run the following command to assign this policy to your group\.  

   ```
   Register-IAMGroupPolicy -GroupName group-name -PolicyArn arn:aws:iam::aws:policy/AmazonSSMFullAccess
   ```

1. Run the following command to verify that the policy has been assigned to the group\.

   ```
   Get-IAMGroupPolicies -GroupName group-name
   ```