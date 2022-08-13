# Using service\-linked roles for Systems Manager<a name="using-service-linked-roles"></a>

AWS Systems Manager uses AWS Identity and Access Management \(IAM\) [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Systems Manager\. Service\-linked roles are predefined by Systems Manager and include all the permissions that the service requires to call other AWS services on your behalf\.

**Note**  
A *service role* role differs from a service\-linked role\. A service role is a type of AWS Identity and Access Management \(IAM\) role that grants permissions to an AWS service so that the service can access AWS resources\. Only a few Systems Manager scenarios require a service role\. When you create a service role for Systems Manager, you choose the permissions to grant so that it can access or interact with other AWS resources\.

You can use the Systems Manager service\-linked role `AWSServiceRoleforAmazonSSM` for the following:
+ The Systems Manager Inventory capability uses the service\-linked role `AWSServiceRoleforAmazonSSM` to collect inventory metadata from tags and resource groups\.
+ The Explorer capability uses the service\-linked role `AWSServiceRoleforAmazonSSM` to enable viewing OpsData and OpsItems from multiple accounts\. This service\-linked role also allows Explorer to create a managed rule when you enable Security Hub as a data source from Explorer or OpsCenter\.

**Topics**
+ [Using roles to collect inventory and view OpsData: `AWSServiceRoleForAmazonSSM`](using-service-linked-roles-service-action-1.md)
+ [Using roles to collect AWS account information for Systems Manager Explorer: `AWSServiceRoleForAmazonSSM_AccountDiscovery`](using-service-linked-roles-service-action-2.md)
+ [Using roles to create OpsData and OpsItems for Systems Manager Explorer: `AWSServiceRoleForSystemsManagerOpsDataSync`](using-service-linked-roles-service-action-3.md)
+ [Using roles to create operational insight OpsItems in Systems Manager OpsCenter: `AWSServiceRoleForAmazonSSM_OpsInsights`](using-service-linked-roles-service-action-4.md)