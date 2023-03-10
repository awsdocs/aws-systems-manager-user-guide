# Security Hub<a name="OpsCenter-integrate-with-security-hub"></a>

 AWS Security Hub collects security data from across AWS accounts and services\. Security Hub helps you analyze your security trends to identify and prioritize the security issues across your AWS environment\. Security Hub tracks security issues as *findings*\. Security Hub has a set of rules to detect security issues and generate findings\. Security Hub sends these findings to EventBridge, which uses rules to send findings to OpsCenter\. Using OpsCenter, you can take action to resolve your security, performance, and operational issues in Systems Manager\. 

When you integrate Security Hub with Explorer and OpsCenter, the system creates OpsItems by using the `AWSServiceRoleForAmazonSSM_OpsInsights` IAM service\-linked role\. Security Hub must be activated before you can integrate it with OpsCenter\. For information on how to turn on Security Hub, see [Setting up Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-settingup.html) in the *AWS Security Hub User Guide*\.

When you integrate OpsCenter with AWS Security Hub, the system creates OpsItems for critical and high severity findings\. You can configure OpsCenter to create OpsItems for medium and low severity findings\. OpsCenter doesn’t create an OpsItem for an informational finding because it doesn’t require remediation\. For more information about the severity of Security Hub findings, see [Severity](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cwe-integration-types.html#securityhub-cwe-integration-types-all-findings) in the *AWS Security Hub User Guide*\. 

OpsCenter has bidirectional integration with Security Hub\. When you update the status and severity fields of an OpsItem related to a security finding, the system sends those updates to Security Hub to ensure that information is in sync\. 

**To integrate OpsCenter with Security Hub**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Select **Settings**\.

1. In **Security Hub findings**, choose **Edit\.**

1. Select the **Disabled** slider to allow Security Hub findings to automatically create OpsItems\.

   To create OpsItems for medium and low severity findings, select the sliders next to **Medium severity is disabled** and **Low severity is disabled**\.

1. Choose **Save** to save your configuration\.

**Note**  
OpsCenter retains data from Security Hub for up to 7 days to retry if there is a system outage\. Security Hub also refreshes every 12 hours on all Security Hub standard rules\.

**To stop receiving Security Hub findings**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Select **Settings**\.

1. In **Security Hub findings**, choose **Edit\.**

1. Select the **Enabled** slider to turn off the automatic creation of OpsItems from Security Hub\.

   To create OpsItems for medium and low severity findings, select the sliders next to **Medium severity is disabled** and **Low severity is disabled**\.

1. Choose **Save** to save your configuration\.