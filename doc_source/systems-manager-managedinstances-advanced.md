# Enabling the advanced\-instances tier<a name="systems-manager-managedinstances-advanced"></a>

AWS Systems Manager offers a standard\-instances tier and an advanced\-instances tier for servers and VMs in your hybrid environment\. The standard\-instances tier enables you to register a maximum of 1,000 on\-premises servers or VMs per AWS account per AWS Region\. If you need to register more than 1,000 on\-premises servers or VMs in a single account and Region, then use the advanced\-instances tier\. You can activate as many managed instances in a hybrid environment as you like in the advanced\-instances tier\. However, all instances configured for Systems Manager using the managed\-instance activation process described earlier in [Create a managed\-instance activation for a hybrid environment](sysman-managed-instance-activation.md) are made available on a pay\-per\-use basis\. This also applies to EC2 instances that use a Systems Manager on\-premises activation \(which is not a common scenario\)\.

**Note**  
Advanced instances also enable you to connect to your hybrid machines by using AWS Systems Manager Session Manager\. Session Manager provides interactive shell access to your instances\. For more information, see [AWS Systems Manager Session Manager](session-manager.md)\.
The standard\-instances limit also applies to EC2 instances that use a Systems Manager on\-premises activation \(which is not a common scenario\)\.
Microsoft application patching is only available on EC2 instances and in the advanced\-instances tier\. To patch Microsoft applications on on\-premises servers and VMs, you must enable the advanced\-instances tier\. For more information, see [About patching applications on Windows Server](about-windows-app-patching.md)\.

This section describes how to configure your hybrid environment to use the advanced\-instances tier\.

**Before You Begin**  
Review pricing details for advanced instances\. Advanced instances are an account\-level feature\. Advanced instances are available on a per\-use\-basis\. For more information see, [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)\. 

## Configuring permissions to enable the advanced\-instances tier<a name="systems-manager-managedinstances-advanced-permissions"></a>

Verify that you have permission in AWS Identity and Access Management \(IAM\) to change your environment from the standard\-instances tier to the advanced\-instances tier\. You must either have the AdministratorAccess policy attached to your IAM user, group, or role\. Or, you must have permission to change the Systems Manager activation\-tier service setting\. The activation\-tier setting uses the following API actions: 
+ [GetServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetServiceSetting.html)
+ [UpdateServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateServiceSetting.html)
+ [ResetServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ResetServiceSetting.html)

Use the following procedure to add an inline IAM policy to a user account\. This policy enables a user to view the current managed\-instance tier setting\. This policy also enables the user to change or reset the current setting in the specified AWS account and Region\.

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**\.

1. In the list, choose the name of the user to embed a policy in\.

1. Choose the **Permissions** tab\.

1. On the right side of the page, under **Permission policies**, choose **Add inline policy**\. 

1. Choose the **JSON** tab\.

1. Replace the default content with the following:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "ssm:GetServiceSetting"
                   
               ],
               "Resource": "*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "ssm:ResetServiceSetting",
                   "ssm:UpdateServiceSetting"
               ],
               "Resource": "arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier"
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter a name for the inline policy\. For example: **Managed\-Instances\-Tier**\.

1. Choose **Create policy**\.

Administrators can specify read\-only permission by assigning the following inline policy to the user's account\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetServiceSetting"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": [
                "ssm:ResetServiceSetting",
                "ssm:UpdateServiceSetting"
            ],
            "Resource": "*"
        }
    ]
}
```

For more information about creating and editing IAM policies, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\.

## Enabling the advanced\-instances tier \(console\)<a name="systems-manager-managedinstances-advanced-enabling"></a>

The following procedure shows you how to use the Systems Manager console to change *all* on\-premises servers and VMs that were added using managed\-instance activation, in the specified AWS account and Region, to use the advanced\-instances tier\.

**Important**  
The following procedure describes how to change an account\-level setting\. This change results in charges being billed to your account\.

**To enable the advanced\-instances tier \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Managed instances**\.

1. Choose the **Settings** tab\.

   If you don't see the **Settings** tab, then do the following:

   1. Verify that the console is open in the AWS Region where you created your managed instances\. You can switch Regions by using the list in the top, right corner of the console\. 

   1. Verify that your instances meet Systems Manager requirements\. For information, see [Systems Manager prerequisites](systems-manager-prereqs.md)\.

   1. For servers and VMs in a hybrid environment, verify that you completed the activation process\. For more information, see [Setting up AWS Systems Manager for hybrid environments](systems-manager-managedinstances.md)\.

1. Choose **Change account settings**\.

1. Review the information in the pop\-up about changing account settings, and then, if you approve, choose the option to accept and continue\.

The system can take several minutes to complete the process of moving all instances from the standard\-instances tier to the advanced\-instances tier\.

**Note**  
For information about changing back to the standard\-instances tier, see [Reverting from the advanced\-instances tier to the standard\-instances tier](systems-manager-managed-instances-advanced-reverting.md)\.

## Enabling the advanced\-instances tier \(AWS CLI\)<a name="systems-manager-managedinstances-advanced-enabling-cli"></a>

The following procedure shows you how to use the AWS CLI to change *all* on\-premises servers and VMs that were added using managed\-instance activation, in the specified AWS account and Region, to use the advanced\-instances tier\.

**Important**  
The following procedure describes how to change an account\-level setting\. This change results in charges being billed to your account\.

**To enable the advanced\-instances tier using the AWS CLI**

1. Open the AWS CLI and run the following command\.

------
#### [ Linux ]

   ```
   aws ssm update-service-setting \
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier \
       --setting-value advanced
   ```

------
#### [ Windows ]

   ```
   aws ssm update-service-setting ^
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier ^
       --setting-value advanced
   ```

------

   There is no output if the command succeeds\.

1. Run the following command to view the current service settings for managed instances in the current AWS account and Region\.

------
#### [ Linux ]

   ```
   aws ssm get-service-setting \
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier
   ```

------
#### [ Windows ]

   ```
   aws ssm get-service-setting ^
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier
   ```

------

   The command returns information like the following\.

   ```
   {
       "ServiceSetting": {
           "SettingId": "/ssm/managed-instance/activation-tier",
           "SettingValue": "advanced",
           "LastModifiedDate": 1555603376.138,
           "LastModifiedUser": "arn:aws:sts::123456789012:assumed-role/Administrator/User_1",
           "ARN": "arn:aws:ssm:us-east-2:123456789012:servicesetting/ssm/managed-instance/activation-tier",
           "Status": "PendingUpdate"
       }
   }
   ```

## Enabling the advanced\-instances tier \(PowerShell\)<a name="systems-manager-managedinstances-advanced-enabling-ps"></a>

The following procedure shows you how to use the AWS Tools for Windows PowerShell to change *all* on\-premises servers and VMs that were added using managed\-instance activation, in the specified AWS account and Region, to use the advanced\-instances tier\.

**Important**  
The following procedure describes how to change an account\-level setting\. This change results in charges being billed to your account\.

**To enable the advanced\-instances tier using PowerShell**

1. Open AWS Tools for Windows PowerShell and run the following command\.

   ```
   Update-SSMServiceSetting `
       -SettingId "arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier" `
       -SettingValue "advanced"
   ```

   There is no output if the command succeeds\.

1. Run the following command to view the current service settings for managed instances in the current AWS account and Region\.

   ```
   Get-SSMServiceSetting `
       -SettingId "arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier"
   ```

   The command returns information like the following\.

   ```
   ARN:arn:aws:ssm:us-east-2:123456789012:servicesetting/ssm/managed-instance/activation-tier
   LastModifiedDate : 4/18/2019 4:02:56 PM
   LastModifiedUser : arn:aws:sts::123456789012:assumed-role/Administrator/User_1
   SettingId        : /ssm/managed-instance/activation-tier
   SettingValue     : advanced
   Status           : PendingUpdate
   ```

The system can take several minutes to complete the process of moving all instances from the standard\-instances tier to the advanced\-instances tier\.

**Note**  
For information about changing back to the standard\-instances tier, see [Reverting from the advanced\-instances tier to the standard\-instances tier](systems-manager-managed-instances-advanced-reverting.md)\.