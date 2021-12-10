# Specify an idle session timeout value<a name="session-preferences-timeout"></a>

Session Manager, a capability of AWS Systems Manager, allows you to specify the amount of time to allow a user to be inactive before a session ends\. By default, sessions time out after 20 minutes of inactivity\. You can modify this setting to specify that a session times out between 1 and 60 minutes of inactivity\.

**To allow idle session timeout \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Session Manager**\.

1. Choose the **Preferences** tab, and then choose **Edit**\.

1. Specify the amount of time to allow a user to be inactive before a session ends in the **minutes** field under **Idle session timeout**\.

1. Choose **Save**\.