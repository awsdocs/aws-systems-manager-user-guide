# QuickStart \#1: Run an Automation Workflow as the Current Authenticated User<a name="automation-quickstart-user"></a>

This walkthrough shows you how to run an Automation workflow that restarts a managed instance by using the AWS\-RestartEC2Instance document\. The workflow runs in the context of the current IAM user\. This means that you don't need to configure additional IAM permissions as long as you have permission to run the Automation document and any actions called by the document\. If you have administator permissions in IAM, then you have permission to run this Automation\.

**To run the Automation document as the current authenticated user**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Managed Instances**\.

1. Copy the instance ID of one more managed instances that you want to restart\.

1. In the navigation pane, choose **Automation**, and then choose **Execute automation**\.

1. In the **Automation document** list choose **AWS\-RestartEC2Instance**\.

1. In the **Document details** section, verify that **Document version** is set to **1 \(Default\)**\.

1. Leave the default settings for the **Execution Mode** and **Targets and Rate Control** sections\.

1. In the **Input parameters** section, paste one or more IDs in the **Instance ID** box\. Separate instance IDs with a comma \(,\)\.
**Note**  
You can copy and paste a vertical list of instance IDs \(IDs separated by carriage returns\), because the system automatically separates each instance ID\.

1. Choose **Execute automation**\. The console displays the status of the Automation execution\.