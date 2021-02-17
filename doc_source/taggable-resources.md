# Taggable Systems Manager resources<a name="taggable-resources"></a>

You can apply tags to the following Systems Manager resource types\.
+ Documents
+ Maintenance windows
+ Managed instances
+ OpsItems
+ Parameters
+ Patch baselines

Each of these types except OpsItems can be added to a resource group\.

Depending on the resource type, you can use tags to identify which resources should be included in an operation\. For example, you can tag a group of managed instances and then run a maintenance window task that targets only instances with that key\-value pair\.

You can also restrict user access to these resource types by creating IAM policies that specify the tags that a user can access and attaching the policy to user accounts or groups\. The following are a few examples of restricting resource access using tags\. 
+ You can apply a tag to a set of custom Systems Manager documents and then create a user policy that grants access to documents with that tag but no others \(or that prohibits access to only those documents\)\.
+ You can assign tags to OpsItems and then create IAM policies that limit which users or groups have access to view or update those resources\. For example, organization directors could be granted full access to all OpsItems, but software developers and support engineers could be granted access only to the projects or client segments they are responsible for\.
+ You can apply a common tag to resources of all six supported types and create an IAM policy that grants access to only those resources, such as `Key=Project,Value=ProjectA` or `Key=Environment,Value=Development`\. You can even grant access to only resources to which both tag pairs have been assigned\. This lets you, for example, restrict users to working only with resources for ProjectA in the Development environment\.

You can use the Systems Manager Resource Groups console, the console for the supported resource types \(for example, the Maintenance Windows console or OpsCenter console\), the AWS CLI, and the AWS Tools for PowerShell\. You can add tags when you create or update a resource\. For example, you can use the AWS CLI [add\-tags\-to\-resource](https://docs.aws.amazon.com/cli/latest/reference/ssm/add-tags-to-resource.html) command to add tags to any of the supported Systems Manager resource types after they have been created\. You can use the [remove\-tags\-from\-resource](https://docs.aws.amazon.com/cli/latest/reference/ssm/remove-tags-from-resource.html) command to remove them\.