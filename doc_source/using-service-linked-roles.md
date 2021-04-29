# Using service\-linked roles for Systems Manager<a name="using-service-linked-roles"></a>

AWS Systems Manager uses AWS Identity and Access Management \(IAM\) [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Systems Manager\. Service\-linked roles are predefined by Systems Manager and include all the permissions that the service requires to call other AWS services on your behalf\. 

**Topics**
+ [Using roles to collect inventory, run maintenance window tasks, and view OpsData: AWSServiceRoleForAmazonSSM](using-service-linked-roles-service-action-1.md)
+ [Using roles to collect AWS account information for Systems Manager Explorer: AWSServiceRoleForAmazonSSM\_AccountDiscovery](using-service-linked-roles-service-action-2.md)
+ [Using roles to create OpsData and OpsItems for Systems Manager Explorer: AWSServiceRoleForSystemsManagerOpsDataSync](using-service-linked-roles-service-action-3.md)