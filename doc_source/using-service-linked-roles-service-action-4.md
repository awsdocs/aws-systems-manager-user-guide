# Using roles to create operational insight OpsItems in Systems Manager OpsCenter: `AWSServiceRoleForAmazonSSM_OpsInsights`<a name="using-service-linked-roles-service-action-4"></a>

AWS Systems Manager uses AWS Identity and Access Management \(IAM\) [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to Systems Manager\. Service\-linked roles are predefined by Systems Manager and include all the permissions that the service requires to call other AWS services on your behalf\. 

A service\-linked role makes setting up Systems Manager easier because you don’t have to manually add the necessary permissions\. Systems Manager defines the permissions of its service\-linked roles, and unless defined otherwise, only Systems Manager can assume its roles\. The defined permissions include the trust policy and the permissions policy, and that permissions policy cannot be attached to any other IAM entity\.

You can delete a service\-linked role only after first deleting their related resources\. This protects your Systems Manager resources because you can't inadvertently remove permission to access the resources\.

For information about other services that support service\-linked roles, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes** in the **Service\-linked role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

## Service\-linked role permissions for Systems Manager operational insight OpsItems<a name="service-linked-role-permissions-service-action-4"></a>

Systems Manager uses the service\-linked role named `AWSServiceRoleForAmazonSSM_OpsInsights`\. AWS Systems Manager uses this IAM service role to create and update operational insight OpsItems in Systems Manager OpsCenter\.

The `AWSServiceRoleForAmazonSSM_OpsInsights` service\-linked role trusts the following services to assume the role:
+ `opsinsights.ssm.amazonaws.com`

The role permissions policy allows Systems Manager to complete the following actions on the specified resources:

```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "AllowCreateOpsItem",
			"Effect": "Allow",
			"Action": [
				"ssm:CreateOpsItem",
				"ssm:AddTagsToResource"
			],
			"Resource": "*"
		},
		{
			"Sid": "AllowAccessOpsItem",
			"Effect": "Allow",
			"Action": [
				"ssm:UpdateOpsItem",
				"ssm:GetOpsItem"
			],
			"Resource": "*",
			"Condition": {
				"StringEquals": {
					"aws:ResourceTag/SsmOperationalInsight": "true"
				}
			}
		}
	]
}
```

You must configure permissions to allow an IAM entity \(such as a user, group, or role\) to create, edit, or delete a service\-linked role\. For more information, see [Service\-linked role permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

## Creating the `AWSServiceRoleForAmazonSSM_OpsInsights` service\-linked role for Systems Manager<a name="create-service-linked-role-service-action-4"></a>

You must create a service\-linked role\. If you enable operational insights by using Systems Manager in the AWS Management Console, you can create the service\-linked role by choosing the **Enable** button\.

## Editing the `AWSServiceRoleForAmazonSSM_OpsInsights` service\-linked role for Systems Manager<a name="edit-service-linked-role-service-action-4"></a>

Systems Manager does not allow you to edit the `AWSServiceRoleForAmazonSSM_OpsInsights` service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Deleting the `AWSServiceRoleForAmazonSSM_OpsInsights` service\-linked role for Systems Manager<a name="delete-service-linked-role-service-action-4"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way you don’t have an unused entity that is not actively monitored or maintained\. However, you must clean up your service\-linked role before you can manually delete it\.

### Cleaning up the `AWSServiceRoleForAmazonSSM_OpsInsights` service\-linked role<a name="service-linked-role-review-before-delete-service-action-4"></a>

Before you can use IAM to delete the `AWSServiceRoleForAmazonSSM_OpsInsights` service\-linked role, you must first disable operational insights in Systems Manager OpsCenter\. For more information, see [Working with operational insights](OpsCenter-working-deduplication-insights.md)\.

### Manually delete the `AWSServiceRoleForAmazonSSM_OpsInsights` service\-linked role<a name="slr-manual-delete-service-action-4"></a>

Use the IAM console, the AWS CLI, or the AWS API to delete the `AWSServiceRoleForAmazonSSM_OpsInsights` service\-linked role\. For more information, see [Deleting a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

## Supported Regions for the Systems Manager`AWSServiceRoleForAmazonSSM_OpsInsights` service\-linked role<a name="slr-regions-service-action-4"></a>

Systems Manager does not support using service\-linked roles in every Region where the service is available\. You can use the AWSServiceRoleForAmazonSSM\_OpsInsights role in the following Regions\.


****  

| Region name | Region identity | Support in Systems Manager | 
| --- | --- | --- | 
| US East \(N\. Virginia\) | us\-east\-1 | Yes | 
| US East \(Ohio\) | us\-east\-2 | Yes | 
| US West \(N\. California\) | us\-west\-1 | Yes | 
| US West \(Oregon\) | us\-west\-2 | Yes | 
| Asia Pacific \(Mumbai\) | ap\-south\-1 | Yes | 
| Asia Pacific \(Tokyo\) | ap\-northeast\-1 | Yes | 
| Asia Pacific \(Seoul\) | ap\-northeast\-2 | Yes | 
| Asia Pacific \(Singapore\) | ap\-southeast\-1 | Yes | 
| Asia Pacific \(Sydney\) | ap\-southeast\-2 | Yes | 
| Asia Pacific \(Hong Kong\) | ap\-east\-1 | Yes | 
| Canada \(Central\) | ca\-central\-1 | Yes | 
| Europe \(Frankfurt\) | eu\-central\-1 | Yes | 
| Europe \(Ireland\) | eu\-west\-1 | Yes | 
| Europe \(London\) | eu\-west\-2 | Yes | 
| Europe \(Paris\) | eu\-west\-3 | Yes | 
| Europe \(Stockholm\) | eu\-north\-1 | Yes | 
| Europe \(Milan\) | eu\-south\-1 | Yes | 
| South America \(São Paulo\) | sa\-east\-1 | Yes | 
| Middle East \(Bahrain\) | me\-south\-1 | Yes | 
| Africa \(Cape Town\) | af\-south\-1 | Yes | 
| AWS GovCloud \(US\) | us\-gov\-west\-1 | Yes | 
| AWS GovCloud \(US\) | us\-gov\-east\-1 | Yes | 