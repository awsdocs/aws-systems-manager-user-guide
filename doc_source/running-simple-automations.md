# Run an automation<a name="running-simple-automations"></a>

When you run an automation, by default, the automation runs in the context of the user who initiated the automation\. This means, for example, if your user has administrator permissions, then the automation runs with administrator permissions and full access to the resources being configured by the automation\. As a security best practice, we recommend that you run automation by using an IAM service role that is known in this case as an *assume* role that is configured with the AmazonSSMAutomationRole managed policy\. You might need to add additional IAM policies to your assume role to use various runbooks\. Using an IAM service role to run automation is called *delegated administration*\.

When you use a service role, the automation is allowed to run against the AWS resources, but the user who ran the automation has restricted access \(or no access\) to those resources\. For example, you can configure a service role and use it with Automation to restart one or more Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. Automation is a capability of AWS Systems Manager\. The automation restarts the instances, but the service role doesn't give the user permission to access those instances\.

You can specify a service role at runtime when you run an automation, or you can create custom runbooks and specify the service role directly in the runbook\. If you specify a service role, either at runtime or in a runbook, then the service runs in the context of the specified service role\. If you don't specify a service role, then the system creates a temporary session in the context of the user and runs the automation\.

**Note**  
You must specify a service role for automation that you expect to run longer than 12 hours\. If you start a long\-running automation in the context of a user, the user's temporary session expires after 12 hours\.

Delegated administration ensures elevated security and control of your AWS resources\. It also allows an enhanced auditing experience because actions are being performed against your resources by a central service role instead of multiple IAM accounts\.

**Before you begin**  
Before you complete the following procedures, you must create the IAM service role and configure a trust relationship for Automation, a capability of AWS Systems Manager\. For more information, see [Task 1: Create a service role for Automation](automation-setup-iam.md#create-service-role)\.

The following procedures describe how to use the Systems Manager console or your preferred command line tool to run a simple automation\.

## Running a simple automation \(console\)<a name="simple-console"></a>

The following procedure describes how to use the Systems Manager console to run a simple automation\.

**To run a simple automation**

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

1. In the **Input parameters** section, specify the required inputs\. Optionally, you can choose an IAM service role from the **AutomationAssumeRole** list\.

1. \(Optional\) Choose a CloudWatch alarm to apply to your automation for monitoring\. To attach a CloudWatch alarm to your automation, the IAM principal that starts the automation must have permission for the `iam:createServiceLinkedRole` action\. For more information about CloudWatch alarms, see [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)\. Note that if your alarm activates, the automation is stopped\. If you use AWS CloudTrail, you will see the API call in your trail\. 

1. Choose **Execute**\. 

The console displays the status of the automation\. If the automation fails to run, see [Troubleshooting Systems Manager Automation](automation-troubleshooting.md)\.

## Running a simple automation \(command line\)<a name="simple-cli"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to run a simple automation\.

**To run a simple automation**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you haven't already\.

   For information, see [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) and [Installing the AWS Tools for PowerShell](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-set-up.html)\.

1. Run the following command to start a simple automation\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm start-automation-execution \
       --document-name runbook name \
       --parameters runbook parameters
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name runbook name ^
       --parameters runbook parameters
   ```

------
#### [ PowerShell ]

   ```
   Start-SSMAutomationExecution `
     -DocumentName runbook name `
     -Parameter runbook parameters
   ```

------

   Here is an example using the runbook `AWS-RestartEC2Instance` to restart the specified EC2 instance\.

------
#### [ Linux & macOS ]

   ```
   aws ssm start-automation-execution \
       --document-name "AWS-RestartEC2Instance" \
       --parameters "InstanceId=i-02573cafcfEXAMPLE"
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name "AWS-RestartEC2Instance" ^
       --parameters "InstanceId=i-02573cafcfEXAMPLE"
   ```

------
#### [ PowerShell ]

   ```
   Start-SSMAutomationExecution `
     -DocumentName AWS-RestartEC2Instance `
     -Parameter @{"InstanceId"="i-02573cafcfEXAMPLE"}
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