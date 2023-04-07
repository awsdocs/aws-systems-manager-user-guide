# How AWS Systems Manager works with IAM<a name="security_iam_service-with-iam"></a>

Before you use AWS Identity and Access Management \(IAM\) to manage access to AWS Systems Manager, you should understand what IAM features are available to use with Systems Manager\. To get a high\-level view of how Systems Manager and other AWS services work with IAM, see [AWS services that work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) in the *IAM User Guide*\.

**Topics**
+ [Systems Manager identity\-based policies](#security_iam_service-with-iam-id-based-policies)
+ [Systems Manager resource\-based policies](#security_iam_service-with-iam-resource-based-policies)
+ [Authorization based on Systems Manager tags](#security_iam_service-with-iam-tags)
+ [Systems Manager IAM roles](#security_iam_service-with-iam-roles)

## Systems Manager identity\-based policies<a name="security_iam_service-with-iam-id-based-policies"></a>

With IAM identity\-based policies, you can specify allowed or denied actions and resources and the conditions under which actions are allowed or denied\. Systems Manager supports specific actions, resources, and condition keys\. To learn about all of the elements that you use in a JSON policy, see [IAM JSON policy elements reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html) in the *IAM User Guide*\.

### Actions<a name="security_iam_service-with-iam-id-based-policies-actions"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Action` element of a JSON policy describes the actions that you can use to allow or deny access in a policy\. Policy actions usually have the same name as the associated AWS API operation\. There are some exceptions, such as *permission\-only actions* that don't have a matching API operation\. There are also some operations that require multiple actions in a policy\. These additional actions are called *dependent actions*\.

Include actions in a policy to grant permissions to perform the associated operation\.

Policy actions in Systems Manager use the following prefix before the action: `ssm:`\. For example, to grant someone permission to create a Systems Manager parameter \(SSM parameter\) with the Systems Manager `PutParameter` API operation, you include the `ssm:PutParameter` action in their policy\. Policy statements must include either an `Action` or `NotAction` element\. Systems Manager defines its own set of actions that describe tasks that you can perform with this service\.

To specify multiple actions in a single statement, separate them with commas as follows:

```
"Action": [
      "ssm:action1",
      "ssm:action2"
```

**Note**  
AWS AppConfig, a capability of AWS Systems Manager, uses the prefix `appconfig:` before actions\.

You can specify multiple actions using wildcards \(\*\)\. For example, to specify all actions that begin with the word `Describe`, include the following action:

```
"Action": "ssm:Describe*"
```



To see a list of Systems Manager actions, see [Actions Defined by AWS Systems Manager](https://docs.aws.amazon.com/service-authorization/latest/reference/list_awssystemsmanager.html#awssystemsmanager-actions-as-permissions) in the *Service Authorization Reference*\.

### Resources<a name="security_iam_service-with-iam-id-based-policies-resources"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Resource` JSON policy element specifies the object or objects to which the action applies\. Statements must include either a `Resource` or a `NotResource` element\. As a best practice, specify a resource using its [Amazon Resource Name \(ARN\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)\. You can do this for actions that support a specific resource type, known as *resource\-level permissions*\.

For actions that don't support resource\-level permissions, such as listing operations, use a wildcard \(\*\) to indicate that the statement applies to all resources\.

```
"Resource": "*"
```



For example, the Systems Manager maintenance window resource has the following ARN format\.

```
arn:aws:ssm:region:account-id:maintenancewindow/window-id
```

To specify the mw\-0c50858d01EXAMPLE maintenance windows in your statement in the US East \(Ohio\) Region, you would use an ARN similar to the following\.

```
"Resource": "arn:aws:ssm:us-east-2:123456789012:maintenancewindow/mw-0c50858d01EXAMPLE"
```

To specify all maintenance windows that belong to a specific account, use the wildcard \(\*\)\.

```
"Resource": "arn:aws:ssm:region:123456789012:maintenancewindow/*"
```

For `Parameter Store` API operations, you can provide or restrict access to all parameters in one level of a hierarchy by using hierarchical names and AWS Identity and Access Management \(IAM\) policies as follows\.

```
"Resource": "arn:aws:ssm:region:123456789012:parameter/Dev/ERP/Oracle/*"
```

Some Systems Manager actions, such as those for creating resources, can't be performed on a specific resource\. In those cases, you must use the wildcard \(\*\)\.

```
"Resource": "*"
```

Some Systems Manager API operations accept multiple resources\. To specify multiple resources in a single statement, separate their ARNs with commas as follows\.

```
"Resource": [
      "resource1",
      "resource2"
```

**Note**  
Most AWS services treat a colon \(:\) or a forward slash \(/\) as the same character in ARNs\. However, Systems Manager requires an exact match in resource patterns and rules\. When creating event patterns, be sure to use the correct ARN characters so that they match the resource's ARN\.

The following table describes the ARN formats for the resource types supported by Systems Manager\. Documents and automation definition resources that are owned by Amazon do not support *account\-id* in their ARN format\.


| Resource type | ARN format | 
| --- | --- | 
| Application \(AWS AppConfig\) | arn:aws:appconfig:region:account\-id:application/application\-id | 
| Association | arn:aws:ssm:region:account\-id:association/association\-id | 
| Automation execution | arn:aws:ssm:region:account\-id:automation\-execution/automation\-execution\-id | 
| Automation definition \(with version subresource\) |  arn:aws:ssm:*region*:*account\-id*:automation\-definition/*automation\-definition\-id*:*version\-id* ![\[Footnote callout 1\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout01.png)  | 
| Configuration profile \(AWS AppConfig\) | arn:aws:appconfig:region:account\-id:application/application\-id/configurationprofile/configurationprofile\-id | 
| Contact \(Incident Manager\) |  arn:aws:ssm\-contacts:*region*:*account\-id*:contact/*contact\-alias*  | 
| Deployment strategy \(AWS AppConfig\) | arn:aws:appconfig:region:account\-id:deploymentstrategy/deploymentstrategy\-id | 
| Document |  arn:aws:ssm:*region*:*account\-id*:document/*document\-name*  | 
| Environment \(AWS AppConfig\) | arn:aws:appconfig:region:account\-id:application/application\-id/environment/environment\-id | 
| Incident |  arn:aws:ssm\-incidents:*region*:*account\-id*:incident\-record/*response\-plan\-name*/*incident\-id*  | 
| Maintenance window |  arn:aws:ssm:*region*:*account\-id*:maintenancewindow/*window\-id*  | 
| Managed node |  arn:aws:ssm:*region*:*account\-id*:managed\-instance/*managed\-node\-id*  | 
| Managed node inventory | arn:aws:ssm:region:account\-id:managed\-instance\-inventory/managed\-node\-id | 
| OpsItem | arn:aws:ssm:region:account\-id:opsitem/OpsItem\-id | 
| Parameter |  A one\-level parameter: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/security_iam_service-with-iam.html) A parameter named with a hierarchical construction: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/security_iam_service-with-iam.html)  | 
| Patch baseline |  arn:aws:ssm:*region*:*account\-id*:patchbaseline/*patch\-baseline\-id*   | 
| Response plan |  arn:aws:ssm\-incidents:*region*:*account\-id*:response\-plan/*response\-plan\-name*  | 
| Session |  arn:aws:ssm:*region*:*account\-id*:session/*session\-id* ![\[Footnote callout 3\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout03.png)  | 
|  All Systems Manager resources  |  arn:aws:ssm:\*  | 
|  All Systems Manager resources owned by the specified AWS account in the specified AWS Region  |  arn:aws:ssm:*region*:*account\-id*:\*  | 

![\[Footnote callout 1\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout01.png) For automation definitions, Systems Manager supports a second\-level resource, *version ID*\. In AWS, these second\-level resources are known as *subresources*\. Specifying a version subresource for an automation definition resource allows you to provide access to certain versions of an automation definition\. For example, you might want to ensure that only the latest version of an automation definition is used in your node management\.

![\[Footnote callout 2\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout02.png) To organize and manage parameters, you can create names for parameters with a hierarchical construction\. With hierarchical construction, a parameter name can include a path that you define by using forward slashes\. You can name a parameter resource with a maximum of fifteen levels\. We suggest that you create hierarchies that reflect an existing hierarchical structure in your environment\. For more information, see [Creating Systems Manager parameters](sysman-paramstore-su-create.md)\.

![\[Footnote callout 3\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/callout03.png) In most cases, the session ID is constructed using the ID of the account user who started the session, plus an alphanumeric suffix\. For example:

```
arn:aws:us-east-2:111122223333:session/JohnDoe-1a2b3c4sEXAMPLE
```

However, if the user ID isn't available, the ARN is constructed this way instead:

```
arn:aws:us-east-2:111122223333:session/session-1a2b3c4sEXAMPLE
```

For more information about the format of ARNs, see [Amazon Resource Names \(ARNs\)](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *Amazon Web Services General Reference*\.

For a list of Systems Manager resource types and their ARNs, see [Resources Defined by AWS Systems Manager](https://docs.aws.amazon.com/service-authorization/latest/reference/list_awssystemsmanager.html#awssystemsmanager-resources-for-iam-policies) in the *Service Authorization Reference*\. To learn with which actions you can specify the ARN of each resource, see [Actions Defined by AWS Systems Manager](https://docs.aws.amazon.com/service-authorization/latest/reference/list_awssystemsmanager.html#awssystemsmanager-actions-as-permissions)\.<a name="policy-conditions"></a>

### Condition keys for Systems Manager<a name="security_iam_service-with-iam-id-based-policies-conditionkeys"></a>

Administrators can use AWS JSON policies to specify who has access to what\. That is, which **principal** can perform **actions** on what **resources**, and under what **conditions**\.

The `Condition` element \(or `Condition` *block*\) lets you specify conditions in which a statement is in effect\. The `Condition` element is optional\. You can create conditional expressions that use [condition operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html), such as equals or less than, to match the condition in the policy with values in the request\. 

If you specify multiple `Condition` elements in a statement, or multiple keys in a single `Condition` element, AWS evaluates them using a logical `AND` operation\. If you specify multiple values for a single condition key, AWS evaluates the condition using a logical `OR` operation\. All of the conditions must be met before the statement's permissions are granted\.

 You can also use placeholder variables when you specify conditions\. For example, you can grant an IAM user permission to access a resource only if it is tagged with their IAM user name\. For more information, see [IAM policy elements: variables and tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html) in the *IAM User Guide*\. 

AWS supports global condition keys and service\-specific condition keys\. To see all AWS global condition keys, see [AWS global condition context keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.



To see a list of Systems Manager condition keys, see [Condition Keys for AWS Systems Manager](https://docs.aws.amazon.com/service-authorization/latest/reference/list_awssystemsmanager.html#awssystemsmanager-policy-keys) in the *Service Authorization Reference*\. To learn with which actions and resources you can use a condition key, see [Actions Defined by AWS Systems Manager](https://docs.aws.amazon.com/service-authorization/latest/reference/list_awssystemsmanager.html#awssystemsmanager-actions-as-permissions)\.

For information about using the `ssm:resourceTag/*` condition key, see the following topics:
+ [Restricting access to root\-level commands through SSM Agent](ssm-agent-restrict-root-level-commands.md)
+ [Restricting Run Command access based on tags](run-command-setting-up.md#tag-based-access) 
+ [Restrict session access based on instance tags](getting-started-restrict-access-examples.md#restrict-access-example-instance-tags)

For information about using the `ssm:Recursive` and` ssm:Overwrite` condition keys, see [Working with parameter hierarchies](sysman-paramstore-hierarchies.md)\.

### Examples<a name="security_iam_service-with-iam-id-based-policies-examples"></a>



To view examples of Systems Manager identity\-based policies, see [AWS Systems Manager identity\-based policy examples](security_iam_id-based-policy-examples.md)\.

## Systems Manager resource\-based policies<a name="security_iam_service-with-iam-resource-based-policies"></a>

Other AWS services, such as Amazon Simple Storage Service \(Amazon S3\), support resource\-based permissions policies\. For example, you can attach a permissions policy to an S3 bucket to manage access permissions to that bucket\. 

Systems Manager doesn't support resource\-based policies\.

## Authorization based on Systems Manager tags<a name="security_iam_service-with-iam-tags"></a>

You can attach tags to Systems Manager resources or pass tags in a request to Systems Manager\. To control access based on tags, you provide tag information in the [condition element](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html) of a policy using the `ssm:resourceTag/key-name`, `aws:ResourceTag/key-name`, `aws:RequestTag/key-name`, or `aws:TagKeys` condition keys\. You can add tags to the following resource types when you create or update them:
+ Document
+ Managed node
+ Maintenance window
+ Parameter
+ Patch baseline
+ OpsItem

For information about tagging Systems Manager resources, see [Tagging Systems Manager resources](tagging-resources.md)\.

To view an example identity\-based policy for limiting access to a resource based on the tags on that resource, see [Viewing Systems Manager documents based on tags](security_iam_id-based-policy-examples.md#security_iam_id-based-policy-examples-view-documents-tags)\.

## Systems Manager IAM roles<a name="security_iam_service-with-iam-roles"></a>

An [IAM role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) is an entity within your AWS account that has specific permissions\.

### Using temporary credentials with Systems Manager<a name="security_iam_service-with-iam-roles-tempcreds"></a>

You can use temporary credentials to sign in with federation, assume an IAM role, or to assume a cross\-account role\. You obtain temporary security credentials by calling AWS Security Token Service \(AWS STS\) API operations such as [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) or [GetFederationToken](https://docs.aws.amazon.com/STS/latest/APIReference/API_GetFederationToken.html)\.

Systems Manager supports using temporary credentials\. 

### Service\-linked roles<a name="security_iam_service-with-iam-roles-service-linked"></a>

[Service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role) allow AWS services to access resources in other services to complete an action on your behalf\. Service\-linked roles are listed in your IAM account and are owned by the service\. An administrator can view but not edit the permissions for service\-linked roles\.

Systems Manager supports service\-linked roles\. For details about creating or managing Systems Manager service\-linked roles, see [Using service\-linked roles for Systems Manager](using-service-linked-roles.md)\.

### Service roles<a name="security_iam_service-with-iam-roles-service"></a>

This feature allows a service to assume a [service role](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-role) on your behalf\. This role allows the service to access resources in other services to complete an action on your behalf\. Service roles are displayed in your IAM account and are owned by the account\. This means that an administrator can change the permissions for this role\. However, doing so might break the functionality of the service\.

Systems Manager supports service roles\. 

### Choosing an IAM role in Systems Manager<a name="security_iam_service-with-iam-roles-choose"></a>

For Systems Manager to interact with your managed nodes, you must choose a role to allow Systems Manager to access nodes on your behalf\. If you have previously created a service role or service\-linked role, then Systems Manager provides you with a list of roles to choose from\. It's important to choose a role that allows access to start and stop managed nodes\. 

To access EC2 instances, you must configure instance permissions\. For information, see [Configure instance permissions for Systems Manager](setup-instance-permissions.md)\. 

To access on\-premises nodes or virtual machines \(VMs\), the role your AWS account needs is an IAM service role for a hybrid environment\. For information, see [Create an IAM service role for a hybrid environment](sysman-service-role.md)\.

An Automation workflow can be initiated under the context of a service role \(or assume role\)\. This allows the service to perform actions on your behalf\. If you don't specify an assume role, Automation uses the context of the user who invoked the execution\. However, certain situations require that you specify a service role for Automation\. For more information, see [Configuring a service role \(assume role\) access for automations](automation-setup.md#automation-setup-configure-role)\.

### AWS managed policies for AWS Systems Manager<a name="managed-policies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS *managed policies* grant necessary permissions for common use cases so you can avoid having to investigate which permissions are needed\. \(You can also create your own custom IAM policies to allow permissions for Systems Manager actions and resources\.\) For more information, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

The following AWS managed policies, which you can attach to users in your account, are specific to Systems Manager:
+ `AmazonSSMFullAccess` – User trust policy that grants full access to the Systems Manager API and documents\.
+ `AmazonSSMReadOnlyAccess` – User trust policy that grants access to Systems Manager read\-only API operations, such as `Get*` and `List*`\.
+ `AmazonSSMAutomationApproverAccess` – User trust policy that allows access to view automation executions and send approval decisions to automation that is waiting for approval\.
+ `AmazonSSMAutomationRole` – Service role policy that provides permissions for the Systems Manager Automation service to run activities defined within Automation runbooks\. Assign this policy to administrators and trusted power users\.
+ `AmazonSSMDirectoryServiceAccess` – Instance trust policy that allows SSM Agent to access AWS Directory Service on behalf of the user for requests to join the domain by the managed node\.
+ `AmazonSSMMaintenanceWindowRole` – Service role policy for Systems Manager Maintenance Windows\.
+ `AmazonSSMManagedInstanceCore` – Instance trust policy that allows a node to use Systems Manager service core functionality\.
+ `AmazonSSMServiceRolePolicy` – Service role policy that provides access to AWS resources managed or used by Systems Manager\.
+ `AWSResourceAccessManagerServiceRolePolicy` – Service role policy containing read\-only AWS Resource Access Manager access to the account's AWS Organizations structure\. It also contains IAM permissions to self\-delete the role\.
+ `AWSSystemsManagerChangeManagementServicePolicy` – Service policy that provides access to AWS resources managed or used by the Systems Manager change management framework and used by the service\-linked role `AWSServiceRoleForSystemsManagerChangeManagement`\.
+ `AWSSystemsManagerOpsDataSyncServiceRolePolicy` – Service policy that allows the `AWSServiceRoleForSystemsManagerOpsDataSync` service\-linked role to create and update OpsItems and OpsData from AWS Security Hub findings\.
+ `AmazonEC2RoleforSSM` – This policy will be no longer supported\. In its place, use the **AmazonSSMManagedInstanceCore** policy to allow Systems Manager service core functionality on EC2 instances\. For information, see [Configure instance permissions for Systems Manager](setup-instance-permissions.md)\. 

**Note**  
In a hybrid environment, you need an additional IAM role that allows servers and VMs to communicate with the Systems Manager service\. This is the IAM service role for Systems Manager\. This role grants AWS Security Token Service \(AWS STS\) *AssumeRole* trust to the Systems Manager service\. The `AssumeRole` action returns a set of temporary security credentials \(consisting of an access key ID, a secret access key, and a security token\)\. You use these temporary credentials to access AWS resources that you might not normally have access to\. For more information, see [Create an IAM service role for a hybrid environment](sysman-service-role.md) and [AssumeRole](https://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) in the *[AWS Security Token Service API Reference](https://docs.aws.amazon.com/STS/latest/APIReference/)*\. 