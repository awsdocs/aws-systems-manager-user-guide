# Increasing Parameter Store Throughput<a name="parameter-store-throughput"></a>

Increasing Parameter Store throughput increases the maximum number of transactions per second \(TPS\) that Parameter Store can processs\. Increased throughput enables you to operate Parameter Store at higher volumes to support applications and workloads that need concurrent access to a large number of parameters\. You can increase the limit to 1,000 TPS on the **Settings** tab\. Increasing the throughput limit incurs a charge on your AWS account\. For more information, see [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)\.

**Note**  
The Parameter Store throughput setting applies to the current AWS account and Region, which means it applies to all AWS Identity and Access Management \(IAM\) users in this AWS account\. The throughput setting applies to standard and advanced parameters\. 

## Configuring Permissions to Increase Parameter Store Throughput<a name="parameter-store-throughput-permissions"></a>

Verify that you have permission in AWS Identity and Access Management \(IAM\) to increase Parameter Store throughput by doing one of the following:
+ Ensure that the `AdministratorAccess` policy is attached to your IAM user, group, or role\.
+ Ensure that you have permission to change the throughput service setting by using the following API actions:
  + [GetServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetServiceSetting.html)
  + [UpdateServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateServiceSetting.html)
  + [ResetServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ResetServiceSetting.html)

Use the following procedure to add an inline IAM policy to a user account\. This policy enables a user to view and change the parameter\-throughput setting for parameters in their account and Region\. 

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
                   "ssm:GetServiceSetting",
                   
               ],
               "Resource": "*"
           },
           {
               "Effect": "Allow",
               "Action": [
                   "ssm:UpdateServiceSetting"
               ],
               "Resource": "arn:aws:ssm:AWS_Region:AWS_account_ID:servicesetting/ssm/parameter-store/high-throughput-enabled"
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter a name for the inline policy\. For example: **Parameter\-Store\-Throughput**\.

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

## Increasing Throughput<a name="parameter-store-throughput-increasing"></a>

Use the following procedure to increase the number of transactions per second that Parameter Store can process for the current AWS account and Region\.

**To increase Parameter Store throughput**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose the **Settings** tab\.

1. Choose **Increase limit**\.

1. Review the message, and choose **Accept**\.

If you no longer need increased throughput, or if you no longer want to incur charges, you can revert to the standard settings\. To revert your settings, repeat this procedure and choose **Reset limit**\.