# Using Identity\-based Policies \(IAM Policies\) for AWS Systems Manager<a name="auth-and-access-control-iam-identity-based-access-control"></a>

The following examples of identity\-based policies demonstrate how an account administrator can attach permissions policies to IAM identities \(that is, users, groups, and roles\) and thereby grant permissions to perform operations on Systems Manager resources\. 

**Important**  
We recommend that you first review the introductory topics that explain the basic concepts and options available to manage access to your Systems Manager resources\. For more information, see [Overview of Managing Access Permissions to Your AWS Systems Manager Resources](auth-and-access-control-iam-access-control-identity-based.md)\.

**Topics**
+ [Permissions Required to Use the AWS Systems Manager Console](#console-permissions)
+ [AWS Managed \(Predefined\) Policies for AWS Systems Manager](#managed-policies)
+ [Customer Managed Policy Examples](#customer-managed-policies)

The following is an example of a permissions policy that allows a user to delete documents with names that begin with **MyDocument\-** in the **us\-west\-2** region:

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "ssm:DeleteDocument"
      ],
      "Resource" : [
        "arn:aws:ssm:us-west-2:123456789012:document:MyDocument-*"
      ]
    }
  ]
}co
```

## Permissions Required to Use the AWS Systems Manager Console<a name="console-permissions"></a>

To use the AWS Systems Manager console, a user must have a minimum set of permissions that allows the user to describe other AWS resources for their AWS account\. To fully use Systems Manager in the Systems Manager console, you must have permissions from the following services:
+ AWS Systems Manager
+ Amazon Elastic Compute Cloud \(Amazon EC2\)
+ AWS Identity and Access Management \(IAM\)

You can grant the required permissions with the following policy statement:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:*",
                "ec2:describeInstances",
                "iam:PassRole",
                "iam:ListRoles"
            ],
            "Resource": "*"
        }
    ]
}
```

If you create an IAM policy that is more restrictive than the minimum required permissions, the console won't function as intended for users with that IAM policy\. To ensure that those users can use the Systems Manager console, also attach the AmazonSSMReadOnlyAccess managed policy to the user, as described in [AWS Managed \(Predefined\) Policies for AWS Systems Manager](#managed-policies)\.

You don't need to allow minimum console permissions for users that are making calls only to the AWS CLI or the Systems Manager API\.

## AWS Managed \(Predefined\) Policies for AWS Systems Manager<a name="managed-policies"></a>

AWS addresses many common use cases by providing standalone IAM policies that are created and administered by AWS\. These AWS *managed policies* grant necessary permissions for common use cases so you can avoid having to investigate which permissions are needed\. For more information, see [AWS Managed Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

The following AWS managed policies, which you can attach to users in your account, are specific to AWS Systems Manager:
+ **AmazonSSMFullAccess ** – User trust policy that grants full access to the Systems Manager API and documents\.
+ **AmazonSSMAutomationRole ** – Service role that provides permissions for the AWS Systems Manager automation service to run activities defined within automation documents\. Assign this policy to administrators and trusted power users\.

   
+ **AmazonSSMReadOnlyAccess** – User trust policy that grants access to Systems Manager read\-only API actions, such as `Get*` and `List*`\.

   
+ **AmazonSSMMaintenanceWindowRole** – Service role for Systems Manager Maintenance Windows\.

   
+ **AmazonEC2RoleforSSM ** – Instance trust policy that enables an instance to communicate with the Systems Manager API\.

You can also create your own custom IAM policies to allow permissions for Systems Manager actions and resources\. You can attach these custom policies to the IAM users or groups that require those permissions\.

**Note**  
In a hybrid environment, you need an additional IAM role that allows servers and VMs to communicate with the Systems Manager service\. This is the IAM service role for Systems Manager\. This role grants AWS Security Token Service \(AWS STS\) *AssumeRole* trust to the Systems Manager service\. The `AssumeRole` action returns a set of temporary security credentials \(consisting of an access key ID, a secret access key, and a security token\)\. You use these temporary credentials to access AWS resources that you might not normally have access to\. For more information, see [Create an IAM Service Role for a Hybrid Environment](sysman-service-role.md) and [AssumeRole](http://docs.aws.amazon.com/STS/latest/APIReference/API_AssumeRole.html) in *[AWS Security Token Service API Reference](http://docs.aws.amazon.com/STS/latest/APIReference/)*\. 

## Customer Managed Policy Examples<a name="customer-managed-policies"></a>

You can create standalone policies that you administer in your own AWS account\. We refer to these as *customer managed policies*\. You can attach these policies to multiple principal entities in your AWS account\. When you attach a policy to a principal entity, you give the entity the permissions that are defined in the policy\. For more information, see [Customer Managed Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#customer-managed-policies) in *[IAM User Guide](http://docs.aws.amazon.com/IAM/latest/UserGuide/)*\.

The following examples of user policies grant permissions for various AWS Systems Manager actions\. Use them to limit the Systems Manager access for your IAM users and roles\. These policies work when performing actions in the Systems Manager API, AWS SDKs, or the AWS CLI\. For users who use the console, you need to grant additional permissions specific to the console\. For more information, see [Permissions Required to Use the AWS Systems Manager Console](#console-permissions)\.

**Note**  
All examples use the US West \(Oregon\) Region \(us\-west\-2\) and contain fictitious account IDs\.

 **Examples** 
+  [Example 1: Allow a User to Perform Systems Manager Operations in a Single Region](#identity-based-policies-example-1) 
+  [Example 2: Allow a User to List Documents for a Single Region](#identity-based-policies-example-2) 

### Example 1: Allow a User to Perform Systems Manager Operations in a Single Region<a name="identity-based-policies-example-1"></a>

The following example grants permissions to perform AWS Systems Manager operations only in the **us\-west\-2** Region:

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "arn:aws:ssm:*"
      ],
      "Resource" : [
        "arn:aws::aws:ssm:us-west-2:111222333444:*"
      ]
    }
  ]
}
```

### Example 2: Allow a User to List Documents for a Single Region<a name="identity-based-policies-example-2"></a>

The following example grants permissions to list all document names that begin with **Update** in the **us\-west\-2** Region:

```
{
  "Version": "2012-10-17",
  "Statement" : [
    {
      "Effect" : "Allow",
      "Action" : [
        "ssm:ListDocuments"
      ],
      "Resource" : [
        "arn:aws:ssm:us-west-2:111222333444:document/Update*"
      ]
    }
  ]
}
```