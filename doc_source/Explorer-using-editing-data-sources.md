# Editing Systems Manager Explorer data sources<a name="Explorer-using-editing-data-sources"></a>

Systems Manager Explorer displays data from the following sources\. 
+ Systems Manager Patch Compliance
+ Amazon EC2
+ OpsCenter OpsItems

To display patch compliance data for your Amazon EC2 instances, you must configure Systems Manager Patch Manager\. To display metadata about your Amazon EC2 instances, you must configure AWS Config configuration recorder\. You can't add additional data sources, but you can configure Explorer to stop displaying either patch compliance data, Amazon EC2 instance metadata, or both\. 

**Note**  
You can't configure Explorer to stop displaying OpsCenter OpsItem data\.

**To stop displaying data from a source**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Explorer**\.

1. Choose **Settings**\.

1. In the **OpsData sources** section, choose **Edit**\.

1. Expand **OpsData sources**\.

1. Clear the check box beside **Systems Manager Patch Compliance** or **Amazon EC2**, or both\.

1. Choose **Save**\.