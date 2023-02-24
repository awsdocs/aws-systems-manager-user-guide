# Default Host Management Configuration<a name="managed-instances-default-host-management"></a>

Default Host Management Configuration allows Systems Manager to manage your Amazon EC2 instances automatically\. After you've turned on this setting, all instances using Instance Metadata Service Version 2 \(IMDSv2\) in the AWS Region and AWS account with SSM Agent version 3\.2\.582\.0 or later installed automatically become managed instances\. Default Host Management Configuration doesn't support Instance Metadata Service Version 1\. For information about transitioning to IMDSv2, see [Transition to using Instance Metadata Service Version 2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/configuring-instance-metadata-service.html#instance-metadata-transition-to-version-2) in the *Amazon EC2 User Guide for Linux Instances*\. For information about checking the version of the SSM Agent installed on your instance, see [Checking the SSM Agent version number](ssm-agent-get-version.md)\. For information about updating the SSM Agent, see [Automatically updating SSM Agent](ssm-agent-automatic-updates.md#ssm-agent-automatic-updates-console)\. Benefits of managed instances include the following:
+ Connect to your instances securely using Session Manager\.
+ Perform automated patch scans using Patch Manager\.
+ View detailed information about your instances using Systems Manager Inventory\.
+ Track and manage instances using Fleet Manager\.
+ Keep the SSM Agent up to date automatically\.

Fleet Manager, Inventory, Patch Manager, and Session Manager are capabilities of AWS Systems Manager\.

Default Host Management Configuration allows instance management without the use of instance profiles and ensures that Systems Manager has permissions to manage all instances in the Region and account\. If the permissions provided aren't sufficient for your use case, you can also add policies to the default IAM role created by the Default Host Management Configuration\. Alternatively, if you don't need permissions for all of the capabilities provided by the default IAM role, you can create your own custom role and policies\. Any changes made to the IAM role you choose for Default Host Management Configuration applies to all managed Amazon EC2 instances in the Region and account\. For more information about the policy used by Default Host Management Configuration, see [AWS managed policy: `AmazonSSMManagedEC2InstanceDefaultPolicy`](security-iam-awsmanpol.md#security-iam-awsmanpol-AmazonSSMManagedEC2InstanceDefaultPolicy)\.

**Note**  
This procedure is intended to be performed only by administrators\. Implement least privilege access when allowing individuals to configure or modify the Default Host Management Configuration\. You can use the following example policies to restrict access to the Default Host Management Configuration\. Replace each *example resource placeholder* with your own information\.

## Service control policy for AWS Organizations<a name="scp-organizations"></a>

```
{
    "Version":"2012-10-17",
    "Statement":[
        {
            "Effect":"Deny",
            "Action":[
                "ssm:UpdateServiceSetting",
                "ssm:ResetServiceSetting"
            ],
            "Resource":"arn:aws:ssm:*:*:servicesetting/ssm/managed-instance/default-ec2-instance-management-role",
            "Condition":{
                "StringNotEqualsIgnoreCase":{
                "aws:PrincipalTag/job-function":[
                    "administrator"
                ]
                }
            }
        },
        {
            "Effect":"Deny",
            "Action":[
                "iam:PassRole"
            ],
            "Resource":"arn:aws:iam::*:role/service-role/AWSSystemsManagerDefaultEC2InstanceManagementRole",
            "Condition":{
                "StringEquals":{
                "iam:PassedToService":"ssm.amazonaws.com"
                },
                "StringNotEqualsIgnoreCase":{
                "aws:PrincipalTag/job-function":[
                    "administrator"
                ]
                }
            }
        },
        {
            "Effect":"Deny",
            "Resource":"arn:aws:iam::*:role/service-role/AWSSystemsManagerDefaultEC2InstanceManagementRole",
            "Action":[
                "iam:AttachRolePolicy",
                "iam:DeleteRole"
            ],
            "Condition":{
                "StringNotEqualsIgnoreCase":{
                "aws:PrincipalTag/job-function":[
                    "administrator"
                ]
                }
            }
        }
    ]
}
```

## Policy for IAM principals<a name="iam-principals-policy"></a>

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "ssm:UpdateServiceSetting",
                "ssm:ResetServiceSetting"
            ],
            "Resource": "arn:aws:ssm:region:account-id:servicesetting/ssm/managed-instance/default-instance-management-role"
        },
        {
            "Effect": "Deny",
            "Action": [
                "iam:AttachRolePolicy",
                "iam:DeleteRole",
                "iam:PassRole"
            ],
            "Resource": "arn:aws:iam::account-id:role/service-role/AWSSystemsManagerDefaultEC2InstanceManagementRole"
        }
    ]
}
```

**To turn on the Default Host Management Configuration setting**  
You can turn on the Default Host Management Configuration from the Fleet Manager console\. To successfully complete this procedure using either the AWS Management Console or your preferred command line tool, you must have permissions for the [GetServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetServiceSetting.html), [ResetServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ResetServiceSetting.html), and [UpdateServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateServiceSetting.html) API operations\. Additionally, you must have permissions for the `iam:PassRole` permission for the `AWSSystemsManagerDefaultEC2InstanceManagementRole` IAM role\. The following is an example policy\. Replace each *example resource placeholder* with your own information\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetServiceSetting",
                "ssm:ResetServiceSetting",
                "ssm:UpdateServiceSetting"
            ],
            "Resource": "arn:aws:ssm:region:account-id:servicesetting/ssm/managed-instance/default-instance-management-role"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": "arn:aws:iam::account-id:role/service-role/AWSSystemsManagerDefaultEC2InstanceManagementRole",
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

Before you begin, if you have instance profiles attached to your Amazon EC2 instances, remove any permissions that allow the `ssm:UpdateInstanceInformation` operation\. The SSM Agent attempts to use instance profile permissions before using the Default Host Management Configuration permissions\. If you allow the `ssm:UpdateInstanceInformation` operation in your instance profiles, the instance will not use the Default Host Management Configuration permissions\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose **Default Host Management Configuration** under the **Account management** dropdown\.

1. Turn on **Enable Default Host Management Configuration**\.

1. Choose the AWS Identity and Access Management \(IAM\) role used to enable Systems Manager capabilities for your instances\. We recommend using the default role provided by Default Host Management Configuration\. It contains the minimum set of permissions necessary to manage your Amazon EC2 instances using Systems Manager\. If you prefer to use a custom role, the role's trust policy must allow Systems Manager as a trusted entity\. 

1. Choose **Configure** to complete setup\. 

After turning on the Default Host Management Configuration, it might take up 30 minutes for your instances to use the credentials of the role you chose\. You must turn on the Default Host Management Configuration in each Region you wish to automatically manage your Amazon EC2 instances\.

**To turn off Default Host Management Configuration**
**Note**  
If you turn off Default Host Management Configuration, and you have not attached an instance profile to your Amazon EC2 instances that allows access to Systems Manager, they will no longer be managed by Systems Manager\. You must turn off the Default Host Management Configuration setting in each Region you wish to no longer automatically manage your Amazon EC2 instances\. Disabling it in one Region doesn't disable it in all Regions\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose **Default Host Management Configuration** under the **Account management** dropdown\.

1. Turn off **Enable Default Host Management Configuration**\.

1. Choose **Configure** to disable Default Host Management Configuration\.

## Turn on Default Host Management Configuration \(command line\)<a name="managed-instances-default-host-management-cli"></a>

The following procedure shows you how to use the AWS Command Line Interface or AWS Tools for Windows PowerShell to turn on the Default Host Management Configuration\.

**To turn on the Default Host Management Configuration using the command line**

1. Create a JSON file on your local machine containing the following trust relationship policy\.

   ```
   {
       "Version":"2012-10-17",
       "Statement":[
           {
               "Sid":"",
               "Effect":"Allow",
               "Principal":{
                   "Service":"ssm.amazonaws.com"
               },
               "Action":"sts:AssumeRole"
           }
       ]
   }
   ```

1. Open the AWS CLI or Tools for Windows PowerShell and run the following command to create a service role in your account\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws iam create-role \
   --role-name AWSSystemsManagerDefaultEC2InstanceManagementRole \
   --assume-role-policy-document file://trust-policy.json
   ```

------
#### [ Windows ]

   ```
   aws iam create-role ^
   --role-name AWSSystemsManagerDefaultEC2InstanceManagementRole ^
   --assume-role-policy-document file://trust-policy.json
   ```

------
#### [ PowerShell ]

   ```
   New-IAMRole `
   -RoleName "AWSSystemsManagerDefaultEC2InstanceManagementRole" `
   -AssumeRolePolicyDocument "file://trust-policy.json"
   ```

------

1. Run the following command to attach the `AmazonSSMManagedEC2InstanceDefaultPolicy` managed policy to your newly created role\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws iam attach-role-policy \
   --policy-arn arn:aws:iam::aws:policy/AmazonSSMManagedEC2InstanceDefaultPolicy \
   --role-name AWSSystemsManagerDefaultEC2InstanceManagementRole
   ```

------
#### [ Windows ]

   ```
   aws iam attach-role-policy ^
   --policy-arn arn:aws:iam::aws:policy/AmazonSSMManagedEC2InstanceDefaultPolicy ^
   --role-name AWSSystemsManagerDefaultEC2InstanceManagementRole
   ```

------
#### [ PowerShell ]

   ```
   Register-IAMRolePolicy `
   -PolicyArn "arn:aws:iam::aws:policy/AmazonSSMManagedEC2InstanceDefaultPolicy" `
   -RoleName "AWSSystemsManagerDefaultEC2InstanceManagementRole"
   ```

------

1. Open the AWS CLI or Tools for Windows PowerShell and run the following command\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm update-service-setting \
   --setting-id arn:aws:ssm:region:account-id:servicesetting/ssm/managed-instance/default-ec2-instance-management-role \
   --setting-value service-role/AWSSystemsManagerDefaultEC2InstanceManagementRole
   ```

------
#### [ Windows ]

   ```
   aws ssm update-service-setting ^
   --setting-id arn:aws:ssm:region:account-id:servicesetting/ssm/managed-instance/default-ec2-instance-management-role ^
   --setting-value service-role/AWSSystemsManagerDefaultEC2InstanceManagementRole
   ```

------
#### [ PowerShell ]

   ```
   Update-SSMServiceSetting `
   -SettingId "arn:aws:ssm:region:account-id:servicesetting/ssm/managed-instance/default-ec2-instance-management-role" `
   -SettingValue "service-role/AWSSystemsManagerDefaultEC2InstanceManagementRole"
   ```

------

   There is no output if the command succeeds\.

1. Run the following command to view the current service settings for Default Host Management Configuration in the current AWS account and AWS Region\.

------
#### [ Linux & macOS ]

   ```
   aws ssm get-service-setting \
   --setting-id arn:aws:ssm:region:account-id:servicesetting/ssm/managed-instance/default-ec2-instance-management-role
   ```

------
#### [ Windows ]

   ```
   aws ssm get-service-setting ^
   --setting-id arn:aws:ssm:region:account-id:servicesetting/ssm/managed-instance/default-ec2-instance-management-role
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMServiceSetting `
   -SettingId "arn:aws:ssm:region:account-id:servicesetting/ssm/managed-instance/default-ec2-instance-management-role"
   ```

------

   The command returns information like the following\.

   ```
   { 
   
   "ServiceSetting": { 
   
       "SettingId": "/ssm/managed-instance/default-ec2-instance-management-role", 
   
       "SettingValue": "service-role/AWSSystemsManagerDefaultEC2InstanceManagementRole", 
   
       "LastModifiedDate": "2022-11-28T08:21:03.576000-08:00", 
   
       "LastModifiedUser": "System", 
   
       "ARN": "arn:aws:ssm:us-west-2:111122223333:servicesetting/ssm/managed-instance/default-ec2-instance-management-role", 
   
       "Status": "Default" 
   
   } 
   
   }
   ```

**To turn off the Default Host Management Configuration using the command line**
+ Open the AWS CLI or Tools for Windows PowerShell and run the following command\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

  ```
  aws ssm reset-service-setting \
  --setting-id arn:aws:ssm:region:account-id:servicesetting/ssm/managed-instance/default-ec2-instance-management-role \
  ```

------
#### [ Windows ]

  ```
  aws ssm reset-service-setting ^
  --setting-id arn:aws:ssm:region:account-id:servicesetting/ssm/managed-instance/default-ec2-instance-management-role ^
  ```

------
#### [ PowerShell ]

  ```
  Reset-SSMServiceSetting `
  -SettingId "arn:aws:ssm:region:account-id:servicesetting/ssm/managed-instance/default-ec2-instance-management-role" `
  ```

------