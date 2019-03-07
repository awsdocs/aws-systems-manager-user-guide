# Using the Advanced\-Instances Tier<a name="systems-manager-managedinstances-advanced"></a>

AWS Systems Manager offers a standard\-instances tier and an advanced\-instances tier for servers and VMs in your hybrid environment\. The standard\-instances tier enables you to register a maximum of 1,000 servers or VMs per AWS account per AWS Region\. If you need to register more than 1,000 servers or VMs in a single account and Region, then use the advanced\-instances tier\. You can create as many instances as you like in the advanced\-instances tier, but all instances configured for Systems Manager are available on a pay\-per\-use basis \(including Amazon EC2 instances that use a Systems Manager on\-premises activation\)\.

**Note**  
Advanced instances also enable you to connect to your hybrid machines by using AWS Systems Manager Session Manager\. Session Manager provides interactive shell access to your instances\. For more information, see [AWS Systems Manager Session Manager](session-manager.md)\.
The standard\-instances limit also applies to Amazon EC2 instances that use a Systems Manager on\-premises activation \(which is not a common scenario\)\.

This section describes how to configure your hybrid environment to use the advanced\-instances tier\.

**Before You Begin**  
Review pricing details for advanced instances\. Advanced instances are an account\-level feature and *all* instances in the account are available on a per\-use\-basis\. For more information see, [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)\. 

## Configuring Permissions to Enable the Advanced\-Instances Tier<a name="systems-manager-managedinstances-advanced-permissions"></a>

Verify that you have permission in AWS Identity and Access Management \(IAM\) to change your environment from the standard\-instances tier to the advanced\-instances tier\. You must either have the AdministratorAccess policy attache to your IAM user, group, or role\. Or, you must have permission to change the Systems Manager activation\-tier service setting\. The activation\-tier setting uses the following API actions: 
+ [GetServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetServiceSetting.html)
+ [UpdateServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateServiceSetting.html)
+ [ResetServiceSetting](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ResetServiceSetting.html)

Use the following procedure to add an inline IAM policy to user a account\. This policy enables a user to view the current managed\-instance tier setting\. This policy also enables the user to change or reset the current setting\.

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
                   "ssm:ResetServiceSetting",
                   "ssm:UpdateServiceSetting"
               ],
               "Resource": "arn:aws:ssm:AWS_Region:AWS_account_ID:servicesetting/ssm/managed-instance/activation-tier"
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

## Enabling the Advanced\-Instances Tier<a name="systems-manager-managedinstances-advanced-enabling"></a>

Use the following procedure to change all instances in the current AWS account and Region to use the advanced\-instances tier\.

**Important**  
The following procedure describes how to change an account\-level setting\. This change results in charges being billed to your account\. If you want to change back to the standard\-instances tier, then you must contact AWS Support\.

**To enable the advanced\-instances tier**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Managed instances**\.

1. Choose the **Settings** tab\.

1. Choose **Change account settings**\.

1. Review the information in the pop\-up about changing account settings, and then, if you approve, choose the option to accept and continue\.

The system can take several minutes to complete the process of moving all instances from the standard\-instances tier to the advanced\-instances tier\.