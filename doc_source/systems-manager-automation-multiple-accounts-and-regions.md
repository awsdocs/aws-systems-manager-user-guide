# Running automations in multiple AWS Regions and accounts<a name="systems-manager-automation-multiple-accounts-and-regions"></a>

You can run AWS Systems Manager automations across multiple AWS Regions and AWS accounts or AWS Organizational Units \(OUs\) from an Automation management account\. Running automations in multiple Regions and accounts or OUs reduces the time required to administer your AWS resources while enhancing the security of your computing environment\.

For example, you can centrally implement patching and security updates, remediate compliance drift on VPC configurations or S3 bucket policies, and manage resources, such as EC2 instances, at scale\. The following graphic shows an example of a user who is running the AWS\-RestartEC2Instances document in multiple Regions and accounts from an Automation management account\. The automation locates the instances by using the specified tags in the specified Regions and accounts\.

**Note**  
When you run an automation across multiple Regions and accounts, you target resources by using tags or the name of an AWS resource group\. The resource group must exist in each target account and Region, and the resource group name must be the same in each target account and Region\. The automation fails to run on those resources that don't have the specified tag or that aren't included in the specified resource group\.

![\[Illustration showing Systems Manager Automation running in multiple Regions and multiple accounts.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/automation-multi-region-and-multi-account.png)

**Important**  
Your account is charged for running automations in multiple Regions and accounts\. Multi\-Region and account step executions are considered *special steps*\. There is no step limit for special steps, but your account is charged for each step processed by Systems Manager\. For more information, see the [AWS Systems Manager Pricing](https://aws.amazon.com/systems-manager/pricing/) page\.

**How It Works**  
Running automations across multiple Regions and accounts or OUs works as follows:

1. Verify that all resources on which you want to run the automation, in all Regions and accounts or OUs, use identical tags\. If they don't, you can add them to an AWS resource group and target that group\. For more information, see [What Is AWS Resource Groups?](https://docs.aws.amazon.com/ARG/latest/userguide/)

1. Sign in to the AWS Identity and Access Management \(IAM\) account that you want to configure as the Automation Primary account\.

1. Use the procedure in this topic to create the IAM automation role named **AWS\-SystemsManager\-AutomationExecutionRole**\. This role gives the user permission to run automation workflows\.

1. Use the procedure in this topic to create the second IAM role, named **AWS\-SystemsManager\-AutomationAdministrationRole**\. This role gives the user permission to run automation workflows in multiple AWS accounts and OUs\.

1. Choose the Automation document, Regions, and accounts or OUs where you want to run the automation\.
**Note**  
Automations do not run recursively through OUs\. Be sure the target OU contains the desired accounts\.

1. Run the automation\. When running automations across multiple Regions, accounts, or OUs, the automation you run from the primary account starts child automations in each of the target accounts\. The automation in the primary account will have `aws:executeAutomation` steps for each of the target accounts\.

1. Use the [GetAutomationExecution](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetAutomationExecution.html), [DescribeAutomationStepExecutions](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeAutomationStepExecutions.html), and [DescribeAutomationExecutions](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeAutomationExecutions.html) API actions from the AWS Systems Manager console or the AWS CLI to monitor workflow progress\. The output of the steps for the automation in your primary account will be the `AutomationExecutionId` of the child automations\. To view the output of the child automations created in your target accounts, be sure to specify the appropriate account, Region, and `AutomationExecutionId` in your request\.

## Setting up management account permissions for multi\-Region and multi\-account automation<a name="systems-manager-automation-multiple-accounts-and-regions-permissions"></a>

Use the following procedure to create the required IAM roles for Systems Manager Automation multi\-Region and multi\-account automation by using AWS CloudFormation\. This procedure describes how to create the **AWS\-SystemsManager\-AutomationExecutionRole** role\. You must create this role in *every* account that you want to target to run multi\-Region and multi\-account automations\.

This procedure also describes how to create the **AWS\-SystemsManager\-AutomationAdministrationRole** role\. You only need to create this role in the Automation management account\.

**To create the required IAM roles for multi\-Region and multi\-account automations by using AWS CloudFormation**

1. Download and unzip the [AWS\-SystemsManager\-AutomationExecutionRole\.zip folder](samples/AWS-SystemsManager-AutomationExecutionRole.zip)\. This folder includes the AWS\-SystemsManager\-AutomationExecutionRole\.json AWS CloudFormation template file\.
**Note**  
We recommend not changing the role name as specified in the template to something besides `AWS-SystemsManager-AutomationExecutionRole`\. Otherwise, your multi\-Region and multi\-Account automations might fail\.

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Choose **Create Stack**\.

1. In the **Choose a template section**, choose **Upload a template to Amazon S3**\.

1. Choose **Browse**, and then choose the `AWS-SystemsManager-AutomationExecutionRole.json` AWS CloudFormation template file\.

1. Choose **Next**\.

1. On the **Specify Details** page, in the **Stack Name** field, enter a name\. 

1. In the **Parameters** section, in the **MasterAccountID** field, enter the ID for the account that you want to use to run multi\-Region and multi\-account automations\.

1. Choose **Next**\.

1. On the **Options** page, enter values for any options you want to use\. Choose **Next**\.

1. On the **Review** page, scroll down and choose the **I acknowledge that AWS CloudFormation might create IAM resources** option\.

1. Choose **Create**\.

   AWS CloudFormation shows the **CREATE\_IN\_PROGRESS** status for approximately three minutes\. The status changes to **CREATE\_COMPLETE**\.

1. Repeat this procedure in *every* account that you want to target to run multi\-Region and multi\-account automations\.

1. Download the [AWS\-SystemManager\-AutomationAdministrationRole\.zip folder](samples/AWS-SystemManager-AutomationAdministrationRole.zip) and repeat this procedure for the **AWS\-SystemManager\-AutomationAdministrationRole** role\. You only need to create the **AWS\-SystemManager\-AutomationAdministrationRole** role in the Automation management account\.
**Note**  
The IAM user or role you use to run a multi\-Region or multi\-account automation must have the `iam:PassRole` permission for the `AWS-SystemManager-AutomationAdministrationRole` role\. We recommend not changing the role name as specified in the template to something besides `AWS-SystemsManager-AutomationAdministrationRole`\. Otherwise, your multi\-Region and multi\-account automations might fail\.

## Run an automation in multiple Regions and accounts \(console\)<a name="systems-manager-automation-multiple-accounts-and-regions-executing"></a>

The following procedure describes how to use the Systems Manager console to run an automation in multiple Regions and accounts from the Automation management account\.

**Before You Begin**  
Before you complete the following procedure, note the following information:
+ AWS account IDs or OUs where you want to run the automation\.
+ [Regions supported by Systems Manager](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) where you want to run the automation\.
+ The tag key and the tag value, or the name of the resource group, where you want to run the automation\.

**To run an automation in multiple Regions and accounts**

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

1. On the **Execute automation document** page, choose **Multi\-account and Region**\.

1. In the **Target accounts and Regions** section, use the **Accounts and organizational \(OUs\)** field to specify the different AWS accounts or AWS Organizational Units \(OUs\) where you want to run the automation\. Separate multiple accounts or OUs with a comma\. 

1. Use the **AWS Regions** list to choose one or more Regions where you want to run the automation\.

1. Use the **Multi\-Region and account rate control** options to restrict the automation to a limited number of accounts running in a limited number of Regions\. These options don't restrict the number of AWS resources that can run the automations\. 

   1. In the **Location \(account\-Region pair\) concurrency** section, choose an option to restrict the number of automations that can run in multiple accounts and Regions at the same time\. For example, if you choose to run an automation in five \(5\) AWS accounts, which are located in four \(4\) AWS Regions, then Systems Manager runs automations in a total of 20 account\-Region pairs\. You can use this option to specify an absolute number, such as **2**, so that the automation only runs in two account\-Region pairs at the same time\. Or you can specify a percentage of the account\-Region pairs that can run at the same time\. For example, with 20 account\-Region pairs, if you specify 20%, then the automation simultaneously runs in a maximum of five \(5\) account\-Region pairs\. 
      + Choose **targets** to enter an absolute number of account\-Region pairs that can run the automation simultaneously\.
      + Choose **percent** to enter a percentage of the total number of account\-Region pairs that can run the automation simultaneously\.

   1. In the **Error threshold** section, choose an option:
      + Choose **errors** to enter an absolute number of errors allowed before Automation stops sending the workflow to other resources\.
      + Choose **percent** to enter a percentage of errors allowed before Automation stops sending the workflow to other resources\.

1. In the **Targets** section, choose how you want to target the AWS Resources where you want to run the Automation\. These options are required\.

   1. Use the **Parameter** list to choose a parameter\. The items in the **Parameter** list are determined by the parameters in the Automation document that you selected at the start of this procedure\. By choosing a parameter you define the type of resource on which the Automation workflow runs\. 

   1. Use the **Targets** list to choose how you want to target resources\.

      1. If you chose to target resources by using parameter values, then enter the parameter value for the parameter you chose in the **Input parameters** section\.

      1. If you chose to target resources by using AWS Resource Groups, then choose the name of the group from the **Resource Group** list\.

      1. If you chose to target resources by using tags, then enter the tag key and \(optionally\) the tag value in the fields provided\. Choose **Add**\.

      1. If you want to run an Automation playbook on all instances in the current AWS account and Region, then choose **All instances**\.

1. In the **Input parameters** section, specify the required inputs\. Optionally, you can choose an IAM service role from the **AutomationAssumeRole** list\.
**Note**  
You may not need to choose some of the options in the **Input parameters** section\. This is because you targeted resources in multiple Regions and accounts by using tags or a resource group\. For example, if you chose the AWS\-RestartEC2Instance document, then you don't need to specify or choose instance IDs in the **Input parameters** section\. The automation locates the instances to restart by using the tags you specified\. 

1. Use the options in the **Rate control** section to restrict the number of AWS resources that can run the Automation within each account\-Region pair\. 

   In the **Concurrency** section, choose an option: 
   + Choose **targets** to enter an absolute number of targets that can run the Automation workflow simultaneously\.
   + Choose **percentage** to enter a percentage of the target set that can run the Automation workflow simultaneously\.

1. In the **Error threshold** section, choose an option:
   + Choose **errors** to enter an absolute number of errors allowed before Automation stops sending the workflow to other resources\.
   + Choose **percentage** to enter a percentage of errors allowed before Automation stops sending the workflow to other resources\.

1. Choose **Execute**\.

## Run an Automation in multiple Regions and accounts \(command line\)<a name="systems-manager-automation-multiple-accounts-and-regions-executing-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to run an automation in multiple Regions and accounts from the Automation management account\.

**Before You Begin**  
Before you complete the following procedure, note the following information:
+ AWS account IDs or OUs where you want to run the automation\.
+ [Regions supported by Systems Manager](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) where you want to run the automation\.
+ The tag key and the tag value, or the name of the resource group, where you want to run the automation\.

**To run an automation in multiple Regions and accounts**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Use the following format to create a command to run an automation in multiple Regions and accounts\.

------
#### [ Linux ]

   ```
   aws ssm start-automation-execution \
       --document-name name_of_Automation_document \
       --parameters AutomationAssumeRole=arn:aws:iam::Automation_management_account_ID:role/AWS-SystemsManager-AutomationAdministrationRole \
       --target-parameter-name parameter_name (required) \
       --targets Key=tag_key,Values=tag_value \
       --target-locations Accounts=account_ID_1,account_ID_2,account_ID_3,Regions=Region_1,Region_2,ExecutionRoleName=AWS-SystemsManager-AutomationExecutionRole
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name name_of_Automation_document ^
       --parameters AutomationAssumeRole=arn:aws:iam::Automation_management_account_ID:role/AWS-SystemsManager-AutomationAdministrationRole ^
       --target-parameter-name parameter_name (required) ^
       --targets Key=tag_key,Values=tag_value ^
       --target-locations Accounts=account_ID_1,account_ID_2,account_ID_3,Regions=Region_1,Region_2,ExecutionRoleName=AWS-SystemsManager-AutomationExecutionRole
   ```

------
#### [ PowerShell ]

   ```
   $Targets = New-Object Amazon.SimpleSystemsManagement.Model.Target
   $Targets.Key = "target_key"
   $Targets.Values = "target_value"
   
   Start-SSMAutomationExecution `
       -DocumentName "name_of_Automation_document" `
       -Parameter @{
       "AutomationAssumeRole"="arn:aws:iam::Automation_management_account_ID:role/AWS-SystemsManager-AutomationAdministrationRole" } `
       -TargetParameterName "parameter_name (required)" `
       -Target $Targets `
       -TargetLocation @{
       "Accounts"="account_ID_1","account_ID_2","account_ID_3";
       "Regions"="Region_1","Region_2";
       "ExecutionRoleName"="AWS-SystemsManager-AutomationExecutionRole" }
   ```

------

   Here are a few examples\.

   **Example 1**: This example restarts EC2 instances in the `123456789012` and `987654321098` accounts, which are located in the `us-east-2` and `us-west-1` Regions\. The instances must be tagged with the tag key\-pair value `Env-PROD`\.

------
#### [ Linux ]

   ```
   aws ssm start-automation-execution \
       --document-name AWS-RestartEC2Instance \
       --parameters AutomationAssumeRole=arn:aws:iam::123456789012:role/AWS-SystemsManager-AutomationAdministrationRole \
       --target-parameter-name InstanceId \
       --targets Key=tag:Env,Values=PROD \
       --target-locations Accounts=123456789012,987654321098,Regions=us-east-2,us-west-1,ExecutionRoleName=AWS-SystemsManager-AutomationExecutionRole
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name AWS-RestartEC2Instance ^
       --parameters AutomationAssumeRole=arn:aws:iam::123456789012:role/AWS-SystemsManager-AutomationAdministrationRole ^
       --target-parameter-name InstanceId ^
       --targets Key=tag:Env,Values=PROD ^
       --target-locations Accounts=123456789012,987654321098,Regions=us-east-2,us-west-1,ExecutionRoleName=AWS-SystemsManager-AutomationExecutionRole
   ```

------
#### [ PowerShell ]

   ```
   $Targets = New-Object Amazon.SimpleSystemsManagement.Model.Target
   $Targets.Key = "tag:Env"
   $Targets.Values = "PROD"
   
   Start-SSMAutomationExecution `
       -DocumentName "AWS-RestartEC2Instance" `
       -Parameter @{
       "AutomationAssumeRole"="arn:aws:iam::123456789012:role/AWS-SystemsManager-AutomationAdministrationRole" } `
       -TargetParameterName "InstanceId" `
       -Target $Targets `
       -TargetLocation @{
       "Accounts"="123456789012","987654321098";
       "Regions"="us-east-2","us-west-1";
       "ExecutionRoleName"="AWS-SystemsManager-AutomationExecutionRole" }
   ```

------

   **Example 2**: This example restarts EC2 instances in the `123456789012` and `987654321098` accounts, which are located in the `eu-central-1` Region\. The instances must be members of the `prod-instances` AWS resource group\.

------
#### [ Linux ]

   ```
   aws ssm start-automation-execution \
       --document-name AWS-RestartEC2Instance \
       --parameters AutomationAssumeRole=arn:aws:iam::123456789012:role/AWS-SystemsManager-AutomationAdministrationRole \
       --target-parameter-name InstanceId \
       --targets Key=ResourceGroup,Values=prod-instances \
       --target-locations Accounts=123456789012,987654321098,Regions=eu-central-1,ExecutionRoleName=AWS-SystemsManager-AutomationExecutionRole
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name AWS-RestartEC2Instance ^
       --parameters AutomationAssumeRole=arn:aws:iam::123456789012:role/AWS-SystemsManager-AutomationAdministrationRole ^
       --target-parameter-name InstanceId ^
       --targets Key=ResourceGroup,Values=prod-instances ^
       --target-locations Accounts=123456789012,987654321098,Regions=eu-central-1,ExecutionRoleName=AWS-SystemsManager-AutomationExecutionRole
   ```

------
#### [ PowerShell ]

   ```
   $Targets = New-Object Amazon.SimpleSystemsManagement.Model.Target
   $Targets.Key = "ResourceGroup"
   $Targets.Values = "prod-instances"
   
   Start-SSMAutomationExecution `
       -DocumentName "AWS-RestartEC2Instance" `
       -Parameter @{
       "AutomationAssumeRole"="arn:aws:iam::123456789012:role/AWS-SystemsManager-AutomationAdministrationRole" } `
       -TargetParameterName "InstanceId" `
       -Target $Targets `
       -TargetLocation @{
       "Accounts"="123456789012","987654321098";
       "Regions"="eu-central-1";
       "ExecutionRoleName"="AWS-SystemsManager-AutomationExecutionRole" }
   ```

------

   **Example 3**: This example restarts EC2 instances in the `ou-1a2b3c-4d5e6c` AWS organizational unit \(OU\)\. The instances are located in the `us-west-1` and `us-west-2` Regions\. The instances must be members of the `WebServices` AWS resource group\.

------
#### [ Linux ]

   ```
   aws ssm start-automation-execution \
       --document-name AWS-RestartEC2Instance \
       --parameters AutomationAssumeRole=arn:aws:iam::123456789012:role/AWS-SystemsManager-AutomationAdministrationRole \
       --target-parameter-name InstanceId \
       --targets Key=ResourceGroup,Values=WebServices \
       --target-locations Accounts=ou-1a2b3c-4d5e6c,Regions=us-west-1,us-west-2,ExecutionRoleName=AWS-SystemsManager-AutomationExecutionRole
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name AWS-RestartEC2Instance ^
       --parameters AutomationAssumeRole=arn:aws:iam::123456789012:role/AWS-SystemsManager-AutomationAdministrationRole ^
       --target-parameter-name InstanceId ^
       --targets Key=ResourceGroup,Values=WebServices ^
       --target-locations Accounts=ou-1a2b3c-4d5e6c,Regions=us-west-1,us-west-2,ExecutionRoleName=AWS-SystemsManager-AutomationExecutionRole
   ```

------
#### [ PowerShell ]

   ```
   $Targets = New-Object Amazon.SimpleSystemsManagement.Model.Target
   $Targets.Key = "ResourceGroup"
   $Targets.Values = "WebServices"
   
   Start-SSMAutomationExecution `
       -DocumentName "AWS-RestartEC2Instance" `
       -Parameter @{
       "AutomationAssumeRole"="arn:aws:iam::123456789012:role/AWS-SystemsManager-AutomationAdministrationRole" } `
       -TargetParameterName "InstanceId" `
       -Target $Targets `
       -TargetLocation @{
       "Accounts"="ou-1a2b3c-4d5e6c";
       "Regions"="us-west-1";
       "ExecutionRoleName"="AWS-SystemsManager-AutomationExecutionRole" }
   ```

------

   The system returns information similar to the following\.

------
#### [ Linux ]

   ```
   {
       "AutomationExecutionId": "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE"
   }
   ```

------
#### [ Windows ]

   ```
   {
       "AutomationExecutionId": "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE"
   }
   ```

------
#### [ PowerShell ]

   ```
   4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE
   ```

------

1. Run the following command to view details for the automation\.

------
#### [ Linux ]

   ```
   aws ssm describe-automation-executions \
       --filters Key=ExecutionId,Values=4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-automation-executions ^
       --filters Key=ExecutionId,Values=4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMAutomationExecutionList | `
       Where {$_.AutomationExecutionId -eq "a4a3c0e9-7efd-462a-8594-01234EXAMPLE"}
   ```

------

1. Run the following command to view details about the automation progress\.

------
#### [ Linux ]

   ```
   aws ssm get-automation-execution \
       --automation-execution-id 4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE
   ```

------
#### [ Windows ]

   ```
   aws ssm get-automation-execution ^
       --automation-execution-id 4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMAutomationExecution `
       -AutomationExecutionId a4a3c0e9-7efd-462a-8594-01234EXAMPLE
   ```

------
**Note**  
You can also monitor the status of the automation in the console\. In the **Automation executions** list, choose the automation you just ran and then choose the **Execution steps** tab\. This tab shows the status of the automation actions\.

## Related content<a name="systems-manager-automation-multiple-accounts-and-regions-executing-related"></a>

[Centralized multi\-account and multi\-Region patching with AWS Systems Manager Automation](http://aws.amazon.com/blogs/mt/centralized-multi-account-and-multi-region-patching-with-aws-systems-manager-automation/)