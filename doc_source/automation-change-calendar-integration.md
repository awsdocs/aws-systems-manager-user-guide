# Implement change controls for Automation<a name="automation-change-calendar-integration"></a>

By default, Automation allows you to use runbooks without date and time constraints\. By integrating Automation with Change Calendar, you can implement change controls to all automations in your AWS account\. With this setting, AWS Identity and Access Management \(IAM\) principals in your account can only run automations during the time periods allowed by your change calendar\. To learn more about working with Change Calendar, see [Working with Change Calendar](systems-manager-change-calendar-working.md)\.

**To turn on change controls \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**\.

1. Choose the **Preferences** tab, and then choose **Edit**\.

1. Select the check box next to **Turn on Change Calendar integration**\.

1. In the **Choose a change calendar** dropdown list, choose the change calendar that you want Automation to follow\.

1. Choose **Save**\.