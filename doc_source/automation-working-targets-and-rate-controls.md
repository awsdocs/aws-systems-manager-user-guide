# Running automations that use targets and rate controls<a name="automation-working-targets-and-rate-controls"></a>

AWS Systems Manager enables you to run automations on a fleet of AWS resources by using targets\. Additionally, you can control the deployment of the automation across your fleet by specifying a concurrency value and an error threshold\. The concurrency value determines how many resources are allowed to run the automation simultaneously\. An error threshold determines how many automations are allowed to fail before Systems Manager stops sending the workflow to other resources\. The concurrency and error threshold features are collectively called *rate controls*\. 

For more information about concurrency and error thresholds, see [About concurrency and error thresholds](automation-working-rate-controls.md)\. For more information about targets, see [About targets](automation-working-targets.md)\.

The following procedures describe how to run an automation with targets and rate controls by using the AWS Systems Manager console, AWS Command Line Interface \(AWS CLI\), and AWS Tools for Windows PowerShell\.

## Running an automation with targets and rate controls \(console\)<a name="automation-working-targets-and-rate-controls-console"></a>

The following procedure describes how to use the Systems Manager console to run an automation with targets and rate controls\.

**To run an automation with targets and rate controls**

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

1. In the **Execution Mode** section, choose **Rate Control**\. You must use this mode or **Multi\-account and Region** if you want to use targets and rate controls\.

1. In the **Targets** section, choose how you want to target the AWS Resources where you want to run the Automation\. These options are required\.

   1. Use the **Parameter** list to choose a parameter\. The items in the **Parameter** list are determined by the parameters in the Automation document that you selected at the start of this procedure\. By choosing a parameter you define the type of resource on which the Automation workflow runs\. 

   1. Use the **Targets** list to choose how you want to target resources\.

      1. If you chose to target resources by using parameter values, then enter the parameter value for the parameter you chose in the **Input parameters** section\.

      1. If you chose to target resources by using AWS Resource Groups, then choose the name of the group from the **Resource Group** list\.

      1. If you chose to target resources by using tags, then enter the tag key and \(optionally\) the tag value in the fields provided\. Choose **Add**\.

      1. If you want to run an Automation playbook on all instances in the current AWS account and Region, then choose **All instances**\.

1. In the **Input parameters** section, specify the required inputs\. Optionally, you can choose an IAM service role from the **AutomationAssumeRole** list\.
**Note**  
You may not need to choose some of the options in the **Input parameters** section\. This is because you targeted resources by using tags or a resource group\. For example, if you chose the AWS\-RestartEC2Instance document, then you don't need to specify or choose instance IDs in the **Input parameters** section\. The Automation execution locates the instances to restart by using the tags or Resource Group you specified\. 

1. Use the options in the **Rate control** section to restrict the number of AWS resources that can run the Automation within each account\-Region pair\. 

   In the **Concurrency** section, choose an option: 
   + Choose **targets** to enter an absolute number of targets that can run the Automation workflow simultaneously\.
   + Choose **percentage** to enter a percentage of the target set that can run the Automation workflow simultaneously\.

1. In the **Error threshold** section, choose an option:
   + Choose **errors** to enter an absolute number of errors allowed before Automation stops sending the workflow to other resources\.
   + Choose **percentage** to enter a percentage of errors allowed before Automation stops sending the workflow to other resources\.

1. Choose **Execute**\. 

To view automations started by your rate control automation, in the navigation pane, choose Automation, and then select **Show child automations**\.

## Running an automation with targets and rate controls \(command line\)<a name="automation-working-targets-and-rate-controls-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to run an automation with targets and rate controls\.

**To run an automation with targets and rate controls**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to view a list of documents\.

------
#### [ Linux ]

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

   Note the name of the Automation document that you want to run\.

1. Run the following command to view details about the Automation document\. Note a parameter name \(for example, `InstanceId`\) that you want to use for the `--target-parameter-name` option\. This parameter determines the type of resource on which the automation runs\.

------
#### [ Linux ]

   ```
   aws ssm describe-document \
       --name document_name
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-document ^
       --name document_name
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMDocumentDescription `
       -Name document_name
   ```

------

1. Create a command that uses the targets and rate control options you want to run\. Here are some template commands to help\.

   *Targeting using tags*

------
#### [ Linux ]

   ```
   aws ssm start-automation-execution \
       --document-name document_name \
       --targets Key=tag:key_name,Values=value \
       --target-parameter-name parameter_name \
       --parameters "input_parameter_name1=input_parameter_value1,input_parameter_name2=input_parameter_value2" \
       --max-concurrency 10 \
       --max-errors 25%
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name document_name ^
       --targets Key=tag:key_name,Values=value ^
       --target-parameter-name parameter_name ^
       --parameters "input_parameter_name1=input_parameter_value1,input_parameter_name2=input_parameter_value2" ^
       --max-concurrency 10 ^
       --max-errors 25%
   ```

------
#### [ PowerShell ]

   ```
   $Targets = New-Object Amazon.SimpleSystemsManagement.Model.Target
   $Targets.Key = "tag:key_name"
   $Targets.Values = "value"
   
   Start-SSMAutomationExecution `
       DocumentName "DocumentName" `
       -Targets $Targets `
       -TargetParameterName "Parameter_Name" `
       -Parameter @{"input_parameter_name1"="input_parameter_value1";"input_parameter_name2"="input_parameter_value2"} `
       -MaxConcurrency "a_number_of_instances_or_a_percentage_of_target_set" `
       -MaxError "a_number_of_errors_or_a_percentage_of_target_set"
   ```

------

   *Targeting using parameter values*

------
#### [ Linux ]

   ```
   aws ssm start-automation-execution \
       --document-name document_name \
       --targets Key=ParameterValues,Values=value_1,value_2,value_3 \
       --target-parameter-name parameter_name \
       --parameters "input_parameter_name1=input_parameter_value1" \
       --max-concurrency 10 \
       --max-errors 25%
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name document_name ^
       --targets Key=ParameterValues,Values=value_1,value_2,value_3 ^
       --target-parameter-name parameter_name ^
       --parameters "input_parameter_name1=input_parameter_value1" ^
       --max-concurrency 10 ^
       --max-errors 25%
   ```

------
#### [ PowerShell ]

   ```
   $Targets = New-Object Amazon.SimpleSystemsManagement.Model.Target
   $Targets.Key = "ParameterValues"
   $Targets.Values = "value_1","value_2","value_3"
   
   Start-SSMAutomationExecution `
       -DocumentName "DocumentName" `
       -Targets $Targets `
       -TargetParameterName "Parameter_Name" `
       -Parameter @{"input_parameter_name1"="input_parameter_value1"} `
       -MaxConcurrency "a_number_of_instances_or_a_percentage_of_target_set" `
       -MaxError "a_number_of_errors_or_a_percentage_of_target_set"
   ```

------

   *Targeting using AWS Resource Groups*

------
#### [ Linux ]

   ```
   aws ssm start-automation-execution \
       --document-name document_name \
       --targets Key=ResourceGroup,Values=Resource_Group_name \
       --target-parameter-name parameter_name \
       --parameters "input_parameter_name1=input_parameter_value1,input_parameter_name2=input_parameter_value2" \
       --max-concurrency 10 \
       --max-errors 25%
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name document_name ^
       --targets Key=ResourceGroup,Values=Resource_Group_name ^
       --target-parameter-name parameter_name ^
       --parameters "input_parameter_name1=input_parameter_value1,input_parameter_name2=input_parameter_value2" ^
       --max-concurrency 10 ^
       --max-errors 25%
   ```

------
#### [ PowerShell ]

   ```
   $Targets = New-Object Amazon.SimpleSystemsManagement.Model.Target
   $Targets.Key = "ResourceGroup"
   $Targets.Values = "Resource_Group_Name"
   
   Start-SSMAutomationExecution `
       -DocumentName "DocumentName" `
       -Targets $Targets `
       -TargetParameterName "Parameter_Name" `
       -Parameter @{"input_parameter_name1"="input_parameter_value1";"input_parameter_name2"="input_parameter_value2"} `
       -MaxConcurrency "a_number_of_instances_or_a_percentage_of_target_set" `
       -MaxError "a_number_of_errors_or_a_percentage_of_target_set"
   ```

------

   *Targeting all instances in the current AWS account and Region*

------
#### [ Linux ]

   ```
   aws ssm start-automation-execution \
       --document-name document_name \
       --targets "Key=AWS::EC2::Instance,Values=*"  \
       --target-parameter-name instanceId \
       --parameters "input_parameter_name1=input_parameter_value1" \
       --max-concurrency 10 \
       --max-errors 25%
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name document_name ^
       --targets Key=AWS::EC2::Instance,Values=* ^
       --target-parameter-name instanceId ^
       --parameters "input_parameter_name1=input_parameter_value1" ^
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
       -DocumentName "DocumentName" `
       -Targets $Targets `
       -TargetParameterName "instanceId" `
       -Parameter @{"input_parameter_name1"="input_parameter_value1"} `
       -MaxConcurrency "a_number_of_instances_or_a_percentage_of_target_set" `
       -MaxError "a_number_of_errors_or_a_percentage_of_target_set"
   ```

------

   The command returns an execution ID\. Copy this ID to the clipboard\. You can use this ID to view the status of the automation\.

------
#### [ Linux ]

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

1. Run the following command to view the automation execution\.

------
#### [ Linux ]

   ```
   aws ssm describe-automation-executions \
       --filter Key=ExecutionId,Values=a4a3c0e9-7efd-462a-8594-01234EXAMPLE
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-automation-executions ^
       --filter Key=ExecutionId,Values=a4a3c0e9-7efd-462a-8594-01234EXAMPLE
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMAutomationExecutionList | `
       Where {$_.AutomationExecutionId -eq "a4a3c0e9-7efd-462a-8594-01234EXAMPLE"}
   ```

------

1. To view details about the automation progress, run the following command\.

------
#### [ Linux ]

   ```
   aws ssm get-automation-execution \
       --automation-execution-id a4a3c0e9-7efd-462a-8594-01234EXAMPLE
   ```

------
#### [ Windows ]

   ```
   aws ssm get-automation-execution ^
       --automation-execution-id a4a3c0e9-7efd-462a-8594-01234EXAMPLE
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMAutomationExecution `
       -AutomationExecutionId a4a3c0e9-7efd-462a-8594-01234EXAMPLE
   ```

------

   The system returns information like the following\.

------
#### [ Linux ]

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