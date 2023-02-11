# Receiving findings from AWS Security Hub in Explorer<a name="explorer-securityhub-integration"></a>

[AWS Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html) provides you with a comprehensive view of your security state in AWS and helps you to check your environment against security industry standards and best practices\. Security Hub collects security data from across AWS accounts, services, and supported third\-party partner products and helps you to analyze your security trends and identify the highest priority security issues\.

Explorer, a capability of AWS Systems Manager, integration with Security Hub allows you to receive findings from Security Hub in Explorer\. Security Hub findings provide security information that you can use in Explorer to aggregate and take action on your security, performance, and operational issues in Systems Manager\. You can view a widget that provides summary of all Security Hub findings based on severity\. 

Turning on Security Hub integration accrues a cost\. Explorer integrates with Systems Manager OpsCenter \(OpsCenter\) to provide Security Hub findings\. There is a cost to use Explorer with OpsCenter\. 

## How Explorer receives findings from Security Hub<a name="explorer-securityhub-integration-receiving-findings"></a>

In Security Hub, security issues are tracked as findings\. Some findings come from issues that are detected by other AWS services or by third\-party partners\. Security Hub also has a set of rules that it uses to detect security issues and generate findings\.

AWS Systems Manager Explorer is one of the AWS services that receives findings from Security Hub\.

### Mechanism for receiving the findings from Security Hub<a name="explorer-securityhub-integration-receive-mechanism"></a>

To receive the findings from Security Hub, Explorer takes advantage of the Security Hub integration with Amazon EventBridge\. Security Hub sends findings to EventBridge, which uses an event rule to send the findings to Explorer\.

### Types of findings that Explorer receives<a name="explorer-securityhub-integration-finding-types-received"></a>

Explorer receives [all findings](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cwe-integration-types.html#securityhub-cwe-integration-types-all-findings) from Security Hub\. You can see all findings based on severity in the Explorer widget when you turn on the Security Hub default settings\. Explorer creates OpsItems for Critical and High security findings by default\. You can configure Explorer to create OpsItems for Medium and Low severity findings\. Explorer doesn't create OpsItems for Informational severity findings\. However, you can view Informational OpsData in the Explorer dashboard from the Security Hub findings summary widget\. Explorer creates OpsData for all findings regardless of severity\. For more information about the severity of Security Hub findings, see [Severity](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cwe-integration-types.html#securityhub-cwe-integration-types-all-findings) in the *AWS Security Hub User Guide*\.

### How long does it take to receive findings from Security Hub?<a name="explorer-securityhub-integration-receive-finding-latency"></a>

When Security Hub creates a new finding, it's usually visible in Explorer within seconds\. 

### Retrying if there is a system outage<a name="explorer-securityhub-integration-retry-receive"></a>

Data from Security Hub is retained in Explorer for up to 7 days to retry if there is a system outage\. Security Hub also refreshes every 12 hours on all Security Hub standard rules\. 

## Turning on and configuring the integration<a name="explorer-securityhub-integration-receive-enable"></a>

This topic describes how to configure Explorer start receiving Security Hub findings\.

**Before you begin**  
Complete the following tasks before you configure Explorer to start receiving Security Hub findings\.
+ Turn on and configure Security Hub\. For more information, see [Setting up Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-settingup.html) in the *AWS Security Hub User Guide*\.
+ Log into the AWS Organizations management account\. Systems Manager requires access to AWS Organizations to create OpsItems from Security Hub findings\. After you log in to the management account, you're prompted to select the **Enable access** button on the Explorer **Configure dashboard** tab, as described in the following procedure\. If you don't log in to the AWS Organizations management account, you can't allow access and Explorer can't create OpsItems from Security Hub findings\.

**To start receiving Security Hub findings**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Explorer**\.

1. Select **Settings**\.

1. Select the **Configure dashboard** tab\.

1. Select **AWS Security Hub**\.

1. Select the **Disabled** slider to turn on **AWS Security Hub**\.

   Critical and High security findings are displayed by default\. To also display Medium and Low security findings, select the **Disabled** slider next to **Medium,Low**\.

1. In the **OpsItems created by Security Hub findings** section, choose **Enable access**\. If you don't see this button, log in to the AWS Organizations management account and return to this page to select the button\.

## How to view findings from Security Hub<a name="explorer-securityhub-integration-view-received-findings"></a>

The following procedure describes how to view Security Hub findings\.

**To view Security Hub findings**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Explorer**\.

1. Find the **AWS Security Hub findings summary** widget\. This displays your Security Hub findings\. You can select a severity level to view a detailed description of the corresponding OpsItem\.

## How to stop receiving findings<a name="explorer-securityhub-integration-disable-receive"></a>

The following procedure describes how to stop receiving Security Hub findings\.

**To stop receiving Security Hub findings**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Explorer**\.

1. Select **Settings**\.

1. Select the **Configure dashboard** tab\.

1. Select the **Enabled** slider to turn off **AWS Security Hub**\.