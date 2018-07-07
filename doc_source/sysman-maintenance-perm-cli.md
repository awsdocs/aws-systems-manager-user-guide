# Control Access to Maintenance Windows \(AWS CLI\)<a name="sysman-maintenance-perm-cli"></a>

The following procedures describe how to use the AWS CLI to create the required roles and permissions for Maintenance Windows\.

**Topics**
+ [Task 1: Create a Custom Service Role for Maintenance Windows](#sysman-maintenance-role-cli)
+ [Task 2: Assign the IAM PassRole Policy to an IAM User or Group](#sysman-mw-passrole-cli)

## Task 1: Create a Custom Service Role for Maintenance Windows<a name="sysman-maintenance-role-cli"></a>

1. Copy and paste the following trust policy into a text file\. Save the file with the following name and file extension: `mw-role-trust-policy.json`\.
**Note**  
`"sns.amazonaws.com"` is required only if you will use Amazon SNS to send notifications related to Maintenance Window tasks run through the [SendCommand](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendCommand.html) API or `send-command` in the AWS CLI\.

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

1. Open the AWS CLI and run the following command in the directory where you placed `mw-role-trust-policy.json` in order to create a Maintenance Window role called `mw-task-role`\. The command assigns the policy you created in the previous step to this role\.

   ```
   aws iam create-role --role-name mw-task-role --assume-role-policy-document file://mw-role-trust-policy.json
   ```

   The system returns information like the following\.

   ```
   {
      "Role":{
         "AssumeRolePolicyDocument":{
            "Version":"2012-10-17",
            "Statement":[
               {
                  "Action":"sts:AssumeRole",
                  "Effect":"Allow",
                  "Principal":{
                     "Service":[
                        "ssm.amazonaws.com",
                        "ec2.amazonaws.com",
                        "sns.amazonaws.com"
                     ]
                  }
               }
            ]
         },
         "RoleId":"AROAIIZKPBKS2LEXAMPLE",
         "CreateDate":"2017-04-04T03:40:17.373Z",
         "RoleName":"mw-task-role",
         "Path":"/",
         "Arn":"arn:aws:iam::123456789012:role/mw-task-role"
      }
   }
   ```
**Note**  
Make a note of the `RoleName` and the `Arn`\. You will specify these when you create a Maintenance Window\.

1. Run the following command to attach the `AmazonSSMMaintenanceWindowRole` managed policy to the role you created in step 2\.

   ```
   aws iam attach-role-policy --role-name mw-task-role --policy-arn arn:aws:iam::aws:policy/service-role/AmazonSSMMaintenanceWindowRole
   ```

## Task 2: Assign the IAM PassRole Policy to an IAM User or Group<a name="sysman-mw-passrole-cli"></a>

When you register a task with a Maintenance Window, you specify a custom service role to run the actual task operations\. This is the role that the service will assume when it runs tasks on your behalf\. Before that, to register the task itself, you must assign the IAM [PassRole](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html) policy to an IAM user account or an IAM group\. This allows the IAM user or IAM group to specify, as part of registering those tasks with the Maintenance Window, the role that should be used when running tasks\.

**To assign the IAM PassRole policy to an IAM user account or group**

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

     For *user\-name*, specify the IAM user who will assign tasks to Maintenance Windows\. For *policy\-name*, specify the name you want to use to identify the policy\. For *path\-to\-document*, specify the path to the file you saved in step 1\. For example: `file://C:\Temp\mw-passrole-policy.json`
**Note**  
If you plan to register tasks for Maintenance Windows using the AWS Systems Manager console, you must also assign the `AmazonSSMFullAccess` policy to your user account\. Run the following command to assign this policy to your account\.  

     ```
     aws iam attach-user-policy --policy-arn arn:aws:iam::aws:policy/AmazonSSMFullAccess --user-name user-name
     ```
   + **For an IAM group:**

     ```
     aws iam put-group-policy --group-name group-name --policy-name "policy-name" --policy-document path-to-document
     ```

     For *group\-name*, specify the IAM group whose members will assign tasks to Maintenance Windows\. For *policy\-name*, specify the name you want to use to identify the policy\. For *path\-to\-document*, specify the path to the file you saved in step 1\. For example: `file://C:\Temp\mw-passrole-policy.json`
**Note**  
If you plan to register tasks for Maintenance Windows using the AWS Systems Manager console, you must also assign the `AmazonSSMFullAccess` policy to your group\. Run the following command to assign this policy to your group\.  

     ```
     aws iam attach-group-policy --policy-arn arn:aws:iam::aws:policy/AmazonSSMFullAccess --group-name group-name
     ```

1. Run the following command to verify that the policy has been assigned to the group\.

   ```
   aws iam list-group-policies --group-name group-name
   ```