# Troubleshooting Change Manager<a name="change-manager-troubleshooting"></a>

Use the following information to help you troubleshoot problems with Change Manager, a capability of AWS Systems Manager\.

**Topics**
+ [“Group *\{GUID\}* not found” error during change request approvals when using Active Directory \(groups](#change-manager-troubleshooting-sso)

## “Group *\{GUID\}* not found” error during change request approvals when using Active Directory \(groups<a name="change-manager-troubleshooting-sso"></a>

**Problem**: When AWS IAM Identity Center \(successor to AWS Single Sign\-On\) \(IAM Identity Center\) is used for user identity management, a member of an Active Directory group who is granted approval permissions in Change Manager receives a “not authorized” or “group not found” error\.
+ **Solution**: When you select Active Directory groups in IAM Identity Center for access to the AWS Management Console, the system schedules a periodic synchronization that copies information from those Active Directory groups into IAM Identity Center\. This process must complete before users authorized through Active Directory group membership can successfully approve a request\. For more information, see [Connect to your Microsoft AD directory](https://docs.aws.amazon.com/singlesignon/latest/userguide/manage-your-identity-source-ad.html) in the *AWS IAM Identity Center \(successor to AWS Single Sign\-On\) User Guide*\.