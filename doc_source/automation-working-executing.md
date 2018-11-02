# Executing an Automation Workflow<a name="automation-working-executing"></a>

The following procedure describes how to execute a Systems Manager Automation workflow\. The workflow runs in the context of the current AWS Identity and Access Management \(IAM\) user\. This means that you don't need to configure additional IAM permissions as long as you have permission to run the Automation document and any actions called by the document\. If you have administrator permissions in IAM, then you already have permission to run this Automation workflow\.

**Note**  
For information about how to run an Automation workflow that uses an IAM service role or more advanced forms of delegated administration, see [Executing Automations by Using Different Security Models](automation-walk-security.md)\. 

**To run an Automation workflow**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**, and then choose **Execute automation**\.

1. In the **Automation document** list, choose the option beside a document name\. Use either the Search bar or the numbers to the right of the Search bar to view more Automation documents\. 
**Note**  
You can view information about a document by choosing the document name\.

1. In the **Document details** section, verify that **Document version** is set to the version that you want to execute\. The system includes the following version options: 
   + **Default version at runtime**: Choose this option if the Automation document is updated periodically and a new default version is assigned\.
   + **Latest version at runtime**: Choose this option if the Automation document is updated periodically, and you want to execute the version that was most recently updated\.
   + **1 Default**: Choose this option to execute the very first, or default, version of the document\.

1. In the **Execution Mode** section, choose an option:
   + **Execute the entire automation at once**: Also called "Auto" mode, choose this option to run the Automation workflow from beginning to end\.
   + **Manually execute single automation steps**: Also called "Interactive" mode, choose this option if you want to initiate each step of the Automation workflow\. The Automation remains in **Waiting** status until you choose the **Execute this step** option for each step in the Automation document\.

1. In the **Targets and Rate Control** section, choose **Enable targets and rate control** if you want to execute the Automation workflow on multiple instances at one time, and you want to choose options that control the workflow execution\.

   For more information about working with targets and rate control features, see [Using Targets and Rate Controls to Execute Automation Workflows on a Fleet](automation-working-targets-and-rate-controls.md)\.

1. In the **Input parameters** section, specify the required inputs\. Optionally, you can choose an IAM service role from the **AutomationAssumeRole** list\.

1. Choose **Execute automation**\. 

The console displays the status of the Automation execution\. If the Automation fails to execute, see [Troubleshooting Systems Manager Automation](automation-troubleshooting.md) for tips to common problems\.