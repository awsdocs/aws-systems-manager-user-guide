# Executing Automation Workflows that Use Targets and Rate Controls<a name="automation-working-targets-and-rate-controls"></a>

AWS Systems Manager enables you to execute Automation workflows on a fleet of AWS resources by using targets\. Additionally, you can control the execution of the Automation across your fleet by specifying a concurrency value and an error threshold\. The concurrency value determines how many resources are allowed to run the Automation simultaneously\. An error threshold determines how many Automation executions are allowed to fail before Systems Manager stops sending the workflow to other resources\. The concurrency and error threshold features are collectively called *rate controls*\. 

For more information about Concurrency and Error thresholds, see [About Concurrency and Error Thresholds](automation-working-rate-controls.md)\. For more information about Targets, see [About Targets](automation-working-targets.md)\.

The following procedures describe how to execute an Automation workflow with targets and rate controls by using the Systems Manager console and the AWS CLI\.

## Executing an Automation workflow with targets and rate controls \(Console\)<a name="automation-working-targets-and-rate-controls-console"></a>

**To execute an Automation workflow with targets and rate controls \(Console\)**

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

1. In the **Execution Mode** section, choose **Rate Control**\. You must use this mode or **Multi\-account and Region** if you want to use targets and rate controls\.

1. In the **Targets** section, choose how you want to target the AWS Resources where you want to run the Automation\. These options are required\.

   1. Use the **Parameter** list to choose a parameter\. The items in the **Parameter** list are determined by the parameters in the Automation document that you selected at the start of this procedure\. By choosing a parameter you define the type of resource on which the Automation workflow runs\. 

   1. Use the **Targets** list to choose how you want to target resources\. If you chose to target resources by using AWS Resource Groups, then choose the name of the group from the **Resource Group** list\. 

      If you chose to target resources by using tags, then enter the tag key and \(optionally\) the tag value in the fields provided\. Choose **Add**\.

1. In the **Input parameters** section, specify the required inputs\. Optionally, you can choose an IAM service role from the **AutomationAssumeRole** list\.
**Note**  
You may not need to choose some of the options in the **Input parameters** section\. This is because you targeted resources by using tags or a Resource Group\. For example, if you chose the AWS\-RestartEC2Instance document, then you don't need to specify or choose instance IDs in the **Input parameters** section\. The Automation execution locates the instances to restart by using the tags or Resource Group you specified\. 

1. Use the options in the **Rate control** section to restrict the number of AWS resources that can execute the Automation within each account\-Region pair\. 

   In the **Concurrency** section, choose an option: 
   + Choose **targets** to enter an absolute number of targets that can execute the Automation workflow simultaneously\.
   + Choose **percentage** to enter a percentage of the target set that can execute the Automation workflow simultaneously\.

1. In the **Error threshold** section, choose an option:
   + Choose **errors** to enter an absolute number of errors allowed before Automation stops sending the workflow to other resources\.
   + Choose **percentage** to enter a percentage of errors allowed before Automation stops sending the workflow to other resources\.

1. Choose **Execute**\. 

## Executing an Automation workflow with targets and rate controls \(AWS CLI\)<a name="automation-working-targets-and-rate-controls-cli"></a>

**To execute an Automation workflow with targets and rate controls \(AWS CLI\)**

1. [Download](https://aws.amazon.com/cli/) the latest version of the AWS CLI to your local machine\.

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges, or you must have been granted the appropriate permission in AWS Identity and Access Management \(IAM\)\.

   ```
   aws configure
   ```

   The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

1. Execute the following command to view a list of documents\.

   ```
   aws ssm list-documents
   ```

   Note the name of the Automation document that you want to execute\.

1. Execute the following command to view details about the Automation document\. Make a note of a parameter name \(for example, InstanceId\) that you want to use for the \-\-target\-parameter\-name option\. This parameter determines the type of resource on which the Automation executes\.

   ```
   aws ssm describe-document --name document_name
   ```

1. Create a command that uses the targets and rate control options you want to execute\. Here are some AWS CLI template commands to help you\.

   *Targeting using tags*

   ```
   aws ssm start-automation-execution --document-name document_name --targets Key=tag:key_name,Values=value --target-parameter-name parameter_name --parameters "input_parameter_name1=input_parameter_value1,input_parameter_name2=input_parameter_value2" --max-concurrency 10 --max-errors 25%
   ```

   *Targeting using parameter values*

   ```
   aws ssm start-automation-execution --document-name document_name --target-parameter-name parameter_name --targets Key=ParameterValues,Values=value_1,value_2,value_3 --parameters "input_parameter_name1=input_parameter_value1" --max-concurrency 50% --max-errors 10%
   ```

   *Targeting using AWS Resource Groups*

   ```
   aws ssm start-automation-execution --document-name document_name --target-parameter-name parameter_name --targets Key=ResourceGroup,Values=Resource_Group_name --max-concurrency 1 --max-errors 0
   ```

   The command returns an execution ID\. Copy this ID to the clipboard\. You can use this ID to view the status of the workflow\.

   ```
   {
       "AutomationExecutionId": "ID"
   }
   ```

1. Execute the following command to view the workflow execution\.

   ```
   aws ssm describe-automation-executions
   ```

1. To view details about the execution progress, run the following command\.

   ```
   aws ssm get-automation-execution --automation-execution-id ID
   ```
**Note**  
You can also monitor the status of the workflow in the console\. In the execution list, choose the execution you just ran and then choose the **Steps** tab\. This tab shows you the status of the workflow actions\.