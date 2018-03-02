# Controlling Access to Maintenance Windows<a name="sysman-maintenance-permissions"></a>

Use one of the following methods to control access to Maintenance Windows by configuring security roles and permissions\. 


+ [Controlling Access to Maintenance Windows \(AWS Console\)](#sysman-maintenance-perm-console)
+ [Controlling Access to Maintenance Windows \(AWS CLI\)](#sysman-maintenance-perm-cli)
+ [Controlling Access to Maintenance Windows \(Tools for Windows PowerShell\)](#sysman-maintenance-perm-ps)

## Controlling Access to Maintenance Windows \(AWS Console\)<a name="sysman-maintenance-perm-console"></a>

The following procedures describe how to create the required roles and permissions for Maintenance Windows by using the AWS console\.

### Create an IAM Role for Systems Manager<a name="sysman-maintenance-role"></a>

Use the following procedure to create a role so that Systems Manager can run tasks in Maintenance Windows on your behalf\.

**To create an IAM role for Maintenance Windows**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**, and then choose **Create role**\.

1. Under** Select type of trusted entity**, choose **AWS service**\. Under **Choose the service that will use this role**, choose **EC2**\. Under **Select your use case**, choose **EC2**, and then choose **Next: Permissions**\.

1. In the list of policies, select the box next to **AmazonSSMMaintenanceWindowRole**, and then choose **Next: Review**\.

1. In **Role name**, enter a name that identifies this role as a Maintenance Windows role\.

1. Choose **Create role**\. The system returns you to the **Roles** page\.

1. Choose the name of the role you just created\.

1. Choose the **Trust relationships** tab, and then choose **Edit trust relationship**\.

1. Delete the current policy, and then copy and paste the following policy into the **Policy Document** field:

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
            "Sid":"",
            "Effect":"Allow",
            "Principal":{
               "Service":[
                  "ec2.amazonaws.com",
                  "ssm.amazonaws.com",
                  "sns.amazonaws.com"
               ]
            },
            "Action":"sts:AssumeRole"
         }
      ]
   }
   ```
**Note**  
`"sns.amazonaws.com"` is required only if you will use Amazon SNS to send notifications related to Maintenance Window tasks run through Run Command\. See step 11 below for more information\.

1. Choose **Update Trust Policy**, and then copy or make a note of the role name and the **Role ARN** value on the **Summary** page\. You will specify this information when you create your Maintenance Window\.

1. If you will configure a Maintenance Window to send notifications about command statuses using Amazon SNS, when run through a Run Command command task, do the following:

   1. Choose the **Permissions** tab\.

   1. Choose **Add inline policy**, and then choose the **JSON** tab\.

   1. In **Policy Document**, paste the following:

      ```
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "sns-access-role-arn"
          }
        ]
      }
      ```

      *sns\-access\-role\-arn* represents the ARN of the IAM role to be for sending SNS notifications related to the Maintenance Window, in the format of `arn:aws:iam::account-id:role/role-name.` For example: `arn:aws:iam::111222333444:role/SNS-Access-role`\. 
**Note**  
In the Systems Manager console, this ARN is selected in the **IAM Role** list on the **Register run command task** page\. For information, see [Assign Tasks to a Maintenance Window](sysman-maintenance-working.md#sysman-maintenance-assign-tasks)\. In the Systems Manager API, this ARN is entered as the value of [ServiceRoleArn](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendCommand.html#EC2-SendCommand-request-ServiceRoleArn) in the [SendCommand](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendCommand.html) request\.

   1. Choose **Review policy**\.

   1. In the **Name** box, type a name to identify this as a policy to allow sending Amazon SNS notifications\.

1. Choose **Create policy**\.

### Assign the IAM PassRole Policy to an IAM User Account<a name="sysman-maintenance-passrole"></a>

When you register a task with a Maintenance Window, you specify the role you created in the previous procedure\. This is the role that the service will assume when it runs tasks on your behalf\. In order to register the task, you must assign the IAM [PassRole](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_passrole.html) policy to your IAM user account\. The policy in the following procedure provides the minimum permissions required to register tasks with a Maintenance Window\.

**To assign the IAM PassRole policy to an IAM user account**

1. In the IAM console navigation pane, choose **Users**, and then choose the name of the user account you want to update\.

1. On the **Permissions** tabs, in the policies list, verify that the **AmazonSSMFullAccess** policy is listed, or that there is a comparable policy that gives the IAM user permission to call the Systems Manager API\.

1. Choose **Add inline policy**\.

1. On the **Create policy** page, in the **Select a service** area, choose **Choose a service**, and then choose **IAM**\.

1. Choose **Select actions**, and then choose **PassRole**\.
**Tip**  
Type **`passr`**`` in the filter box to quickly locate **PassRole**\.

1. Choose the **Resources** line, and then choose **Add ARN**\.

1. In the **Specify ARN for role** field, paste the role ARN you created in the previous procedure, and then choose **Save changes**\.

1. Choose **Review policy**\.

1. On the **Review Policy** page, type a name in the **Name** box, and then choose **Create policy**\.

## Controlling Access to Maintenance Windows \(AWS CLI\)<a name="sysman-maintenance-perm-cli"></a>

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

### Assign the IAM PassRole Policy to an IAM User Account \(AWS CLI\)<a name="sysman-mw-passrole-cli"></a>

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

## Controlling Access to Maintenance Windows \(Tools for Windows PowerShell\)<a name="sysman-maintenance-perm-ps"></a>

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

### Assign the IAM PassRole Policy to an IAM User Account \(Tools for Windows PowerShell\)<a name="sysman-mw-passrole-ps"></a>

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