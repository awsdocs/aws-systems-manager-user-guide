# Configuring roles and permissions for Systems Manager Explorer<a name="Explorer-setup-permissions"></a>

Integrated Setup automatically creates and configures AWS Identity and Access Management \(IAM\) roles for AWS Systems Manager Explorer and AWS Systems Manager OpsCenter\. If you completed Integrated Setup, then you don't need to perform any additional tasks to configure roles and permissions for Explorer\. However, you must configure permission for OpsCenter, as described later in this topic\.

**Topics**
+ [About the roles created by integrated setup](#Explorer-setup-permissions-about)
+ [Configuring permissions for Systems Manager OpsCenter](#Explorer-getting-started-user-permissions)

## About the roles created by integrated setup<a name="Explorer-setup-permissions-about"></a>

Integrated Setup creates and configures the following roles for working with Explorer and OpsCenter\.
+ `AWSServiceRoleForAmazonSSM`: Provides access to AWS resources managed or used by Systems Manager\.
+ `OpsItem-CWE-Role`: Allows CloudWatch Events and EventBridge to create OpsItems in response to common events\.
+ `AWSServiceRoleForAmazonSSM_AccountDiscovery`: Allows Systems Manager to call other AWS services to discover AWS account information when synchronizing data\. For more information about this role, see [About the `AWSServiceRoleForAmazonSSM_AccountDiscovery` role](#Explorer-service-role-details)\.
+ `AmazonSSMExplorerExport`: Allows Explorer to export OpsData to a comma\-separated value \(CSV\) file\.

### About the `AWSServiceRoleForAmazonSSM_AccountDiscovery` role<a name="Explorer-service-role-details"></a>

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

For more information about the `AWSServiceRoleForAmazonSSM_AccountDiscovery` role, see [Using roles to collect AWS account information for OpsCenter and Explorer: `AWSServiceRoleForAmazonSSM_AccountDiscovery`](using-service-linked-roles-service-action-2.md)\.

## Configuring permissions for Systems Manager OpsCenter<a name="Explorer-getting-started-user-permissions"></a>

After you complete Integrated Setup, you must configure user, group, or role permissions so that users can perform actions in OpsCenter\.

**Before You Begin**  
OpsItems can only be viewed or edited in the account where they were created\. You can't share or transfer OpsItems across AWS accounts\. For this reason, we recommend that you configure permissions for OpsCenter in the AWS account that is used to run your AWS workloads\. You can then create users or groups in that account\. In this way, multiple operations engineers or IT professionals can create, view, and edit OpsItems in the same AWS account\.

Explorer and OpsCenter use the following API operations\. You can use all features of Explorer and OpsCenter if your user, group, or role has access to these actions\. You can also create more restrictive access, as described later in this section\.
+  [CreateOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateOpsItem.html) 
+  [CreateResourceDataSync](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateResourceDataSync.html) 
+  [DescribeOpsItems](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeOpsItems.html) 
+  [DeleteResourceDataSync](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeleteResourceDataSync.html) 
+  [GetOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetOpsItem.html) 
+  [GetOpsSummary](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetOpsSummary.html) 
+  [ListResourceDataSync](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_ListResourceDataSync.html) 
+  [UpdateOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateOpsItem.html) 
+  [UpdateResourceDataSync](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_UpdateResourceDataSync.html) 

If you prefer, you can specify read\-only permission by adding the following inline policy to your account, group, or role\.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ssm:GetOpsItem",
        "ssm:GetOpsSummary",
        "ssm:DescribeOpsItems",
        "ssm:GetServiceSetting",
        "ssm:ListResourceDataSync"
      ],
      "Resource": "*"
    }
  ]
}
```

For more information about creating and editing IAM policies, see [Creating IAM Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) in the *IAM User Guide*\. For information about how to assign this policy to an IAM group, see [Attaching a Policy to an IAM Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_manage_attach-policy.html)\. 

Create a permission using the following and add it to your users, groups, or roles: 

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

Depending on the identity application that you are using in your organization,  you can select any of the following options to configure user access\.

To provide access, add permissions to your users, groups, or roles:
+ Users and groups in AWS IAM Identity Center \(successor to AWS Single Sign\-On\):

  Create a permission set\. Follow the instructions in [Create a permission set](https://docs.aws.amazon.com/singlesignon/latest/userguide/howtocreatepermissionset.html) in the *AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide*\.
+ Users managed in IAM through an identity provider:

  Create a role for identity federation\. Follow the instructions in [Creating a role for a third\-party identity provider \(federation\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-idp.html) in the *IAM User Guide*\.
+ IAM users:
  + Create a role that your user can assume\. Follow the instructions in [Creating a role for an IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user.html) in the *IAM User Guide*\.
  + \(Not recommended\) Attach a policy directly to a user or add a user to a user group\. Follow the instructions in [Adding permissions to a user \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html#users_change_permissions-add-console) in the *IAM User Guide*\.

### Restricting access to OpsItems by using tags<a name="OpsCenter-getting-started-user-permissions-tags"></a>

You can also restrict access to OpsItems by using an inline IAM policy that specifies tags\. Here is an example that specifies a tag key of *Department* and a tag value of *Finance*\. With this policy, the user can only call the *GetOpsItem* API operation to view OpsItems that were previously tagged with Key=Department and Value=Finance\. Users can't view any other OpsItems\.

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

Here is an example that specifies API operations for viewing and updating OpsItems\. This policy also specifies two sets of tag key\-value pairs: Department\-Finance and Project\-Unity\.

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