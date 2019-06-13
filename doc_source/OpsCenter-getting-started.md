# Getting Started with OpsCenter<a name="OpsCenter-getting-started"></a>

Complete the following steps to get started with OpsCenter\. 

**Topics**
+ [Step 1: Configuring CloudWatch Events Permissions for Automatically Creating OpsItems](#OpsCenter-getting-started-service-permissions)
+ [Step 2: Configuring User or Group Permissions for OpsCenter](#OpsCenter-getting-started-user-permissions)
+ [Step 3: \(Optional\) Create OpsItem Guidelines for Your Organization](#OpsCenter-getting-started-guidelines)

**Before You Begin**  
Before you get started with OpsCenter, review the pricing details\. For more information, see [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/)\. Also, verify that your IAM user, group, or role has permission to use Systems Manager features and capabilities\. For more information, see [Setting Up AWS Systems Manager](systems-manager-setting-up.md)\. 

**Note**  
OpsCenter enables you to remediate issues with AWS resources by using Systems Manager Automation documents \(runbooks\)\. To use this remediation capability, you must have permission to run Systems Manager Automation documents\. For more information, see [Getting Started with Automation](automation-setup.md)\.

## Step 1: Configuring CloudWatch Events Permissions for Automatically Creating OpsItems<a name="OpsCenter-getting-started-service-permissions"></a>

You can configure Amazon CloudWatch Events to automatically create OpsItems in response to events, such as a state change for an AWS resource, a change in security settings, or a service unavailable state\. By default, Amazon CloudWatch Events doesn't have permission to create OpsItems\. Grant permission by using an AWS Identity and Access Management \(IAM\) policy\. You assign the policy to a role, and then attach the role to CloudWatch Events\.

This section includes the following procedures\.
+ [To create an OpsCenter policy for CloudWatch Events](#OpsCenter-getting-started-service-permissions-procedure-1)
+ [To create an OpsCenter role for CloudWatch Events](#OpsCenter-getting-started-service-permissions-procedure-2)
+ [To attach the OpsCenter policy to the OpsCenter role for CloudWatch Events](#OpsCenter-getting-started-service-permissions-procedure-3)

Use the following procedure to create an IAM policy that enables CloudWatch Events to automatically create OpsItems\.<a name="OpsCenter-getting-started-service-permissions-procedure-1"></a>

**To create an OpsCenter policy for CloudWatch Events**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**\.

1. Choose **Create policy**\.

1. Choose the **JSON** tab\.

1. Replace the default content with the following:

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "ssm:CreateOpsItem",
               "Resource": "*"
           }
       ]
   }
   ```

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter a name\. For example: **OpsCenter\-CWE\-Policy**\.

1. For **Description**, enter information about this policy that identifies its purpose\.

1. Choose **Create policy**\.

Use the following procedure to create an IAM role that enables CloudWatch Events to automatically create OpsItems in OpsCenter\.<a name="OpsCenter-getting-started-service-permissions-procedure-2"></a>

**To create an OpsCenter role for CloudWatch Events**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. Choose **Create role**\.

1. On the **Create role** page, under **Select type of trusted entity**, verify that **AWS service** is selected\.

1. Under **Choose the service that will use this role**, choose **CloudWatch Events**\.

1. Under **Select your use case**, choose **CloudWatch Events**, and then choose **Next: Permissions**\.

1. On the **Permissions** page, leave the default settings and choose **Next: Tags**\.

1. \(Optional\) On the **Tags** page, specify key\-value tag pairs, and then choose **Next: Review**\.

1. On the **Review** page, for **Role name**, enter a name\. For example: **OpsItem\-CWE\-Role**\.

1. For **Description**, either leave the default description or enter information about this role that identifies its purpose\.

1. Choose **Create role**\.

Use the following procedure to attach the policy you created earlier to the role you just created\. <a name="OpsCenter-getting-started-service-permissions-procedure-3"></a>

**To attach the OpsCenter policy to the OpsCenter role for CloudWatch Events**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Roles**\.

1. Locate the role you just created\.

1. Choose the name of the role to open the **Summary** page\.

1. \(Optional\) You can detach the policies that were automatically assigned to this role when you created it\. Choose the **X** beside each policy to detach it\.

1. Choose **Attach policies**\.

1. On the **Attach permissions** page, locate the OpsCenter policy that you created earlier\.

1. Choose this policy, and then choose **Attach policy**\. IAM returns you to the **Roles** page\.

1. Choose the name of the role to open the **Summary** page\.

1. Copy the role ARN\. You must specify this ARN when you configure OpsCenter to automatically create OpsItems by using CloudWatch Events\. For more information, see [Enabling the Default CloudWatch Events Rules for Automatically Creating OpsItems](OpsCenter-creating-OpsItems.md#OpsCenter-automatically-create-OpsItems-1)\.

CloudWatch Events now has the required permissions to automatically create OpsItems in OpsCenter\.

## Step 2: Configuring User or Group Permissions for OpsCenter<a name="OpsCenter-getting-started-user-permissions"></a>

OpsItems can only be viewed or edited in the account where they were created\. You can't share or transfer OpsItems across AWS accounts\. For this reason, we recommend that you configure permissions for OpsCenter in the AWS account that is used to run your AWS workloads\. You can then create AWS Identity and Access Management \(IAM\) users or groups in that account\. In this way, multiple operations engineers or IT professionals can create, view, and edit OpsItems in the same AWS account\.

OpsCenter uses the following API actions\. You can use all features of OpsCenter if your IAM user, group, or role has access to these actions\. You can also create more restrictive access, as described later in this section\.
+ [CreateOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateOpsItem.html)
+ [DescribeOpsItems](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeOpsItems.html)
+ [GetOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetOpsItem.html)
+ [GetOpsSummary](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetOpsSummary.html)
+ [UpdateOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateOpsItem.html)

The following procedure describes how to add a full\-access inline policy to an IAM user\. If you prefer, you can specify read\-only permission by assigning the following inline policy to a user's account, group, or role\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ssm:GetOpsItem",
        "ssm:GetOpsSummary",
        "ssm:DescribeOpsItems"
      ],
      "Resource": "*"
    }
  ]
}
```

For more information about creating and editing IAM policies, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\. For information about how to assign this policy to an IAM group, see [Attaching a Policy to an IAM Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_manage_attach-policy.html)\. 

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**\.

1. In the list, choose a name\.

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
           "ssm:GetOpsItem",
           "ssm:UpdateOpsItem",
           "ssm:DescribeOpsItems",
           "ssm:CreateOpsItem",
           "ssm:GetOpsSummary"
   
         ],
         "Resource": "*"
       }
     ]
   }
   ```

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter a name for the inline policy\. For example: **OpsCenter\-Access\-Full**\.

1. Choose **Create policy**\.

### Restricting Access to OpsItems by Using Tags<a name="OpsCenter-getting-started-user-permissions-tags"></a>

You can also restrict access to OpsItems by using an inline IAM policy that specifies tags\. The policy uses the following format\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "One_or_more_OpsItem_API_actions"
             ],
      "Resource": "*"
	  ,
	  "Condition": { "StringEquals": { "ssm:resourceTag/tag_key": "tag_value" } }
    }
  ]
}
```

Here is an example that species a tag key of *Department* and a tag value of *Finance*\. With this policy, the user can only call the *GetOpsItem* API action to view OpsItems that were previously tagged with Key=Department and Value=Finance\. Users can't view any other OpsItems\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ssm:GetOpsItem"
             ],
      "Resource": "*"
	  ,
	  "Condition": { "StringEquals": { "ssm:resourceTag/Department": "Finance" } }
    }
  ]
}
```

Here is an example that species API actions for viewing and updating OpsItems\. This policy also specifies two sets of tag key\-value pairs: Department\-Finance and Project\-Unity\.

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "ssm:GetOpsItem",
            "ssm:UpdateOpsItem"
         ],
         "Resource":"*",
         "Condition":{
            "StringEquals":{
               "ssm:resourceTag/Department":"Finance",
               "ssm:resourceTag/Project":"Unity"
            }
         }
      }
   ]
}
```

For information about adding tags to an OpsItem, see [Creating OpsItems Manually](OpsCenter-creating-OpsItems.md#OpsCenter-manually-create-OpsItems)\.

## Step 3: \(Optional\) Create OpsItem Guidelines for Your Organization<a name="OpsCenter-getting-started-guidelines"></a>

We recommend that each organization create a simple set of guidelines that promote consistency when creating and editing OpsItems\. Guidelines make it easier for users to locate and resolve OpsItems\. The guidelines for your organization should define best practices when users enter information into the following OpsItem fields\.

**Note**  
Amazon CloudWatch Events populates the **Title**, **Source**, and **Description** fields of automatically generated OpsItems\. You can edit the **Title** and the **Description** fields, but you can't edit the **Source** field\.


****  

| Field | Description | 
| --- | --- | 
|  **Title**  |  Guidelines should encourage a consistent OpsItem naming experience\. For example, your guidelines might require that each title include information about the impacted resource, the status, the environment, and the name or the alias of the engineer actively working the issue, if applicable\. All OpsItems created by CloudWatch include a title that describes the event that caused the creation of the OpsItem, but you can edit these titles\.You can search OpsItems for *Title*:*contains*\. If your naming guidelines encourage consistent use of keywords, you improve your search results\.  | 
|  **Source**  |  Guidelines can include specifying IDs, software version numbers \(if applicable\) or other relevant data to help users identify the origin of the issue\. You can't edit the **Source** field after the OpsItem is created\.  | 
|  **Priority**  |  \(Optional\) Guidelines include determining the highest and lowest priority for your organization, and any service\-level agreements based on priority\. You can specify priority from 1 to 5\.  | 
|  **Description**  |  Guidelines should suggest how much detail about the issue to include and any steps \(if applicable\) for reproducing the issue\.   | 
|  **Notifications**  |  Guidelines should suggest which Amazon Simple Notification Service \(SNS\) topic Amazon Resource Name \(ARN\) to specify when creating or editing OpsItems\. Be aware that SNS notifications are region\-specific\. This means you must specify an ARN that is in the same AWS Region as the OpsItem\.  | 
|  **Related Resources**  |  Guidelines can include details about which Resources should or shouldn't have an ARN specified\. For supported AWS resource types, the ARN creates a deep link to details about the Resource\.   | 
|  **Operational data**  |  You can specify custom data for each OpsItem that provides context about the issue and other relevant data for future reference\. You can specify searchable custom data\. All users with access to the OpsItem Overview page can search for and view this data\. You can also specify private custom data that is only viewable by users who have access to this OpsItem\. Guidelines could specify structure and standards for key\-value pairs\. These key\-value pairs can describe operational data and resolution details, leading to improved search results\.  | 