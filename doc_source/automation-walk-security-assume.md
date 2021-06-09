# Running an automation by using an IAM service role<a name="automation-walk-security-assume"></a>

The following procedures describe how to use the AWS Systems Manager console and AWS Command Line Interface \(AWS CLI\) to run an automation using an AWS Identity and Access Management \(IAM\) service role that is known in this case as an *assume role*\. The service role gives the automation permission to perform actions on your behalf\. Configuring a service role is useful when you want to restrict permissions and run actions with least privilege\. This is useful, for example, when you want to restrict a user's privileges on a resource, such as an Amazon Elastic Compute Cloud \(Amazon EC2\) instance, but you want to allow the user to run an automation that performs a specific set of actions\. In this scenario, you can create a service role with elevated privileges and allow the user to run the automation\.

**Before you begin**  
Before you complete the following procedures, you must create the IAM service role and configure a trust relationship for , a capability of AWS Systems Manager\. For more information, see [Task 1: Create a service role for Automation](automation-permissions.md#automation-role)\.

## Running an automation by using an IAM service role \(console\)<a name="automation-walk-security-assume-console"></a>

The following procedure describes how to use the Systems Manager console to run an automation that uses an IAM service role \(or *assume role*\)\.

**To run an automation using a service role**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**, and then choose **Execute automation**\.

1. In the **Automation document** list, choose a runbook\. Choose one or more options in the **Document categories** pane to filter SSM documents according to their purpose\. To view a runbook that you own, choose the **Owned by me** tab\. To view a runbook that is shared with your account, choose the **Shared with me** tab\. To view all runbooks, choose the **All documents** tab\.
**Note**  
You can view information about a runbook by choosing the runbook name\.

1. In the **Document details** section, verify that **Document version** is set to the version that you want to run\. The system includes the following version options: 
   + **Default version at runtime**: Choose this option if the Automation runbook is updated periodically and a new default version is assigned\.
   + **Latest version at runtime**: Choose this option if the Automation runbook is updated periodically, and you want to run the version that was most recently updated\.
   + **1 \(Default\)**: Choose this option to run the first version of the document, which is the default\.

1. Choose **Next**\.

1. In the **Execution Mode** section, choose **Simple execution**\.
**Note**  
This procedure uses the **Simple execution** mode\. However, you can alternatively choose **Rate control**, **Multi\-account and Region**, or **Manual execution** and run the automation using a service role\.

1. In the **Input parameters** section, specify the required inputs\. For **AutomationAssumeRole**, enter the name of the IAM service role that functions as an assume role\.
**Tip**  
You can select a role from the list, or begin typing the name of a role and select it from the filtered results\.

1. Choose **Execute**\. The console displays the status of the automation\.

## Running an automation by using an IAM service role \(command line\)<a name="automation-walk-security-assume-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux, macOS, or Windows\) or AWS Tools for PowerShell to run an automation that uses an IAM service role that functions in this case as an *assume role*\.

**To run an automation using a service role**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you haven't already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to start an automation that uses an IAM service role\.

------
#### [ Linux & macOS ]

   ```
   aws ssm start-automation-execution \
       --document-name RunbookName \
       --parameters "ParametersRequiredByRunbook","AutomationAssumeRole=arn:aws:iam::123456789012:role/AmazonSSMAutomationRole"
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name RunbookName ^
       --parameters "ParametersRequiredByRunbook","AutomationAssumeRole=arn:aws:iam::123456789012:role/AmazonSSMAutomationRole"
   ```

------
#### [ PowerShell ]

   ```
   Start-SSMAutomationExecution `
       -DocumentName RunbookName `
       -Parameter @{
       "ParametersRequiredByRunbook"="ParameterValues";
       "AutomationAssumeRole"="arn:aws:iam::123456789012:role/AmazonSSMAutomationRole"}
   ```

------

   Here is an example using the runbook `AWS-RestartEC2Instance` to restart the specified EC2 instance using the IAM service role `AmazonSSMAutomationRole`\.

------
#### [ Linux & macOS ]

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
#### [ Linux & macOS ]

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

1. Run the following command to retrieve the status of the automation\.

------
#### [ Linux & macOS ]

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
#### [ Linux & macOS ]

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

For more examples of how to use Systems Manager Automation, see [Automation walkthroughs](automation-walk.md)\. For information about how to get started with Automation, see [Setting up Automation](automation-setup.md)\.