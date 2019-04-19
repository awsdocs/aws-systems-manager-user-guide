# Executing an Automation Workflow Step by Step<a name="automation-working-executing-manually"></a>

The following procedure describes how to execute a Systems Manager Automation workflow using the manual execution mode\. By using the manual execution mode, the automation workflow starts in a *Waiting* status and pauses in the *Waiting* status between each step\. This allows you to control when the workflow proceeds, which is useful if you need to review the result of a step before continuing\.

The workflow runs in the context of the current AWS Identity and Access Management \(IAM\) user\. This means that you don't need to configure additional IAM permissions as long as you have permission to run the Automation document and any actions called by the document\. If you have administrator permissions in IAM, then you already have permission to run this Automation workflow\.

**Note**  
For information about how to run an Automation workflow that uses an IAM service role or more advanced forms of delegated administration, see [Executing Automation Workflows by Using Different Security Models](automation-walk-security.md)\. 

**To run an Automation workflow step by step**

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

1. In the **Execution Mode** section, choose **Manual execution**\.

1. In the **Input parameters** section, specify the required inputs\. Optionally, you can choose an IAM service role from the **AutomationAssumeRole** list\.

1. Choose **Execute**\. 

1. Choose **Execute this step** when you are ready to start the first step of the automation workflow\. The automation workflow proceeds with step one and pauses before executing any subsequent steps specified in the Automation document you chose in step 3 of this procedure\. If the document has multiple steps, you must select **Execute this step** for each step for the workflow to proceed\.
**Note**  
The console displays the status of the Automation execution\. If the Automation fails to execute a step, see [Troubleshooting Systems Manager Automation](automation-troubleshooting.md) for tips to common problems\.

1. After you complete all steps specified in the Automation document, choose **Complete and view results** to finish the automation workflow and view the results\.