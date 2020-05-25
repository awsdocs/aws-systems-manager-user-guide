# Increasing Parameter Store throughput<a name="parameter-store-throughput"></a>

Increasing Parameter Store throughput increases the maximum number of transactions per second \(TPS\) that Parameter Store can process\. Increased throughput enables you to operate Parameter Store at higher volumes to support applications and workloads that need concurrent access to a large number of parameters\. You can increase the limit to 1,000 TPS on the **Settings** tab\. Increasing the throughput limit incurs a charge on your AWS account\. For more information, see [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)\.

**Note**  
The Parameter Store throughput setting applies to all transactions created by all AWS Identity and Access Management \(IAM\) users in the current AWS account and Region\. The throughput setting applies to standard and advanced parameters\. 

## Configuring permissions to increase Parameter Store throughput<a name="parameter-store-throughput-permissions"></a>

Verify that you have permission in AWS Identity and Access Management \(IAM\) to increase Parameter Store throughput by doing one of the following:
+ Ensure that the `AdministratorAccess` policy is attached to your IAM user, group, or role\.
+ Ensure that you have permission to change the throughput service setting by using the following API actions:
  + [GetServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetServiceSetting.html)
  + [UpdateServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateServiceSetting.html)
  + [ResetServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ResetServiceSetting.html)

Use the following procedure to add an inline IAM policy to a user account\. This policy enables a user to view and change the parameter\-throughput setting for parameters in their account and Region\. 

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**\.

1. In the list, choose the name of the user to attach a policy to\.

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
                   "ssm:UpdateServiceSetting"
               ],
               "Resource": "arn:aws:ssm:region:account-id:servicesetting/ssm/parameter-store/high-throughput-enabled"
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter a name for the inline policy, such as **Parameter\-Store\-Throughput** or another name you prefer\.

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

## Increasing throughput \(console\)<a name="parameter-store-throughput-increasing"></a>

The following procedure shows how to use the Systems Manager console to increase the number of transactions per second that Parameter Store can process for the current AWS account and Region\.

**Tip**  
If you haven't created a parameter yet, you can use the AWS CLI or AWS Tools for Windows PowerShell to increase throughput\. For information, see [Increasing throughput \(AWS CLI\)](#parameter-store-throughput-increasing-cli) and [Increasing throughput \(PowerShell\)](#parameter-store-throughput-increasing-ps)\.

**To increase Parameter Store throughput**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose the **Settings** tab\.

1. Choose **Set limit**\.

1. Review the message, and choose **Accept**\.

If you no longer need increased throughput, or if you no longer want to incur charges, you can revert to the standard settings\. To revert your settings, repeat this procedure and choose **Reset limit**\.

## Increasing throughput \(AWS CLI\)<a name="parameter-store-throughput-increasing-cli"></a>

The following procedure shows how to use the AWS Command Line Interface \(AWS CLI\) to increase the number of transactions per second that Parameter Store can process for the current AWS account and Region\.

**To increase Parameter Store throughput using the AWS CLI**

1. Open the AWS CLI and run the following command to increase the transactions per second that Parameter Store can process in the current AWS account and Region\.

   ```
   aws ssm update-service-setting --setting-id arn:aws:ssm:region:account-id:servicesetting/ssm/parameter-store/high-throughput-enabled --setting-value true
   ```

   There is no output if the command succeeds\.

1. Run the following command to view the current throughput service settings for Parameter Store in the current AWS account and Region\.

   ```
   aws ssm get-service-setting --setting-id arn:aws:ssm:region:account-id:servicesetting/ssm/parameter-store/high-throughput-enabled
   ```

   ```
   {
       "ServiceSetting": {
           "SettingId": "/ssm/parameter-store/high-throughput-enabled",
           "SettingValue": "true",
           "LastModifiedDate": 1556551683.923,
           "LastModifiedUser": "arn:aws:sts::123456789012:assumed-role/Administrator/Jasper",
           "ARN": "arn:aws:ssm:us-east-2:123456789012:servicesetting/ssm/parameter-store/high-throughput-enabled",
           "Status": "Customized"
       }
   }
   ```

If you no longer need increased throughput, or if you no longer want to incur charges, you can revert to the standard settings\. To revert your settings, run the following command\.

```
aws ssm reset-service-setting --setting-id arn:aws:ssm:region:account-id:servicesetting/ssm/parameter-store/high-throughput-enabled
```

```
{
    "ServiceSetting": {
        "SettingId": "/ssm/parameter-store/high-throughput-enabled",
        "SettingValue": "false",
        "LastModifiedDate": 1555532818.578,
        "LastModifiedUser": "System",
        "ARN": "arn:aws:ssm:us-east-2:123456789012:servicesetting/ssm/parameter-store/high-throughput-enabled",
        "Status": "Default"
    }
}
```

## Increasing throughput \(PowerShell\)<a name="parameter-store-throughput-increasing-ps"></a>

The following procedure shows how to use the AWS Tools for Windows PowerShell to increase the number of transactions per second that Parameter Store can process for the current AWS account and Region\.

**To increase Parameter Store throughput using PowerShell**

1. Increase Parameter Store throughput in the current AWS account and Region using the AWS Tools for PowerShell\.

   ```
   Update-SSMServiceSetting -SettingId "arn:aws:ssm:region:account-id:servicesetting/ssm/parameter-store/high-throughput-enabled" -SettingValue "true" -Region us-east-2
   ```

   There is no output if the command succeeds\.

1. Run the following command to view the current throughput service settings for Parameter Store in the current AWS account and Region\.

   ```
   Get-SSMServiceSetting -SettingId "arn:aws:ssm:region:account-id:servicesetting/ssm/parameter-store/high-throughput-enabled" -Region us-east-2
   ```

   ```
   ARN              : arn:aws:ssm:us-east-2:123456789012:servicesetting/ssm/parameter-store/high-throughput-enabled
   LastModifiedDate : 4/29/2019 3:35:44 PM
   LastModifiedUser : arn:aws:sts::123456789012:assumed-role/Administrator/Jasper
   SettingId        : /ssm/parameter-store/high-throughput-enabled
   SettingValue     : true
   Status           : Customized
   ```

If you no longer need increased throughput, or if you no longer want to incur charges, you can revert to the standard settings\. To revert your settings, run the following command\.

```
Reset-SSMServiceSetting -SettingId "arn:aws:ssm:region:account-id:servicesetting/ssm/parameter-store/high-throughput-enabled" -Region region
```

```
ARN              : arn:aws:ssm:us-east-2:123456789012:servicesetting/ssm/parameter-store/high-throughput-enabled
LastModifiedDate : 4/17/2019 8:26:58 PM
LastModifiedUser : System
SettingId        : /ssm/parameter-store/high-throughput-enabled
SettingValue     : false
Status           : Default
```