# Integrating Patch Manager with AWS Security Hub<a name="security-hub-integration"></a>

AWS Security Hub provides a comprehensive view of security alerts across AWS\. It aggregates, organizes, and prioritizes your security alerts, or findings, from multiple AWS services and partner tools\. This reduces the effort of collecting and prioritizing security findings across accounts\.

When you integrate Patch Manager with Security Hub, Security Hub monitors the patching posture of your fleets from a security point of view\. You can also run automated, continuous account\-level configuration and compliance checks to identify specific accounts and resources requiring attention\. You can view the security findings across accounts to see the current security and compliance status using the integrated dashboard\. 

Using Amazon CloudWatch Events, you can receive alerts when Security Hub detects that instances in your fleet are out of compliance\.

For more information about Security Hub, see the *AWS Security Hub User Guide*\.

There is a charge to use Security Hub\. For more information, see [Security Hub pricing](https://aws.amazon.com/security-hub/pricing/)\.

## Enabling AWS Security Hub<a name="security-hub-integration-enable"></a>

This procedure applies when Security Hub hasn’t been enabled in the account yet\.

**Prerequisites**  
You must have the required AWS Identity and Access Management \(IAM\) permissions to enable Security Hub\. For information, see [Attaching the required IAM policy to the IAM identity](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-settingup.html) in the *AWS Security Hub User Guide*\.

**To enable AWS Security Hub**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. Choose the **Settings** tab\.

1. Choose **Open Security Hub**\.

1. Continue with the steps in [Enabling Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-settingup.html) in the *AWS Security Hub User Guide*\.

## Adding Patch Manager to Security Hub integration<a name="security-hub-integration-add"></a>

This procedure describes how to integrate Patch Manager and Security Hub when Security Hub is already active but Patch Manager integration is disabled\. You only need to complete this procedure if integration was manually disabled\.

**To add Patch Manager to Security Hub integration**

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. Choose the **Settings** tab\.

1. Under the **Export to Security Hub** section, to the right of **Patch compliance findings are not being exported to Security Hub**, choose **Enable**\.

## Disabling Patch Manager from Security Hub integration<a name="security-hub-integration-disable"></a>

This procedure removes Patch Manager integration with Security Hub\. It doesn’t disable Security Hub itself\.

**To disable Patch Manager from Security Hub integration**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Patch Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Patch Manager**\.

1. Choose the **Settings** tab\.

1. Under the **Export to Security Hub** section, to the right of **Patch compliance findings are not being exported to Security Hub**, choose **Disable**\.

## Reviewing Security Hub findings for patch compliance<a name="security-hub-integration-review"></a>

This procedure describes how to view findings in Security Hub about managed instances in your fleet that are out of patch compliance\.

**To review Security Hub findings for patch compliance**

1. Sign in to the AWS Management Console and open the AWS Security Hub console at [https://console\.aws\.amazon\.com/securityhub/](https://console.aws.amazon.com/securityhub/)\.

1. In the navigation pane, choose **Findings**\.

1. Choose the **Add filters** box\.

1. In the menu, under **Filters**, choose **Product name**\.

1. In the dialog box that opens, choose **is** in the first field and then enter **Patch Manager** in the second field\.

1. Choose **Apply**\.

1. Add any additional filters you want to help narrow down your results\.

1. In the list of results, choose the title of a finding you want more information about\.

   A pane opens on the right side of the screen with more details about the resource, the issue discovered, and a recommended remediation\.
**Important**  
At this time, Security Hub reports the resource type of all managed instances as `EC2 instance`\. This includes on\-premises servers and virtual machines \(VMs\) that you have registered for use with Systems Manager\.

**Related content**
+ [Findings](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings.html) in the *AWS Security Hub User Guide*
+ [Multi\-Account patch compliance with Patch Manager and Security Hub](http://aws.amazon.com/blogs/mt/multi-account-patch-compliance-with-patch-manager-and-security-hub/) in the *AWS Management & Governance Blog*