# Configuring roles and permissions for Systems Manager Explorer<a name="Explorer-setup-permissions"></a>

Integrated Setup automatically creates and configures IAM roles for Systems Manager Explorer and OpsCenter\. If you completed Integrated Setup, then you don't need to perform any additional tasks to configure roles and permissions for Explorer\. However, you must configure permission for OpsCenter, as described later in this topic\.

**Topics**
+ [About the roles created by integrated setup](#Explorer-setup-permissions-about)
+ [Configuring permissions for Systems Manager OpsCenter](#Explorer-getting-started-user-permissions)

## About the roles created by integrated setup<a name="Explorer-setup-permissions-about"></a>

Integrated Setup creates and configures the following roles for working with Explorer and OpsCenter\.
+ **AWSServiceRoleForAmazonSSM**: Provides access to AWS Resources managed or used by Systems Manager\.
+ **OpsItem\-CWE\-Role**: Enables CloudWatch Events and EventBridge to create OpsItems in response to common events\.
+ **AWSServiceRoleForAmazonSSM\_AccountDiscovery**: Enables Systems Manager to call other AWS services to discover AWS account information when synchronizing data\. For more information about this role, see [About the AWSServiceRoleForAmazonSSM\_AccountDiscovery role](#Explorer-service-role-details)\.
+ **AmazonSSMExplorerExport**: Enables Explorer to export OpsData to a comma\-separated value \(CSV\) file\.

### About the AWSServiceRoleForAmazonSSM\_AccountDiscovery role<a name="Explorer-service-role-details"></a>

If you configure Explorer to display data from multiple accounts and Regions by using AWS Organizations and a resource data sync, then Systems Manager creates a service\-linked role\. Systems Manager uses this role to get information about your AWS accounts in AWS Organizations\. The role uses the following permissions policy\.

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "organizations:DescribeAccount",
            "organizations:DescribeOrganization",
            "organizations:ListAccounts",
            "organizations:ListAWSServiceAccessForOrganization",
            "organizations:ListChildren",
            "organizations:ListParents"
         ],
         "Resource":"*"
      }
   ]
}
```

For more information about the AWSServiceRoleForAmazonSSM\_AccountDiscovery role, see [Using Roles to Collect AWS Account Information for Systems Manager Explorer](using-service-linked-roles-service-action-2.md)\.

## Configuring permissions for Systems Manager OpsCenter<a name="Explorer-getting-started-user-permissions"></a>

After you complete Integrated Setup, you must configure IAM user, group, or role permissions so that users can perform actions in OpsCenter\.

**Before You Begin**  
OpsItems can only be viewed or edited in the account where they were created\. You can't share or transfer OpsItems across AWS accounts\. For this reason, we recommend that you configure permissions for OpsCenter in the AWS account that is used to run your AWS workloads\. You can then create IAM users or groups in that account\. In this way, multiple operations engineers or IT professionals can create, view, and edit OpsItems in the same AWS account\.

Explorer and OpsCenter use the following API actions\. You can use all features of Explorer and OpsCenter if your IAM user, group, or role has access to these actions\. You can also create more restrictive access, as described later in this section\.
+  [CreateOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateOpsItem.html) 
+  [CreateResourceDataSync](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateResourceDataSync.html) 
+  [DescribeOpsItems](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeOpsItems.html) 
+  [DeleteResourceDataSync](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeleteResourceDataSync.html) 
+  [GetOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetOpsItem.html) 
+  [GetOpsSummary](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetOpsSummary.html) 
+  [ListResourceDataSync](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ListResourceDataSync.html) 
+  [UpdateOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateOpsItem.html) 
+  [UpdateResourceDataSync](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateResourceDataSync.html) 

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
           "ssm:CreateResourceDataSync",
           "ssm:DeleteResourceDataSync",
           "ssm:ListResourceDataSync",
           "ssm:UpdateResourceDataSync"
   
         ],
         "Resource": "*"
       }
     ]
   }
   ```

1. Choose **Review policy**\.

1. On the **Review policy** page, for **Name**, enter a name for the inline policy\. For example: **OpsCenter\-Access\-Full**\.

1. Choose **Create policy**\.

### Restricting access to OpsItems by using tags<a name="OpsCenter-getting-started-user-permissions-tags"></a>

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

Here is an example that specifies a tag key of *Department* and a tag value of *Finance*\. With this policy, the user can only call the *GetOpsItem* API action to view OpsItems that were previously tagged with Key=Department and Value=Finance\. Users can't view any other OpsItems\.

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

Here is an example that specifies API actions for viewing and updating OpsItems\. This policy also specifies two sets of tag key\-value pairs: Department\-Finance and Project\-Unity\.

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

For information about adding tags to an OpsItem, see [Creating OpsItems manually](OpsCenter-manually-create-OpsItems.md)\.