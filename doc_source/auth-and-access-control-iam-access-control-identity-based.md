# Overview of Managing Access Permissions to Your AWS Systems Manager Resources<a name="auth-and-access-control-iam-access-control-identity-based"></a>

Every AWS resource is owned by an AWS account, and permissions to create or access a resource are governed by permissions policies\. An account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\)\. Some services—such as AWS Lambda, Amazon Simple Notification Service \(Amazon SNS\), and Amazon Simple Storage Service \(Amazon S3\)—also support attaching permissions policies to resources\. 

**Note**  
An *account administrator* \(or administrator user\) is a user with administrator privileges\. For more information, see [IAM Best Practices](http://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) in the *IAM User Guide*\.

When granting permissions, the account administrator decides who gets the permissions, the resources that they get permissions for, and the specific actions that you want to allow on those resources\.

**Topics**
+ [AWS Systems Manager Resources and Operations](#arn-formats)
+ [Understanding Resource Ownership](#understanding-resource-ownership)
+ [Managing Access to Resources](#managing-access-resources)
+ [Specifying Policy Elements: Resources, Actions, Effects, and Principals](#actions-effects-principals)
+ [Specifying Conditions in a Policy](#policy-conditions)

## AWS Systems Manager Resources and Operations<a name="arn-formats"></a>

Systems Manager includes several primary resources:
+ Automation definition
+ Automation execution
+ Document
+ Maintenance Window
+ Managed instance
+ Managed instance inventory
+ Parameter
+ Patch baseline
+ Resource data sync

For automation definitions, Systems Manager supports a second\-level resource, *version ID*\. In AWS, these second\-level resources are, known as *subresources*\. Specifying a version subresource for an automation definition resource lets you provide access to certain versions of an automation definition\. For example, you might want to ensure that only the latest version of an automation definition is used in your instance management\.

To organize and manage parameters, you can create names for parameters with a hierarchical construction\. With hierachical construction, a parameter name can include a path that you define by using forward slashes\. You can name a parameter resource with a maximum of five levels\. We suggest that you create hierarchies that reflect an existing hierarchical structure in your environment\. For more information, see [Creating Systems Manager Parameters](sysman-paramstore-su-create.md)\.

Each resource has a unique Amazon Resource Names \(ARNs\)\. In a policy, you identify the resource that a policy applies to by using its ARN\. For more information about ARNs, see [Amazon Resource Names \(ARN\) and AWS Service Namespaces](http://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *Amazon Web Services General Reference*\.

The following table shows the structure of the ARN format for each resource type in Systems Manager:


| Resource Type | ARN Format | 
| --- | --- | 
| Automation execution | arn:aws:ssm:region:account\-id:automation\-execution/automation\-execution\-id | 
| Automation definition \(with version subresource\) |  arn:aws:ssm:*region*:*account\-id*:automation\-definition/*automation\-definition\-id*:*version\-id*  | 
| Document |  arn:aws:ssm:*region*:*account\-id*:document/*document\-name*  | 
| Maintenance Window |  arn:aws:ssm:*region*:*account\-id*:maintenancewindow/*window\-execution\-id*  | 
| Maintenance Window task |  arn:aws:ssm:*region*:*account\-id*:windowtask/*window\-task\-id*  | 
| Maintenance Window target |  arn:aws:ssm:*region*:*account\-id*:windowtarget/*window\-target\-id*  | 
| Managed instance |  arn:aws:ssm:*region*:*account\-id*:managed\-instance/*managed\-instance\-id*  | 
| Managed instance inventory | arn:aws:ssm:region:account\-id:managed\-instance\-inventory/managed\-instance\-id | 
| Parameter |  A one\-level parameter: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/auth-and-access-control-iam-access-control-identity-based.html) A parameter named with a hierarchical construction: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/auth-and-access-control-iam-access-control-identity-based.html)  | 
| Patch baseline |  arn:aws:ssm:*region*:*account\-id*:patchbaseline/*patch\-baseline\-id*   | 
|  All Systems Manager resources  |  arn:aws:ssm:\*  | 
|  All Systems Manager resources owned by the specified account in the specified region  |  arn:aws:ssm:*region*:*account\-id*:\*  | 

**Note**  
Most AWS services treat a colon \(:\) or a forward slash \(/\) as the same character in ARNs\. However, Systems Manager requires an exact match in resource patterns and rules\. When creating event patterns, be sure to use the correct ARN characters so that they match the resource's ARN\.

For example, you can indicate a specific document \(*myDocument*\) in your statement using its ARN as follows:

```
"Resource": "arn:aws:ssm:us-west-2:123456789012:document/myDocument"
```

You can specify all documents that belong to a specific account by using the wildcard character \(\*\) as follows:

```
"Resource": "arn:aws:ssm:us-west-2:123456789012:document/*"
```

For `Parameter Store` API actions, you can provide or restrict access to all parameters in one level of a hierarchy by using hierarchical names and AWS Identity and Access Management \(IAM\) policies as follows:

```
"Resource": "arn:aws:ssm:us-west-2:123456789012:parameter/Dev/ERP/Oracle/*"
```

To specify all resources, or when a specific API action does not support ARNs, use the wildcard character \(\*\) in the `Resource` element as follows:

```
"Resource": "*"
```

Some Systems Manager API actions accept multiple resources\. To specify multiple resources in a single statement, separate their ARNs with commas as follows:

```
"Resource": ["arn1", "arn2"]
```

For a list of Systems Manager operations that work with these resource types, see [AWS Systems Manager Permissions Reference](auth-and-access-control-permissions-reference.md)\.

## Understanding Resource Ownership<a name="understanding-resource-ownership"></a>

A *resource owner* is the AWS account that created the resource, regardless of who in the account created the resources\. Specifically, the resource owner is the AWS account of the [principal entity](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html) \(the root account, an IAM user, or an IAM role\) that authenticates the resource creation request\. The following examples illustrate how this works:
+ If you use the root account credentials of your AWS account to create a rule, your AWS account is the owner of the Systems Manager resource\.
+ If you create an IAM user in your AWS account and grant permissions to create Systems Manager resources to that user, the user can create Systems Manager resources\. However, your AWS account, to which the user belongs, owns the Systems Manager resources\.
+ If you create an IAM role in your AWS account with permissions to create Systems Manager resources, anyone who can assume the role can create Systems Manager resources\. Your AWS account, to which the role belongs, owns the Systems Manager resources\.

## Managing Access to Resources<a name="managing-access-resources"></a>

A *permissions policy* describes who has access to what\. The following section explains the available options for creating permissions policies\.

**Note**  
This section discusses using IAM in the context of Systems Manager\. It doesn't provide detailed information about the IAM service\. For complete IAM documentation, see [What Is IAM?](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*\. For information about IAM policy syntax and descriptions, see [AWS IAM Policy Reference](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

Policies attached to an IAM identity are referred to as *identity\-based policies* \(IAM policies\)\. Policies attached to a resource are referred to as *resource\-based policies*\. Systems Manager supports only identity\-based policies\.

**Topics**
+ [Identity\-Based Policies \(IAM Policies\)](#identity-based-policies)
+ [Resource\-Based Policies](#resource-based-policies-overview)

### Identity\-Based Policies \(IAM Policies\)<a name="identity-based-policies"></a>

You can attach policies to IAM identities\. By creating identity\-based IAM policies, you can restrict the calls and resources that users in your account have access to, and then attach those policies to IAM users\. For more information about how to create IAM roles and to explore example IAM policy statements for Systems Manager, see [Overview of Managing Access Permissions to Your AWS Systems Manager Resources](#auth-and-access-control-iam-access-control-identity-based)\. For example, you can do the following: 
+ **Attach a permissions policy to a user or a group in your account** – To grant a user permissions to view applications, deployment groups, and other Systems Manager resources in the AWS Systems Manager console, you can attach a permissions policy to a user or a group that the user belongs to\.
+ **Attach a permissions policy to a role \(grant cross\-account permissions\)** – To grant cross\-account permissions, you can attach an identity\-based permissions policy to an IAM role\. For example, the administrator in Account A can create a role to grant cross\-account permissions to another AWS account \(for example, Account B\) or an AWS service as follows:

   

  1. Account A administrator creates an IAM role and attaches a permissions policy to the role that grants permissions on resources in Account A\.

      

  1. Account A administrator attaches a trust policy to the role identifying Account B as the principal who can assume the role\.

      

  1. Account B administrator can then delegate permissions to assume the role to any users in Account B\. Doing this allows users in Account B to create or access resources in Account A\. If you want to grant an AWS service permissions to assume the role, the principal in the trust policy can also be an AWS service principal\.

      

  For more information about using IAM to delegate permissions, see [Access Management](http://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) in the *IAM User Guide*\.

The types of actions that you can control access to with resource\-based policies vary depending on the resource type, as outlined in the following table:


****  

|  Resource types  |  Action types  | 
| --- | --- | 
|  All  |  View and list details about resources  | 
|  Automation definition  |  Start Stop  | 
|  Document Maintenance Window Parameter  |  Create Delete Update  | 
|  Managed instance  |  Deregister Register  | 
|  Managed instance inventory  |  Create Update  | 
|  Patch baseline  |  Create Delete Deregister Register Update  | 
|  Resource data sync  |  Create Delete  | 

### Resource\-Based Policies<a name="resource-based-policies-overview"></a>

Other AWS services, such as Amazon Simple Storage Service, also support resource\-based permissions policies\. For example, you can attach a permissions policy to an S3 bucket to manage access permissions to that bucket\. Systems Manager doesn't support resource\-based policies\. 

## Specifying Policy Elements: Resources, Actions, Effects, and Principals<a name="actions-effects-principals"></a>

For each Systems Manager resource, Systems Manager defines a set of applicable API operations\. To allow you to grant permissions for these API operations, Systems Manager defines a set of actions that you can specify in a policy\. Some API operations can require permissions for more than one action\. For more information about resources and API operations, see [AWS Systems Manager Resources and Operations](#arn-formats) and [AWS Systems Manager Permissions Reference](auth-and-access-control-permissions-reference.md)\. For a list of actions, see [AWS Systems Manager Resources and Operations](#arn-formats) [Actions](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_Operations.html)\.

The following are the basic policy elements:
+ **Resource** – You use an Amazon Resource Name \(ARN\) to identify the resource that the policy applies to\. For Systems Manager resources, you can, use the wildcard character \(\*\) in IAMpolicies\. For more information, see [AWS Systems Manager Resources and Operations](#arn-formats)\.
+ **Action** – You use action keywords to identify resource operations that you want to allow or deny\. For example, the `ssm:GetDocument` permission allows the user permissions to perform the `GetDocument` operation\.
+ **Effect** – You specify the effect that occurs when the user requests the specific action, either allow or deny\. If you don't explicitly grant access to \(allow\) a resource, access is implicitly denied\. You can also explicitly deny access to a resource, which you might do to make sure that a user cannot access it, even if a different policy grants access\.
+ **Principal** – In identity\-based policies \(IAM policies\), the user that the policy is attached to is the implicit principal\. For resource\-based policies, you specify the user, account, service, or other entity that you want to receive permissions\. Systems Manager supports only identity\-based policies\.

To learn more about IAM policy syntax and descriptions, see [AWS IAM Policy Reference](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) in the *IAM User Guide*\.

For a table showing all of the Systems Manager API actions and the resources that they apply to, see [AWS Systems Manager Permissions Reference](auth-and-access-control-permissions-reference.md)\.

## Specifying Conditions in a Policy<a name="policy-conditions"></a>

When you grant permissions, you can use the language in the access policy to specify the conditions under which a policy should take effect\. For example, you might want a policy to be applied only after a specific date\. For more information about specifying conditions in a policy language, see [Condition](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#Condition) in the *IAM User Guide*\.

To express conditions, you use predefined condition keys\. There are no condition keys specific to AWS Systems Manager\. However, there are AWS\-wide condition keys that you can use as appropriate\. For a complete list of AWS\-wide keys, see [Available Keys for Conditions](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html#AvailableKeys) in the *IAM User Guide*\. 