# AWS\-AttachIAMToInstance<a name="automation-aws-attachiamtoinstance"></a>

**Description**

Attach an AWS Identity and Access Management \(IAM\) role to a managed instance\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWS-AttachIAMToInstance)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ ForceReplace

  Type: Boolean

  Description: \(Optional\) Flag to specify whether to replace the existing IAM profile or not\.

  Default: true
+ InstanceId

  Type: String

  Description: \(Required\) The ID of the instance on which you want to assign an IAM role\.
+ RoleName

  Type: String

  Description: \(Required\) The IAM role name to add to the managed instance\.

**Document Steps**

1. aws:executeAwsApi \- DescribeInstanceProfile \- Find the IAM instance profile attached to the EC2 instance\.

1. aws:branch \- CheckInstanceProfileAssociations \- Check the IAM instance profile attached to the EC2 instance\.

   1. If an IAM instance profile is attached and `ForceReplace` is set to `true`:

      1. aws:executeAwsApi \- DisassociateIamInstanceProfile \- Disassociate the IAM instance profile from the EC2 instance\.

   1. aws:executeAwsApi \- ListInstanceProfilesForRole \- List instance profiles for the IAM role provided\.

   1. aws:branch \- CheckInstanceProfileCreated \- Check if the IAM role provided has an associated instance profile\.

      1. If the IAM role has an associated instance profile:

         1. aws:executeAwsApi \- AttachIAMProfileToInstance \- Attach the IAM instance profile role to the EC2 instance\.

      1. If the IAM role does not have an associated instance profile:

         1. aws:executeAwsApi \- CreateInstanceProfileForRole \- Create an instance profile role for the specified IAM role\.

         1. aws:executeAwsApi \- AddRoleToInstanceProfile \- Attach the instance profile role to the specified IAM role\.

         1. aws:executeAwsApi \- GetInstanceProfile \- Get the instance profile data for the specified IAM role\.

         1. aws:executeAwsApi \- AttachIAMProfileToInstanceWithRetry \- Attach the IAM instance profile role to the EC2 instance\.

**Outputs**

AttachIAMProfileToInstanceWithRetry\.AssociationId

GetInstanceProfile\.InstanceProfileName

GetInstanceProfile\.InstanceProfileArn

AttachIAMProfileToInstance\.AssociationId

ListInstanceProfilesForRole\.InstanceProfileName

ListInstanceProfilesForRole\.InstanceProfileArn