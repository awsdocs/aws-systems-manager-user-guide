# Control Access to Maintenance Windows \(Tools for Windows PowerShell\)<a name="sysman-maintenance-perm-ps"></a>

Use the following procedure to create an IAM role for Maintenance Windows using the Tools for Windows PowerShell\.

**To create an IAM role for Maintenance Windows**

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
                  "ec2.amazonaws.com",
                  "sns.amazonaws.com"
               ]
            },
            "Action":"sts:AssumeRole"
         }
      ]
   }
   ```

1. Open Tools for Windows PowerShell and run the following command to create a role called mw\-task\-role\. The role uses the policy that you created in the previous step\.

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

1. Execute the following command to attach the `AmazonSSMMaintenanceWindowRole` managed policy to the role you created in the previous step\.

   ```
   Register-IAMRolePolicy -RoleName mw-task-role -PolicyArn
                   arn:aws:iam::aws:policy/service-role/AmazonSSMMaintenanceWindowRole
   ```

## Assign the IAM PassRole Policy to an IAM User Account \(Tools for Windows PowerShell\)<a name="sysman-mw-passrole-ps"></a>

When you register a task with a Maintenance Window, you specify the role you created in the previous procedure\. This is the role that the service will assume when it runs tasks on your behalf\. In order to register the task, you must assign the IAM [PassRole](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html) policy to your IAM user account\. The policy in the following procedure provides the minimum permissions required to register tasks with a Maintenance Window\.

**To assign the IAM PassRole policy to an IAM user account**

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

1. Execute the following command\. For `user-name`, specify the IAM user who will assign tasks to Maintenance Windows\. For `policy-document`, specify the path to the file you saved in step 1\.

   ```
   Write-IAMUserPolicy -UserName name-of-IAM-user -PolicyDocument (Get-Content -raw path to document, for example: C:\temp\passrole-policy.json) -PolicyName a name for the policy
   ```
**Note**  
If you plan to register tasks for Maintenance Windows using the AWS Systems Manager console, you must also assign the AmazonSSMReadOnlyAccess policy to your user account\. Execute the following command to assign this policy to your account\.  

   ```
   Register-IAMUserPolicy -UserName IAM-account-name -PolicyArn arn:aws:iam::aws:policy/AmazonSSMReadOnlyAccess
   ```

1. Execute the following command to verify that the policy has been assigned to the user\.

   ```
   Get-IAMUserPolicies -UserName name-of-user
   ```