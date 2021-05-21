# Receiving findings from AWS Security Hub in OpsCenter<a name="opscenter-securityhub-integration"></a>

[AWS Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html) provides you with a comprehensive view of your security state in AWS and helps you to check your environment against security industry standards and best practices\. Security Hub collects security data from across Amazon Web Services accounts, services, and supported third\-party partner products and helps you to analyze your security trends and identify the highest priority security issues\.

The AWS Systems Manager OpsCenter integration with Security Hub enables you to receive findings from Security Hub in OpsCenter\. Security Hub findings provide security information that you can use in OpsCenter to aggregate and take action on your security, performance, and operational issues in Systems Manager\. 

You can automatically create operational issues \(OpsItems\) in OpsCenter for diagnosis and remediation of critical and high severity findings\. To help you with the diagnosis, the OpsItem includes relevant information, such as AWS resource ID and type, and findings details\. You can also use Systems Manager Automation runbooks within OpsCenter to run predefined workflows to help remediate common security issues with AWS resources more easily\. 

OpsCenter has bidirectional integration with Security Hub\. When you make updates to OpsItem status and severity fields related to a security finding, those changes are automatically sent to Security Hub to ensure you always see the latest and correct information\. OpsCenter is also integrated with AWS Systems Manager Explorer \(Explorer\)\. In Explorer, you can view a widget that provides summary of all Security Hub findings based on severity\.

## How OpsCenter receives findings from Security Hub<a name="opscenter-securityhub-integration-receiving-findings"></a>

In Security Hub, security issues are tracked as findings\. Some findings come from issues that are detected by other AWS services or by third\-party partners\. Security Hub also has a set of rules that it uses to detect security issues and generate findings\.

AWS Systems Manager OpsCenter is one of the AWS services that receives findings from Security Hub\.

### Mechanism for receiving the findings from Security Hub<a name="opscenter-securityhub-integration-receive-mechanism"></a>

To receive the findings from Security Hub, OpsCenter takes advantage of the Security Hub integration with Amazon EventBridge\. Security Hub sends findings to EventBridge, which uses an event rule to send the findings to OpsCenter\.

### Types of findings that OpsCenter receives<a name="opscenter-securityhub-integration-finding-types-received"></a>

 OpsItems are automatically created for Critical and High severity findings\. You can configure OpsCenter to display Medium and Low severity findings as described later in [Enabling and configuring the integration](#opscenter-securityhub-integration-receive-enable)\. You can configure OpsCenter and Explorer to create OpsItems for all findings except Informational severity findings\. For more information about the severity of Security Hub findings, see [Severity](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cwe-integration-types.html#securityhub-cwe-integration-types-all-findings) in the *AWS Security Hub User Guide*\.

### How long does it take to receive findings from Security Hub?<a name="opscenter-securityhub-integration-receive-finding-latency"></a>

When Security Hub creates a new finding, it is usually visible in OpsCenter within seconds\.

### Retrying if there is a system outage<a name="opscenter-securityhub-integration-retry-receive"></a>

Data from Security Hub is retained in OpsCenter for up to 7 days to retry if there is a system outage\. Security Hub also refreshes every 12 hours on all Security Hub standard rules\. 

## Enabling and configuring the integration<a name="opscenter-securityhub-integration-receive-enable"></a>

To use the integration with Security Hub, you must enable Security Hub\. For information on how to enable Security Hub, see [Setting up Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-settingup.html) in the *AWS Security Hub User Guide*\.

The following procedure describes how to start receiving and configure Security Hub findings\.

**To start receiving Security Hub findings**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Select **OpsItems**\. Then select **Configure sources**\.

1. In the **Security Hub findings** section, select **Edit\.**

1. Select the **Disabled** slider to enable Security Hub findings to automatically create OpsItems\.

   Critical and High security findings create OpsItems by default\. To also create OpsItems for Medium and Low security findings, select the **Disabled** slider next to **Medium,Low**\.

   When you have configured Security Hub integration as you want, select **Save**\.

## How to view findings from Security Hub<a name="opscenter-securityhub-integration-view-received-findings"></a>

The following procedure describes how to view Security Hub findings\.

**To view Security Hub findings through Explorer**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Explorer**\.

1. Navigate to the Security Hub findings widget\. Select the severity you want to view a detailed list view\. The corresponding OpsItem will be displayed\.

## How to stop receiving findings<a name="opscenter-securityhub-integration-disable-receive"></a>

The following procedure describes how to stop receiving Security Hub findings\.

**To stop receiving Security Hub findings**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Select **OpsItems**\. Then select **Configure sources**\.

1. In the **Security Hub findings** section, select **Edit\.**

1. Select the **Enable** slider to disable Security Hub findings from automatically creating OpsItems\. Then select **Save**\.