# QuickStart \#2: Run an Automation Workflow by Using an IAM Service Role<a name="automation-quickstart-assume"></a>

This walkthrough shows you how to run an Automation workflow that restarts a managed instance by using the AWS\-RestartEC2Instance document\. The workflow runs the Automation by using an IAM service role \(or *assume* role\. The service role gives the Automation service permission to perform actions on your behalf\. Configuring a service role is useful when you want restrict permissions and run actions with least privilege\. For example, if you want to restrict a user's privileges on a resource, such as an EC2 instance, but you want the user to run an Automation workflow that performs a specific and allowable set of actions\. In this scenario, you can create a service role with higher privileges and allow the user to run the Automation workflow\.

**Before You Begin**  
Before you complete the following procedure, you must create the IAM service role and configure a trust relationship for Automation\. For more information, see the following procedures: [Task 1: Create a Service Role for Automation](automation-permissions.md#automation-role) and [Task 2: Add a Trust Relationship for Automation](automation-permissions.md#automation-trust2)\.

**To run the Automation document by using a service role**

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

1. In the **Automation Assume Role** box, paste the ARN of the IAM service role\.

1. Choose **Execute automation**\. The console displays the status of the Automation execution\.

For more examples of how use Systems Manager Automation, see [Systems Manager Automation Walkthroughs](automation-walk.md)\. For information about how to get started with Automation, see [Setting Up Automation](automation-setup.md)\.