# Viewing details of an OpsItem<a name="OpsCenter-working-with-OpsItems-viewing-details"></a>

To get a comprehensive view of an OpsItem, use the **OpsItem details** page in the OpsCenter console\. The **Overview** page displays the following information: 
+ **OpsItems details**– Displays general information for the selected OpsItem\.
+ **Related Resources** – A related resource is the impacted resource or the resource that initiated the event that created the OpsItem\. 
+ **Automation executions in the last 30 days ** – A list of runbooks that ran in last 30 days\. 
+ **Runbooks** – You can choose a runbook from a list of available runbooks\. 
+ **Similar OpsItems** – This is a system\-generated list of OpsItems that might be related or of interest to you\. To generate the list, the system scans the titles and descriptions of all OpsItems and returns OpsItems that use similar words\. 
+ **Operational data** – Operational data is custom data that provides useful reference details about the OpsItem\. For example, you can specify log files, error strings, license keys, troubleshooting tips, or other relevant data\.
+ **Related OpsItems** – You can specify the IDs of OpsItems that are in some way related to the current OpsItem\.
+ **Related Resource Details** – Displays data providers, including Amazon CloudWatch metrics and alarms, AWS CloudTrail logs, and details from AWS Config\.

**To view details of an OpsItem**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose an OpsItem to view its details\.