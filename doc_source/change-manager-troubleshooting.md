# Troubleshooting Change Manager<a name="change-manager-troubleshooting"></a>

Use the following information to help you troubleshoot problems with Change Manager\.

**Topics**
+ [“Group *\{GUID\}* not found” error during change request approvals when using Active Directory \(AD\) groups](#change-manager-troubleshooting-sso)

## “Group *\{GUID\}* not found” error during change request approvals when using Active Directory \(AD\) groups<a name="change-manager-troubleshooting-sso"></a>

**Problem**: When AWS Single Sign\-On \(AWS SSO\) is used for user identity management, a member of an AD group who is granted approval permissions in Change Manager receives a “not authorized” or “group not found” error\.
+ **Solution**: When you select AD groups in AWS SSO for access to the AWS Management Console, the system schedules a periodic synchronization that copies information from those AD groups into AWS SSO\. This process must complete before users authorized through AD group membership can successfully approve a request\. For more information, see [Connect to Your Microsoft Active Directory](https://docs.aws.amazon.com/singlesignon/latest/userguide/manage-your-identity-source-ad.html) in the *AWS Single Sign\-On User Guide*\.