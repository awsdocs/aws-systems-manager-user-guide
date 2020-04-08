# Using Roles to Collect AWS Account Information for Systems Manager Explorer<a name="using-service-linked-roles-service-action-2"></a>

AWS Systems Manager uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Systems Manager\. Service\-linked roles are predefined by Systems Manager and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up Systems Manager easier because you don’t have to manually add the necessary permissions\. Systems Manager defines the permissions of its service\-linked roles, and unless defined otherwise, only Systems Manager can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

You can delete a service\-linked role only after first deleting their related resources\. This protects your Systems Manager resources because you can't inadvertently remove permission to access the resources\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for Systems Manager<a name="service-linked-role-permissions-service-action-2"></a>

Systems Manager uses the service\-linked role named **AWSServiceRoleForAmazonSSM\_AccountDiscovery** – AWS Systems Manager uses this IAM service role to call other AWS services to discover AWS account information\.

The AWSServiceRoleForAmazonSSM\_AccountDiscovery service\-linked role trusts the following services to assume the role:
+ `accountdiscovery.ssm.amazonaws.com`

The role permissions policy allows Systems Manager to complete the following actions on the specified resources:
+ organizations:DescribeAccount
+ organizations:DescribeOrganization
+ organizations:ListAccounts
+ organizations:ListAWSServiceAccessForOrganization
+ organizations:ListChildren
+ organizations:ListParents

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-Linked Role Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating a service\-linked role for Systems Manager<a name="create-service-linked-role-service-action-2"></a>

You don't need to manually create a service\-linked role\. When you create a resource data sync by using Systems Manager Explorer in the AWS Management Console, the AWS CLI, or the AWS API, Systems Manager creates the service\-linked role for you\. 

If you delete this service\-linked role, and then need to create it again, you can use the same process to recreate the role in your account\. When you create a resource data sync by using Systems Manager Explorer, Systems Manager creates the service\-linked role for you again\. 

## Editing a service\-linked role for Systems Manager<a name="edit-service-linked-role-service-action-2"></a>

Systems Manager does not allow you to edit the AWSServiceRoleForAmazonSSM\_AccountDiscovery service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a service\-linked role for Systems Manager<a name="delete-service-linked-role-service-action-2"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way you don’t have an unused entity that is not actively monitored or maintained\. However, you must clean up your service\-linked role before you can manually delete it\.

### Cleaning up a service\-linked role<a name="service-linked-role-review-before-delete-service-action-2"></a>

Before you can use IAM to delete a service\-linked role, you must first delete all Explorer resource data syncs\. For more information, see [Deleting a Systems Manager Explorer Resource Data Sync](Explorer-using-resource-data-sync-delete.md)\.

**Note**  
If the Systems Manager service is using the role when you try to delete the resources, then the deletion might fail\. If that happens, wait for a few minutes and try the operation again\.

### Manually delete the service\-linked role<a name="slr-manual-delete-service-action-2"></a>

Use the IAM console, the AWS CLI, or the AWS API to delete the AWSServiceRoleForAmazonSSM\_AccountDiscovery service\-linked role\. For more information, see [Deleting a Service\-Linked Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported Regions for Systems Manager service\-linked roles<a name="slr-regions-service-action-2"></a>

Systems Manager supports using service\-linked roles in all of the regions where the service is available\. For more information, see [AWS Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html)\.

Systems Manager does not support using service\-linked roles in every region where the service is available\. You can use the AWSServiceRoleForAmazonSSM\_AccountDiscovery role in the following regions\.


****  

| Region name | Region identity | Support in Systems Manager | 
| --- | --- | --- | 
| US East \(N\. Virginia\) | us\-east\-1 | Yes | 
| US East \(Ohio\) | us\-east\-2 | Yes | 
| US West \(N\. California\) | us\-west\-1 | Yes | 
| US West \(Oregon\) | us\-west\-2 | Yes | 
| Asia Pacific \(Mumbai\) | ap\-south\-1 | Yes | 
| Asia Pacific \(Osaka\-Local\) | ap\-northeast\-3 | Yes | 
| Asia Pacific \(Seoul\) | ap\-northeast\-2 | Yes | 
| Asia Pacific \(Singapore\) | ap\-southeast\-1 | Yes | 
| Asia Pacific \(Sydney\) | ap\-southeast\-2 | Yes | 
| Asia Pacific \(Tokyo\) | ap\-northeast\-1 | Yes | 
| Canada \(Central\) | ca\-central\-1 | Yes | 
| Europe \(Frankfurt\) | eu\-central\-1 | Yes | 
| Europe \(Ireland\) | eu\-west\-1 | Yes | 
| Europe \(London\) | eu\-west\-2 | Yes | 
| Europe \(Paris\) | eu\-west\-3 | Yes | 
| South America \(São Paulo\) | sa\-east\-1 | Yes | 
| AWS GovCloud \(US\) | us\-gov\-west\-1 | No | 