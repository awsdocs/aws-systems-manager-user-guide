# Editing Systems Manager Explorer data sources<a name="Explorer-using-editing-data-sources"></a>

AWS Systems Manager Explorer displays data from the following sources\. You can edit Explorer settings to add or remove data sources: 
+ Amazon Elastic Compute Cloud \(Amazon EC2\)
+ AWS Systems Manager OpsCenter
+ AWS Systems Manager Patch Manager patch compliance
+ AWS Systems Manager State Manager association compliance
+ AWS Trusted Advisor
+ AWS Compute Optimizer
+ AWS Support Center cases
+ AWS Config rule and resource compliance
+ AWS Security Hub findings

**Note**  
To view AWS Support Center cases in Explorer, you must have either an Enterprise or Business account set up with AWS Support\.
You can't configure Explorer to stop displaying OpsCenter OpsItem data\.

**Before you begin**  
Verify that you set up and configured services that populate Explorer widgets with data\. For more information, see [Setting up related services](Explorer-setup-related-services.md)\.

**To edit data sources**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Explorer**\.

1. Choose **Settings**\.

1. In the **OpsData sources** section, choose **Edit**\.

1. Expand **OpsData sources**\.

1. Add or remove one or more sources\.

1. Choose **Save**\.