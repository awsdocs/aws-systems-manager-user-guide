# Running an Automation Workflow by Using an IAM Service Role<a name="automation-walk-security-assume"></a>

This procedure shows how to run an Automation workflow that runs using an AWS Identity and Access Management \(IAM\) service role \(or *assume* role\)\. The service role gives the Automation workflow permission to perform actions on your behalf\. Configuring a service role is useful when you want restrict permissions and run actions with least privilege\. For example, if you want to restrict a user's privileges on a resource, such as an Amazon EC2 instance, but you want to allow the user to run an Automation workflow that performs a specific set of actions\. In this scenario, you can create a service role with elevated privileges and allow the user to run the Automation workflow\.

**Before You Begin**  
Before you complete the following procedure, you must create the IAM service role and configure a trust relationship for Automation\. For more information, see [Task 1: Create a Service Role for Automation](automation-permissions.md#automation-role) and [Task 2: Add a Trust Relationship for Automation](automation-permissions.md#automation-trust2)\.

**To run the Automation workflow using a service role**

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
This procedure uses the **Simple execution** mode\. However, you can alternatively choose **Rate control**, **Multi\-account and Region**, or **Manual execution** and run the Automation workflow using a service role\.

1. In the **Input parameters** section, specify the required inputs\. In the **Automation Assume Role** box, paste the ARN of the IAM service role\.

1. Choose **Execute**\. The console displays the status of the Automation execution\.

For more examples of how to use Systems Manager Automation, see [Automation Walkthroughs](automation-walk.md)\. For information about how to get started with Automation, see [Getting Started with Automation](automation-setup.md)\.