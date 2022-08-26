# Default Host Management Configuration<a name="managed-instances-default-host-management"></a>

Default Host Management Configuration allows Systems Manager to manage your Amazon EC2 instances automatically\. After you've turned on this setting, all instances with the supported SSM Agent version automatically become managed instances\. Benefits of managed instances include the following:
+ Connect to your instances securely using Session Manager\.
+ Perform automated patch scans using Patch Manager\.
+ View detailed information about your instances using Systems Manager Inventory\.
+ Track and manage instances using Fleet Manager\.
+ Keep the SSM Agent up to date automatically\. 

Fleet Manager, Inventory, Patch Manager, and Session Manager are capabilities of AWS Systems Manager\.

Default Host Management Configuration allows instance management without the use of instance profiles and ensures that Systems Manager manages all instances in your account\. If the instance already has an instance profile configured, the SSM Agent attempts to use it before using the Default Host Management Configuration permissions\. For more information about the policy used by Default Host Management Configuration, see [AWS managed policy: `AmazonSSMManagedEC2InstanceDefaultPolicy`](security-iam-awsmanpol.md#security-iam-awsmanpol-AmazonSSMManagedEC2InstanceDefaultPolicy)\.

**To turn on the Default Host Management Configuration setting**  
You can turn on the Default Host Management Configuration from the Fleet Manager console\. This procedure is intended to be performed by administrators\. To successfully complete this procedure using either the AWS Management Console or your preferred command line tool, you must have permissions for the `iam:PassRole` API operation for all resources, or for the `AmazonSSMManagedEC2InstanceDefaultRole` IAM role\.

**Note**  
You must turn on the Default Host Management Configuration in each AWS Region you wish to automatically manage your Amazon EC2 instances\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose **Default Host Management Configuration** under the **Account management** dropdown\.

1. Turn on **Enable Default Host Management Configuration**\.

1. Choose the AWS Identity and Access Management \(IAM\) role used to enable Systems Manager capabilities for your instances\. We recommend using the default role provided by Default Host Management Configuration\. It contains the minimum set of permissions necessary to manage your Amazon EC2 instances using Systems Manager\. 

1. Choose **Configure** to complete setup\. 

**To turn off Default Host Management Configuration**
**Note**  
If you turn off Default Host Management Configuration, and you have not attached an instance profile to your Amazon EC2 instances that allows access to Systems Manager, they will no longer be managed by Systems Manager\. You must turn off the Default Host Management Configuration setting in each Region you wish to no longer automatically manage your Amazon EC2 instances\. Disabling it in one Region doesn't disable it in all Regions\.

1. In the navigation pane, choose **Fleet Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Fleet Manager** in the navigation pane\.

1. Choose **Default Host Management Configuration** under the **Account management** dropdown\.

1. Turn off **Enable Default Host Management Configuration**\.

1. Choose **Configure** to disable Default Host Management Configuration\.

## Turn on Default Host Management Configuration \(command line\)<a name="managed-instances-default-host-management-cli"></a>

The following procedure shows you how to use the AWS Command Line Interface or AWS Tools for Windows PowerShell to turn on the Default Host Management Configuration\. Verify that you have permission in AWS Identity and Access Management \(IAM\) to turn on this setting\. You must either have the `AdministratorAccess` policy attached to your IAM user, group, or role, or you must have permission to change the Default Host Management Configuration service setting\. The Default Host Management Configuration setting uses the following API operations: 
+ [GetServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetServiceSetting.html)
+ [UpdateServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateServiceSetting.html)
+ [ResetServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ResetServiceSetting.html)

**To turn on the Default Host Management Configuration using the command line**

1. Create a JSON file on your local machine containing the following trust relationship policy\.

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

1. Open the AWS CLI or Tools for Windows PowerShell and run the following command\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws iam create-role \
       --role-name AmazonSSMManagedEC2InstanceDefaultRole \
       --assume-role-policy-document file://trust-policy.json
   ```

------
#### [ Windows ]

   ```
   aws iam create-role ^
       --role-name AmazonSSMManagedEC2InstanceDefaultRole ^
       --assume-role-policy-document file://trust-policy.json
   ```

------
#### [ PowerShell ]

   ```
   New-IAMRole `
       -RoleName "AmazonSSMManagedEC2InstanceDefaultRole" `
       -AssumeRolePolicyDocument "file://trust-policy.json"
   ```

------

1. Run the following command to attach the `AmazonSSMManagedEC2InstanceDefaultPolicy` managed policy to your newly created role\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws iam attach-role-policy \
       --policy-arn arn:aws:iam::aws:policy/AmazonSSMManagedEC2InstanceDefaultPolicy \
       --role-name AmazonSSMManagedEC2InstanceDefaultRole
   ```

------
#### [ Windows ]

   ```
   aws iam attach-role-policy ^
       --policy-arn arn:aws:iam::aws:policy/AmazonSSMManagedEC2InstanceDefaultPolicy ^
       --role-name AmazonSSMManagedEC2InstanceDefaultRole
   ```

------
#### [ PowerShell ]

   ```
   Register-IAMRolePolicy `
       -PolicyArn "arn:aws:iam::aws:policy/AmazonSSMManagedEC2InstanceDefaultPolicy" `
       -RoleName "AmazonSSMManagedEC2InstanceDefaultRole"
   ```

------

1. Open the AWS CLI or Tools for Windows PowerShell and run the following command\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm update-service-setting \
       --setting-id arn:aws:ssm:region:account-id:servicesetting/ssm/managed-instance/default-ec2-instance-management-role \
       --setting-value AmazonSSMManagedEC2InstanceDefaultRole
   ```

------
#### [ Windows ]

   ```
   aws ssm update-service-setting ^
       --setting-id arn:aws:ssm:region:account-id:servicesetting/ssm/managed-instance/default-ec2-instance-management-role ^
       --setting-value AmazonSSMManagedEC2InstanceDefaultRole
   ```

------
#### [ PowerShell ]

   ```
   Update-SSMServiceSetting `
       -SettingId "arn:aws:ssm:region:account-id:servicesetting/ssm/managed-instance/default-ec2-instance-management-role" `
       -SettingValue "AmazonSSMManagedEC2InstanceDefaultRole"
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
   
           "SettingValue": "AmazonSSMManagedEC2InstanceDefaultRole", 
   
           "LastModifiedDate": "2022-07-19T08:21:03.576000-08:00", 
   
           "LastModifiedUser": "System", 
   
           "ARN": "arn:aws:ssm:us-west-2:111122223333:servicesetting/ssm/managed-instance/default-ec2-instance-management-role", 
   
           "Status": "Default" 
   
       } 
   
   }
   ```