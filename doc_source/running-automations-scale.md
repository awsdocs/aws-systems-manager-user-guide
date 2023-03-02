# Run automations at scale<a name="running-automations-scale"></a>

With AWS Systems Manager Automation, you can run automations on a fleet of AWS resources by using *targets*\. Additionally, you can control the deployment of the automation across your fleet by specifying a concurrency value and an error threshold\. The concurrency and error threshold features are collectively called *rate controls*\. The concurrency value determines how many resources are allowed to run the automation simultaneously\. Automation also provides an adaptive concurrency mode you can opt in to\. Adaptive concurrency automatically scales your automation quota from 100 concurrently running automations up to 500\. An error threshold determines how many automations are allowed to fail before Systems Manager stops sending the automation to other resources\.

For more information about concurrency and error thresholds, see [Control automations at scale](running-automations-scale-controls.md)\. For more information about targets, see [Mapping targets for an automation](running-automations-map-targets.md)\.

The following procedures show you how to turn on adaptive concurrency, and how to run an automation with targets and rate controls by using the Systems Manager console and AWS Command Line Interface \(AWS CLI\)\.

## Running an automation with targets and rate controls \(console\)<a name="scale-console"></a>

The following procedure describes how to use the Systems Manager console to run an automation with targets and rate controls\.

**To run an automation with targets and rate controls**

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

1. In the **Execution Mode** section, choose **Rate Control**\. You must use this mode or **Multi\-account and Region** if you want to use targets and rate controls\.

1. In the **Targets** section, choose how you want to target the AWS resources where you want to run the Automation\. These options are required\.

   1. Use the **Parameter** list to choose a parameter\. The items in the **Parameter** list are determined by the parameters in the Automation runbook that you selected at the start of this procedure\. By choosing a parameter you define the type of resource on which the Automation workflow runs\. 

   1. Use the **Targets** list to choose how you want to target resources\.

      1. If you chose to target resources by using parameter values, then enter the parameter value for the parameter you chose in the **Input parameters** section\.

      1. If you chose to target resources by using AWS Resource Groups, then choose the name of the group from the **Resource Group** list\.

      1. If you chose to target resources by using tags, then enter the tag key and \(optionally\) the tag value in the fields provided\. Choose **Add**\.

      1. If you want to run an Automation runbook on all instances in the current AWS account and AWS Region, then choose **All instances**\.

1. In the **Input parameters** section, specify the required inputs\. Optionally, you can choose an IAM service role from the **AutomationAssumeRole** list\.
**Note**  
You might not need to choose some of the options in the **Input parameters** section\. This is because you targeted resources by using tags or a resource group\. For example, if you chose the `AWS-RestartEC2Instance` runbook, then you don't need to specify or choose instance IDs in the **Input parameters** section\. The Automation execution locates the instances to restart by using the tags or resource group you specified\. 

1. Use the options in the **Rate control** section to restrict the number of AWS resources that can run the Automation within each account\-Region pair\. 

   In the **Concurrency** section, choose an option: 
   + Choose **targets** to enter an absolute number of targets that can run the Automation workflow simultaneously\.
   + Choose **percentage** to enter a percentage of the target set that can run the Automation workflow simultaneously\.

1. In the **Error threshold** section, choose an option:
   + Choose **errors** to enter an absolute number of errors allowed before Automation stops sending the workflow to other resources\.
   + Choose **percentage** to enter a percentage of errors allowed before Automation stops sending the workflow to other resources\.

1. \(Optional\) Choose a CloudWatch alarm to apply to your automation for monitoring\. To attach a CloudWatch alarm to your automation, the IAM principal that starts the automation must have permission for the `iam:createServiceLinkedRole` action\. For more information about CloudWatch alarms, see [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)\. Note that if your alarm activates, the automation is stopped\. If you use AWS CloudTrail, you will see the API call in your trail\.

1. Choose **Execute**\. 

To view automations started by your rate control automation, in the navigation pane, choose Automation, and then select **Show child automations**\.

## Running an automation with targets and rate controls \(command line\)<a name="scale-cli"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to run an automation with targets and rate controls\.

**To run an automation with targets and rate controls**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you haven't already\.

   For information, see [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) and [Installing the AWS Tools for PowerShell](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-set-up.html)\.

1. Run the following command to view a list of documents\.

------
#### [ Linux & macOS ]

   ```
   aws ssm list-documents
   ```

------
#### [ Windows ]

   ```
   aws ssm list-documents
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMDocumentList
   ```

------

   Note the name of the runbook that you want to use\.

1. Run the following command to view details about the runbook\. Replace the *runbook name* with the name of the runbook whose details you want to view\. Also, note a parameter name \(for example, `InstanceId`\) that you want to use for the `--target-parameter-name` option\. This parameter determines the type of resource on which the automation runs\.

------
#### [ Linux & macOS ]

   ```
   aws ssm describe-document \
       --name runbook name
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-document ^
       --name runbook name
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMDocumentDescription `
       -Name runbook name
   ```

------

1. Create a command that uses the targets and rate control options you want to run\. Replace each *example resource placeholder* with your own information\.

   *Targeting using tags*

------
#### [ Linux & macOS ]

   ```
   aws ssm start-automation-execution \
       --document-name runbook name \
       --targets Key=tag:key name,Values=value \
       --target-parameter-name parameter name \
       --parameters "input parameter name=input parameter value,input parameter 2 name=input parameter 2 value" \
       --max-concurrency 10 \
       --max-errors 25%
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name runbook name ^
       --targets Key=tag:key name,Values=value ^
       --target-parameter-name parameter name ^
       --parameters "input parameter name=input parameter value,input parameter 2 name=input parameter 2 value" ^
       --max-concurrency 10 ^
       --max-errors 25%
   ```

------
#### [ PowerShell ]

   ```
   $Targets = New-Object Amazon.SimpleSystemsManagement.Model.Target
   $Targets.Key = "tag:key name"
   $Targets.Values = "value"
   
   Start-SSMAutomationExecution `
       DocumentName "runbook name" `
       -Targets $Targets `
       -TargetParameterName "parameter name" `
       -Parameter @{"input parameter name"="input parameter value";"input parameter 2 name"="input parameter 2 value"} `
       -MaxConcurrency "10" `
       -MaxError "25%"
   ```

------

   *Targeting using parameter values*

------
#### [ Linux & macOS ]

   ```
   aws ssm start-automation-execution \
       --document-name runbook name \
       --targets Key=ParameterValues,Values=value,value 2,value 3 \
       --target-parameter-name parameter name \
       --parameters "input parameter name=input parameter value" \
       --max-concurrency 10 \
       --max-errors 25%
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name runbook name ^
       --targets Key=ParameterValues,Values=value,value 2,value 3 ^
       --target-parameter-name parameter name ^
       --parameters "input parameter name=input parameter value" ^
       --max-concurrency 10 ^
       --max-errors 25%
   ```

------
#### [ PowerShell ]

   ```
   $Targets = New-Object Amazon.SimpleSystemsManagement.Model.Target
   $Targets.Key = "ParameterValues"
   $Targets.Values = "value","value 2","value 3"
   
   Start-SSMAutomationExecution `
       -DocumentName "runbook name" `
       -Targets $Targets `
       -TargetParameterName "parameter name" `
       -Parameter @{"input parameter name"="input parameter value"} `
       -MaxConcurrency "10" `
       -MaxError "25%"
   ```

------

   *Targeting using AWS Resource Groups*

------
#### [ Linux & macOS ]

   ```
   aws ssm start-automation-execution \
       --document-name runbook name \
       --targets Key=ResourceGroup,Values=Resource group nname \
       --target-parameter-name parameter name \
       --parameters "input parameter name=input parameter value" \
       --max-concurrency 10 \
       --max-errors 25%
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name runbook name ^
       --targets Key=ResourceGroup,Values=Resource group name ^
       --target-parameter-name parameter name ^
       --parameters "input parameter name=input parameter value" ^
       --max-concurrency 10 ^
       --max-errors 25%
   ```

------
#### [ PowerShell ]

   ```
   $Targets = New-Object Amazon.SimpleSystemsManagement.Model.Target
   $Targets.Key = "ResourceGroup"
   $Targets.Values = "Resource group name"
   
   Start-SSMAutomationExecution `
       -DocumentName "runbook name" `
       -Targets $Targets `
       -TargetParameterName "parameter name" `
       -Parameter @{"input parameter name"="input parameter value"} `
       -MaxConcurrency "10" `
       -MaxError "25%"
   ```

------

   *Targeting all Amazon EC2 instances in the current AWS account and AWS Region*

------
#### [ Linux & macOS ]

   ```
   aws ssm start-automation-execution \
       --document-name runbook name \
       --targets "Key=AWS::EC2::Instance,Values=*"  \
       --target-parameter-name instanceId \
       --parameters "input parameter name=input parameter value" \
       --max-concurrency 10 \
       --max-errors 25%
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name runbook name ^
       --targets Key=AWS::EC2::Instance,Values=* ^
       --target-parameter-name instanceId ^
       --parameters "input parameter name=input parameter value" ^
       --max-concurrency 10 ^
       --max-errors 25%
   ```

------
#### [ PowerShell ]

   ```
   $Targets = New-Object Amazon.SimpleSystemsManagement.Model.Target
   $Targets.Key = "AWS::EC2::Instance"
   $Targets.Values = "*"
   
   Start-SSMAutomationExecution `
       -DocumentName "runbook name" `
       -Targets $Targets `
       -TargetParameterName "instanceId" `
       -Parameter @{"input parameter name"="input parameter value"} `
       -MaxConcurrency "10" `
       -MaxError "25%"
   ```

------

   The command returns an execution ID\. Copy this ID to the clipboard\. You can use this ID to view the status of the automation\.

------
#### [ Linux & macOS ]

   ```
   {
       "AutomationExecutionId": "a4a3c0e9-7efd-462a-8594-01234EXAMPLE"
   }
   ```

------
#### [ Windows ]

   ```
   {
       "AutomationExecutionId": "a4a3c0e9-7efd-462a-8594-01234EXAMPLE"
   }
   ```

------
#### [ PowerShell ]

   ```
   a4a3c0e9-7efd-462a-8594-01234EXAMPLE
   ```

------

1. Run the following command to view the automation\. Replace each *automation execution ID* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm describe-automation-executions \
       --filter Key=ExecutionId,Values=automation execution ID
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-automation-executions ^
       --filter Key=ExecutionId,Values=automation execution ID
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMAutomationExecutionList | `
       Where {$_.AutomationExecutionId -eq "automation execution ID"}
   ```

------

1. To view details about the automation progress, run the following command\. Replace each *automation execution ID* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm get-automation-execution \
       --automation-execution-id automation execution ID
   ```

------
#### [ Windows ]

   ```
   aws ssm get-automation-execution ^
       --automation-execution-id automation execution ID
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMAutomationExecution `
       -AutomationExecutionId automation execution ID
   ```

------

   The system returns information like the following\.

------
#### [ Linux & macOS ]

   ```
   {
       "AutomationExecution": {
           "StepExecutionsTruncated": false,
           "AutomationExecutionStatus": "Success",
           "MaxConcurrency": "1",
           "Parameters": {},
           "MaxErrors": "1",
           "Outputs": {},
           "DocumentName": "AWS-StopEC2Instance",
           "AutomationExecutionId": "a4a3c0e9-7efd-462a-8594-01234EXAMPLE",
           "ResolvedTargets": {
               "ParameterValues": [
                   "i-02573cafcfEXAMPLE"
               ],
               "Truncated": false
           },
           "ExecutionEndTime": 1564681619.915,
           "Targets": [
               {
                   "Values": [
                       "DEV"
                   ],
                   "Key": "tag:ENV"
               }
           ],
           "DocumentVersion": "1",
           "ExecutionStartTime": 1564681576.09,
           "ExecutedBy": "arn:aws:sts::123456789012:assumed-role/Administrator/Admin",
           "StepExecutions": [
               {
                   "Inputs": {
                       "InstanceId": "i-02573cafcfEXAMPLE"
                   },
                   "Outputs": {},
                   "StepName": "i-02573cafcfEXAMPLE",
                   "ExecutionEndTime": 1564681619.093,
                   "StepExecutionId": "86c7b811-3896-4b78-b897-01234EXAMPLE",
                   "ExecutionStartTime": 1564681576.836,
                   "Action": "aws:executeAutomation",
                   "StepStatus": "Success"
               }
           ],
           "TargetParameterName": "InstanceId",
           "Mode": "Auto"
       }
   }
   ```

------
#### [ Windows ]

   ```
   {
       "AutomationExecution": {
           "StepExecutionsTruncated": false,
           "AutomationExecutionStatus": "Success",
           "MaxConcurrency": "1",
           "Parameters": {},
           "MaxErrors": "1",
           "Outputs": {},
           "DocumentName": "AWS-StopEC2Instance",
           "AutomationExecutionId": "a4a3c0e9-7efd-462a-8594-01234EXAMPLE",
           "ResolvedTargets": {
               "ParameterValues": [
                   "i-02573cafcfEXAMPLE"
               ],
               "Truncated": false
           },
           "ExecutionEndTime": 1564681619.915,
           "Targets": [
               {
                   "Values": [
                       "DEV"
                   ],
                   "Key": "tag:ENV"
               }
           ],
           "DocumentVersion": "1",
           "ExecutionStartTime": 1564681576.09,
           "ExecutedBy": "arn:aws:sts::123456789012:assumed-role/Administrator/Admin",
           "StepExecutions": [
               {
                   "Inputs": {
                       "InstanceId": "i-02573cafcfEXAMPLE"
                   },
                   "Outputs": {},
                   "StepName": "i-02573cafcfEXAMPLE",
                   "ExecutionEndTime": 1564681619.093,
                   "StepExecutionId": "86c7b811-3896-4b78-b897-01234EXAMPLE",
                   "ExecutionStartTime": 1564681576.836,
                   "Action": "aws:executeAutomation",
                   "StepStatus": "Success"
               }
           ],
           "TargetParameterName": "InstanceId",
           "Mode": "Auto"
       }
   }
   ```

------
#### [ PowerShell ]

   ```
   AutomationExecutionId       : a4a3c0e9-7efd-462a-8594-01234EXAMPLE
   AutomationExecutionStatus   : Success
   CurrentAction               : 
   CurrentStepName             : 
   DocumentName                : AWS-StopEC2Instance
   DocumentVersion             : 1
   ExecutedBy                  : arn:aws:sts::123456789012:assumed-role/Administrator/Admin
   ExecutionEndTime            : 8/1/2019 5:46:59 PM
   ExecutionStartTime          : 8/1/2019 5:46:16 PM
   FailureMessage              : 
   MaxConcurrency              : 1
   MaxErrors                   : 1
   Mode                        : Auto
   Outputs                     : {}
   Parameters                  : {}
   ParentAutomationExecutionId : 
   ProgressCounters            : 
   ResolvedTargets             : Amazon.SimpleSystemsManagement.Model.ResolvedTargets
   StepExecutions              : {i-02573cafcfEXAMPLE}
   StepExecutionsTruncated     : False
   Target                      : 
   TargetLocations             : {}
   TargetMaps                  : {}
   TargetParameterName         : InstanceId
   Targets                     : {tag:Name}
   ```

------
**Note**  
You can also monitor the status of the automation in the console\. In the **Automation executions** list, choose the automation you just ran and then choose the **Execution steps** tab\. This tab shows the status of the automation actions\.