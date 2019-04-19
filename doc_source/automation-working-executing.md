# Executing a Simple Automation Workflow<a name="automation-working-executing"></a>

The following procedure describes how to execute a simple Systems Manager Automation workflow\. The workflow runs in the context of the current AWS Identity and Access Management \(IAM\) user\. This means that you don't need to configure additional IAM permissions as long as you have permission to run the Automation document and any actions called by the document\. If you have administrator permissions in IAM, then you already have permission to run this Automation workflow\.

**Note**  
For information about how to run an Automation workflow that uses an IAM service role or more advanced forms of delegated administration, see [Executing Automation Workflows by Using Different Security Models](automation-walk-security.md)\. 

**To run a simple Automation workflow**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**, and then choose **Execute automation**\.

1. In the **Automation document** list, choose the option beside a document name\. To view more Automation documents, use either the Search bar or the numbers to the right of the Search bar\. 
**Note**  
You can view information about a document by choosing the document name\.

1. In the **Document details** section, verify that **Document version** is set to the version that you want to execute\. The system includes the following version options: 
   + **Default version at runtime**: Choose this option if the Automation document is updated periodically and a new default version is assigned\.
   + **Latest version at runtime**: Choose this option if the Automation document is updated periodically, and you want to execute the version that was most recently updated\.
   + **1 \(Default\)**: Choose this option to execute the first version of the document, which is the default\.

1. Choose **Next**\.

1. In the **Execution Mode** section, choose **Simple execution**\.

1. In the **Input parameters** section, specify the required inputs\. Optionally, you can choose an IAM service role from the **AutomationAssumeRole** list\.

1. Choose **Execute**\. 

The console displays the status of the Automation execution\. If the Automation fails to execute, see [Troubleshooting Systems Manager Automation](automation-troubleshooting.md) for tips to common problems\.