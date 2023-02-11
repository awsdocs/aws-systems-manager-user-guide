# Viewing OpsCenter logs and reports<a name="OpsCenter-logging-auditing"></a>

AWS CloudTrail logs AWS Systems Manager OpsCenter API calls to the console, the AWS Command Line Interface \(AWS CLI\), and the SDK\. You can view the information in the CloudTrail console or in an Amazon Simple Storage Service \(Amazon S3\) bucket\. Amazon S3 uses one bucket to store all CloudTrail logs for your account\.

Logs of OpsCenter actions show create, update, get, and describe OpsItem activities\. For more information about viewing and using CloudTrail logs of Systems Manager activity, see [Logging AWS Systems Manager API calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)\.

AWS Systems Manager OpsCenter provides you with the following information about OpsItems:
+ **OpsItem status summary** – Provides a summary of OpsItems by status \(Open and In progress, Open, or In Progress\)\.
+ **Sources with most open OpsItems** – Provides a breakdown of the top AWS services with open OpsItems\.
+ **OpsItems by source and age** – Provides a count of OpsItems, grouped by source and days since creation\.

**To view the OpsCenter summary report**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. On the OpsItems **Overview** page, choose **Summary**\.

1. Under **OpsItems by source and age**, choose the Search bar to filter OpsItems according to **Source**\. Use the list to filter according to **Status**\.