# Control Access to Maintenance Windows \(AWS CLI\)<a name="sysman-maintenance-perm-cli"></a>

Use the following procedure to create an IAM role for Maintenance Windows using the AWS CLI\.

**To create an IAM role for Maintenance Windows**

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
                  "ec2.amazonaws.com",
                  "sns.amazonaws.com"
               ]
            },
            "Action":"sts:AssumeRole"
         }
      ]
   }
   ```

1. Open the AWS CLI and run the following command to create a Maintenance Window role called mw\-task\-role\. The command assigns the policy you created in the previous step to this role\.

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

1. Execute the following command to attach the `AmazonSSMMaintenanceWindowRole` managed policy to the role you created in step 2\.

   ```
   aws iam attach-role-policy --role-name mw-task-role --policy-arn arn:aws:iam::aws:policy/service-role/AmazonSSMMaintenanceWindowRole
   ```

## Assign the IAM PassRole Policy to an IAM User Account \(AWS CLI\)<a name="sysman-mw-passrole-cli"></a>

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

1. Open the AWS CLI\.

1. Execute the following command\. For `user-name`, specify the IAM user who will assign tasks to Maintenance Windows\. For `policy-document`, specify the path to the file you saved in step 1\.

   ```
   aws iam put-user-policy --user-name name of user --policy-name "a name for the policy" --policy-document path to document, for example: file://C:\Temp\passrole.json
   ```
**Note**  
If you plan to register tasks for Maintenance Windows using the AWS Systems Manager console, you must also assign the AmazonSSMReadOnlyAccess policy to your user account\. Execute the following command to assign this policy to your account\.  

   ```
   aws iam attach-user-policy --policy-arn arn:aws:iam::aws:policy/AmazonSSMReadOnlyAccess --user-name IAM account name
   ```

1. Execute the following command to verify that the policy has been assigned to the user\.

   ```
   aws iam list-user-policies --user-name name-of-user
   ```