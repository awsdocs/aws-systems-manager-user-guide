# Reverting from the advanced\-instances tier to the standard\-instances tier<a name="systems-manager-managed-instances-advanced-reverting"></a>

This section describes how to change hybrid instances currently running in the advanced\-instances tier back to the standard\-instances tier\. This configuration applies to all hybrid instances in an AWS account and a single AWS Region\.

**Before You Begin**  
Review the following important details\.

**Note**  
You can't revert back to the standard\-instance tier if you are running more than 1,000 hybrid instances in the account and Region\. You must first deregister hybrid instances until you have 1,000 or fewer\. This also applies to EC2 instances that use a Systems Manager on\-premises activation \(which is not a common scenario\)\. For more information, see [Deregistering managed instances in a hybrid environment](systems-manager-managed-instances-advanced-deregister.md)\.
After you revert, you won't be able to use Session Manager to interactively access your hybrid instances\.
After you revert, you won't be able to use Patch Manager to patch Microsoft applications on hybrid servers and virtual machines \(VMs\)\.
The process of reverting all hybrid instances back to the standard\-instance tier can take 30 minutes or more to complete\.

This section describes how to revert all hybrid instances in an AWS account and Region from the advanced\-instances tier to the standard\-instances tier\.

## Reverting to the standard\-instances tier \(console\)<a name="systems-manager-managed-instances-advanced-reverting-console"></a>

The following procedure shows you how to use the Systems Manager console to change all on\-premises servers and VMs in your hybrid environment to use the standard\-instances tier in the specified AWS account and Region\.

**To revert to the standard\-instances tier \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Managed instances**\.

1. Choose the **Settings** tab\.

1. Choose **Change account setting**\.

1. Review the information in the pop\-up about changing account settings, and then if you approve, choose the option to accept and continue\.

## Reverting to the standard\-instances tier \(AWS CLI\)<a name="systems-manager-managed-instances-advanced-reverting-cli"></a>

The following procedure shows you how to use the AWS CLI to change all on\-premises servers and VMs in your hybrid environment to use the standard\-instances tier in the specified AWS account and Region\.

**To revert to the standard\-instances tier using the AWS CLI**

1. Open the AWS CLI and run the following command\.

------
#### [ Linux ]

   ```
   aws ssm update-service-setting \
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier \
       --setting-value standard
   ```

------
#### [ Windows ]

   ```
   aws ssm update-service-setting ^
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier ^
       --setting-value standard
   ```

------

   There is no output if the command succeeds\.

1. Run the following command 30 minutes later to view the settings for managed instances in the current AWS account and Region\.

------
#### [ Linux ]

   ```
   aws ssm get-service-setting \
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier
   ```

------
#### [ Linux ]

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
           "SettingValue": "standard",
           "LastModifiedDate": 1555603376.138,
           "LastModifiedUser": "System",
           "ARN": "arn:aws:ssm:us-east-2:123456789012:servicesetting/ssm/managed-instance/activation-tier",
           "Status": "Default"
       }
   }
   ```

   The status changes to *Default* after the request has been approved\.

## Reverting to the standard\-instances tier \(PowerShell\)<a name="systems-manager-managed-instances-advanced-reverting-ps"></a>

The following procedure shows you how to use AWS Tools for Windows PowerShell to change all on\-premises servers and VMs in your hybrid environment to use the standard\-instances tier in the specified AWS account and Region\.

**To revert to the standard\-instances tier using PowerShell**

1. Open AWS Tools for Windows PowerShell and run the following command\.

   ```
   Update-SSMServiceSetting `
       -SettingId "arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier" `
       -SettingValue "standard"
   ```

   There is no output if the command succeeds\.

1. Run the following command 30 minutes later to view the settings for managed instances in the current AWS account and Region\.

   ```
   Get-SSMServiceSetting `
       -SettingId "arn:aws:ssm:region:aws-account-id:servicesetting/ssm/managed-instance/activation-tier"
   ```

   The command returns information like the following\.

   ```
   ARN: arn:aws:ssm:us-east-2:123456789012:servicesetting/ssm/managed-instance/activation-tier
   LastModifiedDate : 4/18/2019 4:02:56 PM
   LastModifiedUser : System
   SettingId        : /ssm/managed-instance/activation-tier
   SettingValue     : standard
   Status           : Default
   ```

   The status changes to *Default* after the request has been approved\.