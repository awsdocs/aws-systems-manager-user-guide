# Running an automation manually<a name="automation-working-executing-manually"></a>

The following procedures describe how to use the AWS Systems Manager console and AWS Command Line Interface \(AWS CLI\) to run an automation using the manual execution mode\. By using the manual execution mode, the automation starts in a *Waiting* status and pauses in the *Waiting* status between each step\. This allows you to control when the automation proceeds, which is useful if you need to review the result of a step before continuing\.

The automation runs in the context of the current AWS Identity and Access Management \(IAM\) user\. This means that you don't need to configure additional IAM permissions as long as you have permission to use the runbook, and any actions called by the runbook\. If you have administrator permissions in IAM, then you already have permission to run this automation\.

**Note**  
For information about how to run an automation that uses an IAM service role or more advanced forms of delegated administration, see [Running automations by using different security models](automation-walk-security.md)\. 

## Running an automation step by step \(console\)<a name="automation-working-executing-manually-console"></a>

The following procedure shows how to use the Systems Manager console to manually run an automation step by step\.

**To run an automation step by step**

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

1. In the **Execution Mode** section, choose **Manual execution**\.

1. In the **Input parameters** section, specify the required inputs\. Optionally, you can choose an IAM service role from the **AutomationAssumeRole** list\.

1. Choose **Execute**\. 

1. Choose **Execute this step** when you're ready to start the first step of the automation\. The automation proceeds with step one and pauses before running any subsequent steps specified in the runbook you chose in step 3 of this procedure\. If the runbook has multiple steps, you must select **Execute this step** for each step for the automation to proceed\. Each time you choose **Execute this step** the action runs\.
**Note**  
The console displays the status of the automation\. If the automation fails to run a step, see [Troubleshooting Systems Manager Automation](automation-troubleshooting.md)\.

1. After you complete all steps specified in the runbook, choose **Complete and view results** to finish the automation and view the results\.

## Running an automation step by step \(command line\)<a name="automation-working-executing-manually-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux, macOS, or Windows\) or AWS Tools for PowerShell to manually run an automation step by step\.

**To run an automation step by step**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you haven't already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to start a manual automation\.

------
#### [ Linux & macOS ]

   ```
   aws ssm start-automation-execution \
       --document-name DocumentName \
       --mode Interactive \
       --parameters ParametersRequiredByDocument
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name DocumentName ^
       --mode Interactive ^
       --parameters ParametersRequiredByDocument
   ```

------
#### [ PowerShell ]

   ```
   Start-SSMAutomationExecution `
       -DocumentName DocumentName `
       -Mode Interactive `
       -Parameter ParametersRequiredByDocument
   ```

------

   Here is an example using the runbook `AWS-RestartEC2Instance` to restart the specified EC2 instance\.

------
#### [ Linux & macOS ]

   ```
   aws ssm start-automation-execution \
       --document-name "AWS-RestartEC2Instance" \
       --mode Interactive \
       --parameters "InstanceId=i-1234567890abcdef0"
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name "AWS-RestartEC2Instance" ^
       --mode Interactive ^
       --parameters "InstanceId=i-1234567890abcdef0"
   ```

------
#### [ PowerShell ]

   ```
   Start-SSMAutomationExecution `
       -DocumentName AWS-RestartEC2Instance `
       -Mode Interactive 
       -Parameter @{"InstanceId"="i-1234567890abcdef0"}
   ```

------

   The system returns information like the following\.

------
#### [ Linux & macOS ]

   ```
   {
       "AutomationExecutionId": "ba9cd881-1b36-4d31-a698-0123456789ab"
   }
   ```

------
#### [ Windows ]

   ```
   {
       "AutomationExecutionId": "ba9cd881-1b36-4d31-a698-0123456789ab"
   }
   ```

------
#### [ PowerShell ]

   ```
   27ba8174-59ae-4e13-8626-0123456789ab
   ```

------

1. Run the following command when you're ready to start the first step of the automation\. The automation proceeds with step one and pauses before running any subsequent steps specified in the runbook you chose in step 1 of this procedure\. If the runbook has multiple steps, you must run the following command for each step for the automation to proceed\.

------
#### [ Linux & macOS ]

   ```
   aws ssm send-automation-signal \
       --automation-execution-id ba9cd881-1b36-4d31-a698-0123456789ab \
       --signal-type StartStep \
       --payload StepName="stopInstances"
   ```

------
#### [ Windows ]

   ```
   aws ssm send-automation-signal ^
       --automation-execution-id ba9cd881-1b36-4d31-a698-0123456789ab ^
       --signal-type StartStep ^
       --payload StepName="stopInstances"
   ```

------
#### [ PowerShell ]

   ```
   Send-SSMAutomationSignal `
       -AutomationExecutionId 27ba8174-59ae-4e13-8626-0123456789ab `
       -SignalType StartStep 
       -Payload @{"StepName"="stopInstances"}
   ```

------

   There is no output if the command succeeds\.

1. Run the following command to retrieve the status of each step execution in the automation\.

------
#### [ Linux & macOS ]

   ```
   aws ssm describe-automation-step-executions \
       --automation-execution-id ba9cd881-1b36-4d31-a698-0123456789ab
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-automation-step-executions ^
       --automation-execution-id ba9cd881-1b36-4d31-a698-0123456789ab
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMAutomationStepExecution `
       -AutomationExecutionId 27ba8174-59ae-4e13-8626-e177cdc11686
   ```

------

   The system returns information like the following\.

------
#### [ Linux & macOS ]

   ```
   {
       "StepExecutions": [
           {
               "StepName": "stopInstances",
               "Action": "aws:changeInstanceState",
               "ExecutionStartTime": 1557167178.42,
               "ExecutionEndTime": 1557167220.617,
               "StepStatus": "Success",
               "Inputs": {
                   "DesiredState": "\"stopped\"",
                   "InstanceIds": "[\"i-1234567890abcdef0\"]"
               },
               "Outputs": {
                   "InstanceStates": [
                       "stopped"
                   ]
               },
               "StepExecutionId": "654243ba-71e3-4771-b04f-0123456789ab",
               "OverriddenParameters": {},
               "ValidNextSteps": [
                   "startInstances"
               ]
           },
           {
               "StepName": "startInstances",
               "Action": "aws:changeInstanceState",
               "ExecutionStartTime": 1557167273.754,
               "ExecutionEndTime": 1557167480.73,
               "StepStatus": "Success",
               "Inputs": {
                   "DesiredState": "\"running\"",
                   "InstanceIds": "[\"i-1234567890abcdef0\"]"
               },
               "Outputs": {
                   "InstanceStates": [
                       "running"
                   ]
               },
               "StepExecutionId": "8a4a1e0d-dc3e-4039-a599-0123456789ab",
               "OverriddenParameters": {}
           }
       ]
   }
   ```

------
#### [ Windows ]

   ```
   {
       "StepExecutions": [
           {
               "StepName": "stopInstances",
               "Action": "aws:changeInstanceState",
               "ExecutionStartTime": 1557167178.42,
               "ExecutionEndTime": 1557167220.617,
               "StepStatus": "Success",
               "Inputs": {
                   "DesiredState": "\"stopped\"",
                   "InstanceIds": "[\"i-1234567890abcdef0\"]"
               },
               "Outputs": {
                   "InstanceStates": [
                       "stopped"
                   ]
               },
               "StepExecutionId": "654243ba-71e3-4771-b04f-0123456789ab",
               "OverriddenParameters": {},
               "ValidNextSteps": [
                   "startInstances"
               ]
           },
           {
               "StepName": "startInstances",
               "Action": "aws:changeInstanceState",
               "ExecutionStartTime": 1557167273.754,
               "ExecutionEndTime": 1557167480.73,
               "StepStatus": "Success",
               "Inputs": {
                   "DesiredState": "\"running\"",
                   "InstanceIds": "[\"i-1234567890abcdef0\"]"
               },
               "Outputs": {
                   "InstanceStates": [
                       "running"
                   ]
               },
               "StepExecutionId": "8a4a1e0d-dc3e-4039-a599-0123456789ab",
               "OverriddenParameters": {}
           }
       ]
   }
   ```

------
#### [ PowerShell ]

   ```
   Action: aws:changeInstanceState
   ExecutionEndTime     : 5/6/2019 19:45:46
   ExecutionStartTime   : 5/6/2019 19:45:03
   FailureDetails       : 
   FailureMessage       : 
   Inputs               : {[DesiredState, "stopped"], [InstanceIds, ["i-1234567890abcdef0"]]}
   IsCritical           : False
   IsEnd                : False
   MaxAttempts          : 0
   NextStep             : 
   OnFailure            : 
   Outputs              : {[InstanceStates, Amazon.Runtime.Internal.Util.AlwaysSendList`1[System.String]]}
   OverriddenParameters : {}
   Response             : 
   ResponseCode         : 
   StepExecutionId      : 8fcc9641-24b7-40b3-a9be-0123456789ab
   StepName             : stopInstances
   StepStatus           : Success
   TimeoutSeconds       : 0
   ValidNextSteps       : {startInstances}
   ```

------

1. Run the following command to complete the automation after all steps specified within the chosen runbook have finished\.

------
#### [ Linux & macOS ]

   ```
   aws ssm stop-automation-execution \
       --automation-execution-id ba9cd881-1b36-4d31-a698-0123456789ab \
       --type Complete
   ```

------
#### [ Windows ]

   ```
   aws ssm stop-automation-execution ^
       --automation-execution-id ba9cd881-1b36-4d31-a698-0123456789ab ^
       --type Complete
   ```

------
#### [ PowerShell ]

   ```
   Stop-SSMAutomationExecution `
       -AutomationExecutionId 27ba8174-59ae-4e13-8626-0123456789ab `
       -Type Complete
   ```

------

   There is no output if the command succeeds\.