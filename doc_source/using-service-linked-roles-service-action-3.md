# Using roles to create OpsData and OpsItems for Explorer: `AWSServiceRoleForSystemsManagerOpsDataSync`<a name="using-service-linked-roles-service-action-3"></a>

AWS Systems Manager uses AWS Identity and Access Management \(IAM\) [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Systems Manager\. Service\-linked roles are predefined by Systems Manager and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up Systems Manager easier because you don’t have to manually add the necessary permissions\. Systems Manager defines the permissions of its service\-linked roles, and unless defined otherwise, only Systems Manager can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy can't be attached to any other IAM entity\.

You can delete a service\-linked role only after first deleting their related resources\. This protects your Systems Manager resources because you can't inadvertently remove permission to access the resources\.

For information about other services that support service\-linked roles, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for Systems Manager OpsData sync<a name="slr-permissions-service-action-3"></a>

Systems Manager uses the service\-linked role named `AWSServiceRoleForSystemsManagerOpsDataSync` – AWS Systems Manager uses this IAM service role for Explorer to create OpsData and OpsItems\.

The `AWSServiceRoleForSystemsManagerOpsDataSync` service\-linked role trusts the following services to assume the role:
+ `opsdatasync.ssm.amazonaws.com`

The role permissions policy allows Systems Manager to complete the following actions on the specified resources:
+ Systems Manager Explorer requires that a service\-linked role grant permission to update a security finding when an OpsItem is updated, create and update an OpsItem, and turn off the Security Hub data source when an SSM managed rule is deleted by customers\.

The managed policy that is used to provide permissions for the `AWSServiceRoleForSystemsManagerOpsDataSync` role is `AWSSystemsManagerOpsDataSyncServiceRolePolicy`\. For details about the permissions it grants, see [AWS managed policy: `AWSSystemsManagerOpsDataSyncServiceRolePolicy`](security-iam-awsmanpol.md#security-iam-awsmanpol-AWSSystemsManagerOpsDataSyncServiceRolePolicy)\. 

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-linked role permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating the `AWSServiceRoleForSystemsManagerOpsDataSync` service\-linked role for Systems Manager<a name="create-slr-service-action-3"></a>

You don't need to manually create a service\-linked role\. When you enable Explorer in the AWS Management Console, Systems Manager creates the service\-linked role for you\. 

**Important**  
This service\-linked role can be displayed in your account if you completed an action in another service that uses the features supported by this role\. Also, if you were using the Systems Manager service before January 1, 2017, when it began supporting service\-linked roles, then Systems Manager created the `AWSServiceRoleForSystemsManagerOpsDataSync` role in your account\. To learn more, see [A new role appeared in my IAM account](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_roles.html#troubleshoot_roles_new-role-appeared)\.

If you delete this service\-linked role, and then need to create it again, you can use the same process to recreate the role in your account\. When you enable Explorer in the AWS Management Console, Systems Manager creates the service\-linked role for you again\. 

You can also use the IAM console to create a service\-linked role with the **AWS Service Role for AWS Systems Manager** use case\. In the AWS CLI or the AWS API, create a service\-linked role with the `ssm.amazonaws.com` service name\. For more information, see [Creating a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\. If you delete this service\-linked role, you can use this same process to create the role again\.

## Editing the `AWSServiceRoleForSystemsManagerOpsDataSync` service\-linked role for Systems Manager<a name="edit-slr-service-action-3"></a>

Systems Manager doesn't allow you to edit the `AWSServiceRoleForSystemsManagerOpsDataSync` service\-linked role\. After you create a service\-linked role, you can't change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting the `AWSServiceRoleForSystemsManagerOpsDataSync` service\-linked role for Systems Manager<a name="delete-slr-service-action-3"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way you don’t have an unused entity that isn't actively monitored or maintained\. However, you must clean up the resources for your service\-linked role before you can manually delete it\.

**Note**  
If the Systems Manager service is using the role when you try to delete the resources, then the deletion might fail\. If that happens, wait for a few minutes and try the operation again\.

The procedure for deleting Systems Manager resources used by the `AWSServiceRoleForSystemsManagerOpsDataSync` role depends on if you've configured Explorer or OpsCenter to integrate with Security Hub\.

**To delete Systems Manager resources used by the `AWSServiceRoleForSystemsManagerOpsDataSync` role**
+ To stop Explorer from creating new OpsItems for Security Hub findings, see [How to stop receiving findings](explorer-securityhub-integration.md#explorer-securityhub-integration-disable-receive)\.
+ To stop OpsCenter from creating new OpsItems for Security Hub findings, see  

**To manually delete the `AWSServiceRoleForSystemsManagerOpsDataSync` service\-linked role using IAM**

Use the IAM console, the AWS CLI, or the AWS API to delete the `AWSServiceRoleForSystemsManagerOpsDataSync` service\-linked role\. For more information, see [Deleting a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported Regions for the Systems Manager`AWSServiceRoleForSystemsManagerOpsDataSync` service\-linked role<a name="slr-regions-service-action-3"></a>

Systems Manager supports using service\-linked roles in all of the Regions where the service is available\. For more information, see [AWS Systems Manager endpoints and quotas](https://docs.aws.amazon.com/general/latest/gr/ssm.html)\.

Systems Manager doesn't support using service\-linked roles in every Region where the service is available\. You can use the `AWSServiceRoleForSystemsManagerOpsDataSync` role in the following Regions\.


****  

| AWS Region name | Region identity | Support in Systems Manager | 
| --- | --- | --- | 
| US East \(N\. Virginia\) | us\-east\-1 | Yes | 
| US East \(Ohio\) | us\-east\-2 | Yes | 
| US West \(N\. California\) | us\-west\-1 | Yes | 
| US West \(Oregon\) | us\-west\-2 | Yes | 
| Asia Pacific \(Mumbai\) | ap\-south\-1 | Yes | 
| Asia Pacific \(Osaka\) | ap\-northeast\-3 | Yes | 
| Asia Pacific \(Seoul\) | ap\-northeast\-2 | Yes | 
| Asia Pacific \(Singapore\) | ap\-southeast\-1 | Yes | 
| Asia Pacific \(Sydney\) | ap\-southeast\-2 | Yes | 
| Asia Pacific \(Tokyo\) | ap\-northeast\-1 | Yes | 
| Canada \(Central\) | ca\-central\-1 | Yes | 
| Europe \(Frankfurt\) | eu\-central\-1 | Yes | 
| Europe \(Ireland\) | eu\-west\-1 | Yes | 
| Europe \(London\) | eu\-west\-2 | Yes | 
| Europe \(Paris\) | eu\-west\-3 | Yes | 
| Europe \(Stockholm\) | eu\-north\-1 | Yes | 
| South America \(São Paulo\) | sa\-east\-1 | Yes | 
| AWS GovCloud \(US\) | us\-gov\-west\-1 | No | 