# AWSConfigRemediation\-DeleteIAMUser<a name="automation-aws-delete-iam-user"></a>

**Description**

The AWSConfigRemediation\-DeleteIAMUser runbook deletes the AWS Identity and Access Management \(IAM\) user you specify\. This automation deletes or detaches the following resources associated with the IAM user:
+ Access keys
+ Attached managed policies
+ Git credentials
+ IAM group memberships
+ IAM user password
+ Inline policies
+ Multi\-factor authentication \(MFA\) devices
+ Signing certificates
+ SSH public keys

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DeleteIAMUser)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ IAMUserId

  Type: String

  Description: \(Required\) The ID of the IAM user you want to delete\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `iam:DeactivateMFADevice`
+ `iam:DeleteAccessKey`
+ `iam:DeleteLoginProfile`
+ `iam:DeleteServiceSpecificCredential`
+ `iam:DeleteSigningCertificate`
+ `iam:DeleteSSHPublicKey`
+ `iam:DeleteVirtualMFADevice`
+ `iam:DeleteUser`
+ `iam:DeleteUserPolicy`
+ `iam:DetachUserPolicy`
+ `iam:GetUser`
+ `iam:ListAttachedUserPolicies`
+ `iam:ListAccessKeys`
+ `iam:ListGroupsForUser`
+ `iam:ListMFADevices`
+ `iam:ListServiceSpecificCredentials`
+ `iam:ListSigningCertificates`
+ `iam:ListSSHPublicKeys`
+ `iam:ListUserPolicies`
+ `iam:ListUsers`
+ `iam:RemoveUserFromGroup`

**Document Steps**
+ aws:executeScript \- Gathers the user name of the IAM user you specify in the `IAMUserId` parameter\.
+ aws:executeScript \- Gathers access keys, certificates, credentials, MFA devices, and SSH keys associated with the IAM user\.
+ aws:executeScript \- Gathers group memberships and policies for the IAM user\.
+ aws:executeScript \- Deletes access keys, certificates, credentials, MFA devices, and SSH keys associated with the IAM user\.
+ aws:executeScript \- Deletes group memberships and policies for the IAM user\.
+ aws:executeScript \- Deletes the IAM user and verifies the user has been deleted\.