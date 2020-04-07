# Running an Automation Workflow by Using an IAM Service Role<a name="automation-walk-security-assume"></a>

The following procedures describe how to use the AWS Systems Manager console, AWS Command Line Interface \(AWS CLI\), and AWS Tools for Windows PowerShell to run an Automation workflow using an AWS Identity and Access Management \(IAM\) service role \(or *assume role*\)\. The service role gives the Automation workflow permission to perform actions on your behalf\. Configuring a service role is useful when you want to restrict permissions and run actions with least privilege\. This is useful, for example, when you want to restrict a user's privileges on a resource, such as an Amazon EC2 instance, but you want to allow the user to run an Automation workflow that performs a specific set of actions\. In this scenario, you can create a service role with elevated privileges and allow the user to run the Automation workflow\.

**Before You Begin**  
Before you complete the following procedures, you must create the IAM service role and configure a trust relationship for Automation\. For more information, see [Task 1: Create a Service Role for Automation](automation-permissions.md#automation-role) and [Task 2: Add a Trust Relationship for Automation](automation-permissions.md#automation-trust2)\.

## Running an Automation Workflow by Using an IAM Service Role \(Console\)<a name="automation-walk-security-assume-console"></a>

The following procedure describes how to use the Systems Manager console to run an Automation workflow that uses an IAM service role \(or *assume role*\)\.

**To run an Automation workflow using a service role**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**, and then choose **Execute automation**\.

1. In the **Automation document** list, choose a document\. Choose one or more options in the **Document categories** pane to filter SSM documents according to their purpose\. To view a document that you own, choose the **Owned by me** tab\. To view a document that is shared with your account, choose the **Shared with me** tab\. To view all documents, choose the **All documents** tab\.
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

## Running an Automation Workflow by Using an IAM Service Role \(Command Line\)<a name="automation-walk-security-assume-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to run an Automation workflow that uses an IAM service role \(or *assume role*\)\.

**To run an Automation workflow using a service role**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or Upgrade AWS Command Line Tools](getting-started-cli.md)\.

1. Run the following command to start an Automation workflow that uses an IAM service role\.

------
#### [ Linux ]

   ```
   aws ssm start-automation-execution \
     --document-name DocumentName \
     --parameters "ParametersRequiredByDocument","AutomationAssumeRole=arn:aws:iam::123456789012:role/AmazonSSMAutomationRole"
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
     --document-name DocumentName ^
     --parameters "ParametersRequiredByDocument","AutomationAssumeRole=arn:aws:iam::123456789012:role/AmazonSSMAutomationRole"
   ```

------
#### [ PowerShell ]

   ```
   Start-SSMAutomationExecution `
     -DocumentName DocumentName `
     -Parameter @{
       "ParametersRequiredByDocument"="ParameterValues";
       "AutomationAssumeRole"="arn:aws:iam::123456789012:role/AmazonSSMAutomationRole"}
   ```

------

   Here is an example using the document `AWS-RestartEC2Instance` to restart the specified EC2 instance using the IAM service role `AmazonSSMAutomationRole`\.

------
#### [ Linux ]

   ```
   aws ssm start-automation-execution \
     --document-name "AWS-RestartEC2Instance" \
     --parameters "InstanceId=i-1234567890abcdef0","AutomationAssumeRole=arn:aws:iam::123456789012:role/AmazonSSMAutomationRole"
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
     --document-name "AWS-RestartEC2Instance" ^
     --parameters "InstanceId=i-1234567890abcdef0","AutomationAssumeRole=arn:aws:iam::123456789012:role/AmazonSSMAutomationRole"
   ```

------
#### [ PowerShell ]

   ```
   Start-SSMAutomationExecution `
     -DocumentName "AWS-RestartEC2Instance" `
     -Parameter @{
       "InstanceId"="i-1234567890abcdef0";
       "AutomationAssumeRole"="arn:aws:iam::123456789012:role/AWS-SystemsManager-AutomationAdministrationRole"}
   ```

------

   The system returns information like the following\.

------
#### [ Linux ]

   ```
   {
       "AutomationExecutionId": "4105a4fc-f944-11e6-9d32-0123456789ab"
   }
   ```

------
#### [ Windows ]

   ```
   {
       "AutomationExecutionId": "4105a4fc-f944-11e6-9d32-0123456789ab"
   }
   ```

------
#### [ PowerShell ]

   ```
   4105a4fc-f944-11e6-9d32-0123456789ab
   ```

------

1. Run the following command to retrieve the status of the Automation workflow\.

------
#### [ Linux ]

   ```
   aws ssm describe-automation-executions \
     --filter "Key=ExecutionId,Values=4105a4fc-f944-11e6-9d32-0123456789ab"
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-automation-executions ^
     --filter "Key=ExecutionId,Values=4105a4fc-f944-11e6-9d32-0123456789ab"
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMAutomationExecutionList | `
     Where {$_.AutomationExecutionId -eq "4105a4fc-f944-11e6-9d32-0123456789ab"}
   ```

------

   The system returns information like the following\.

------
#### [ Linux ]

   ```
   {
       "AutomationExecutionMetadataList": [
           {
               "AutomationExecutionStatus": "InProgress",
               "CurrentStepName": "stopInstances",
               "Outputs": {},
               "DocumentName": "AWS-RestartEC2Instance",
               "AutomationExecutionId": "4105a4fc-f944-11e6-9d32-0123456789ab",
               "DocumentVersion": "1",
               "ResolvedTargets": {
                   "ParameterValues": [],
                   "Truncated": false
               },
               "AutomationType": "Local",
               "Mode": "Auto",
               "ExecutionStartTime": 1564600648.159,
               "CurrentAction": "aws:changeInstanceState",
               "ExecutedBy": "arn:aws:sts::123456789012:assumed-role/Administrator/Admin",
               "LogFile": "",
               "Targets": []
           }
       ]
   }
   ```

------
#### [ Windows ]

   ```
   {
       "AutomationExecutionMetadataList": [
           {
               "AutomationExecutionStatus": "InProgress",
               "CurrentStepName": "stopInstances",
               "Outputs": {},
               "DocumentName": "AWS-RestartEC2Instance",
               "AutomationExecutionId": "4105a4fc-f944-11e6-9d32-0123456789ab",
               "DocumentVersion": "1",
               "ResolvedTargets": {
                   "ParameterValues": [],
                   "Truncated": false
               },
               "AutomationType": "Local",
               "Mode": "Auto",
               "ExecutionStartTime": 1564600648.159,
               "CurrentAction": "aws:changeInstanceState",
               "ExecutedBy": "arn:aws:sts::123456789012:assumed-role/Administrator/Admin",
               "LogFile": "",
               "Targets": []
           }
       ]
   }
   ```

------
#### [ PowerShell ]

   ```
   AutomationExecutionId       : 4105a4fc-f944-11e6-9d32-0123456789ab
   AutomationExecutionStatus   : InProgress
   AutomationType              : Local
   CurrentAction               : aws:changeInstanceState
   CurrentStepName             : startInstances
   DocumentName                : AWS-RestartEC2Instance
   DocumentVersion             : 1
   ExecutedBy                  : arn:aws:sts::123456789012:assumed-role/Administrator/Admin
   ExecutionEndTime            : 1/1/0001 12:00:00 AM
   ExecutionStartTime          : 7/31/2019 7:17:28 PM
   FailureMessage              : 
   LogFile                     : 
   MaxConcurrency              : 
   MaxErrors                   : 
   Mode                        : Auto
   Outputs                     : {}
   ParentAutomationExecutionId : 
   ResolvedTargets             : Amazon.SimpleSystemsManagement.Model.ResolvedTargets
   Target                      : 
   TargetMaps                  : {}
   TargetParameterName         : 
   Targets                     : {}
   ```

------

For more examples of how to use Systems Manager Automation, see [Automation Walkthroughs](automation-walk.md)\. For information about how to get started with Automation, see [Getting Started with Automation](automation-setup.md)\.