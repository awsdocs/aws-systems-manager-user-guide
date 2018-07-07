# Using Service\-Linked Roles for Systems Manager<a name="using-service-linked-roles"></a>

AWS Systems Manager uses AWS Identity and Access Management \(IAM\)[ service\-linked roles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Systems Manager\. Service\-linked roles are predefined by Systems Manager and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up Systems Manager easier because you don’t have to manually add the necessary permissions\. Systems Manager defines the permissions of its service\-linked roles, and unless defined otherwise, only Systems Manager can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-Linked Role Permissions for Systems Manager<a name="slr-permissions"></a>

Systems Manager uses the service\-linked role named **AWSServiceRoleForAmazonSSM** – AWS Systems Manager uses this IAM service role to manage AWS resources on your behalf\.

The AWSServiceRoleForAmazonSSM service\-linked role trusts only ssm\.amazonaws\.com to assume this role\. 

Only Systems Manager Inventory requires a service\-linked role\. The role enables the system to collect Inventory metadata from tags and Resource Groups\. 

The AWSServiceRoleForAmazonSSM service\-linked role permissions policy allows Systems Manager to complete the following actions on all related resources:
+ `ssm:CancelCommand`
+ `ssm:GetCommandInvocation`
+ `ssm:ListCommandInvocations`
+ `ssm:ListCommands`
+ `ssm:SendCommand`
+ `ec2:DescribeInstanceAttribute`
+ `ec2:DescribeInstanceStatus`
+ `ec2:DescribeInstances`
+ `resource-groups:ListGroups`
+ `resource-groups:ListGroupResources`
+ `tag:GetResources`

## Creating a Service\-Linked Role for Systems Manager<a name="create-slr"></a>

You can use the IAM console to create a service\-linked role with the **AWS Service Role for AWS Systems Manager** use case\. In the IAM CLI or the IAM API, create a service\-linked role with the `ssm.amazonaws.com ` service name\. For more information, see [Creating a Service\-Linked Role](http://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#create-service-linked-role) in the *IAM User Guide*\. If you delete this service\-linked role, you can use this same process to create the role again\.

## Editing a Service\-Linked Role for Systems Manager<a name="edit-slr"></a>

Systems Manager does not allow you to edit the AWSServiceRoleForAmazonSSM service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a Service\-Linked Role](http://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting a Service\-Linked Role for Systems Manager<a name="delete-slr"></a>

If you no longer need to use a feature or service that requires a service\-linked role, then we recommend that you delete that role\. That way you don’t have an unused entity that is not actively monitored or maintained\. If you delete the service\-linked role used by Systems Manager Inventory, then the Inventory data for tags and Resource Groups will no longer be synchronized\. You must clean up the resources for your service\-linked role before you can manually delete it\.

**Note**  
If the Systems Manager service is using the role when you try to delete the tags or Resource Groups, then the deletion might fail\. If that happens, wait for a few minutes and try the operation again\.

**To delete Systems Manager resources used by the AWSServiceRoleForAmazonSSM**

1. To delete tags, see [Adding and Deleting Tags on an Individual Resource](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html#adding-or-deleting-tags)\.

1. To delete Resource Groups, see [Delete Groups from AWS Resource Groups](https://docs.aws.amazon.com/ARG/latest/userguide/deleting-resource-groups.html)\.

**To manually delete the service\-linked role using IAM**

Use the IAM console, the IAM CLI, or the IAM API to delete the AWSServiceRoleForAmazonSSM service\-linked role\. For more information, see [Deleting a Service\-Linked Role](http://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.