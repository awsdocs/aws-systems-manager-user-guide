# Running an Automation Workflow as the Current Authenticated User<a name="automation-walk-security-user"></a>

This procedure shows how to run an Automation workflow that runs in the context of the current IAM user\. This means that you don't need to configure additional IAM permissions as long as you have permission to run the Automation document and any actions called by the document\. If you have administrator permissions in IAM, then you have permission to run this Automation\.

**To run the Automation document as the current authenticated user**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**, and then choose **Execute automation**\.

1. In the **Automation document** list, choose the option next to a document name\. To view more Automation documents, use either the Search bar or the numbers to the right of the Search bar\. 
**Note**  
You can view information about a document by choosing the document name\.

1. In the **Document details** section, verify that **Document version** is set to the version that you want to run\. The system includes the following version options: 
   + **Default version at runtime**: Choose this option if the Automation document is updated periodically and a new default version is assigned\.
   + **Latest version at runtime**: Choose this option if the Automation document is updated periodically, and you want to run the version that was most recently updated\.
   + **1 \(Default\)**: Choose this option to run the first version of the document, which is the default\.

1. Choose **Next**\.

1. In the **Execution Mode** section, choose **Simple execution**\.
**Note**  
This procedure uses the **Simple execution** mode\. However, you can alternatively choose **Rate control** or **Manual execution** and run the Automation workflow as the current authenticated user\.

1. In the **Input parameters** section, specify the required inputs\. To run the Automation workflow as the current authenticated user, do not specify an IAM service role for the value AutomationAssumeRole\.

1. Choose **Execute**\. The console displays the status of the Automation execution\.