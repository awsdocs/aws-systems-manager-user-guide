# Running automations in multiple AWS Regions and accounts<a name="systems-manager-automation-multiple-accounts-and-regions"></a>

You can run AWS Systems Manager automations across multiple AWS Regions and AWS accounts or AWS Organizations organizational units \(OUs\) from a central account\. Automation is a capability of AWS Systems Manager\. Running automations in multiple Regions and accounts or OUs reduces the time required to administer your AWS resources while enhancing the security of your computing environment\.

For example, you can do the following by using automation runbooks:
+ Implement patching and security updates centrally\.
+ Remediate compliance drift on VPC configurations or Amazon S3 bucket policies\.
+ Manage resources, such as Amazon Elastic Compute Cloud \(Amazon EC2\) EC2 instances, at scale\.

The following diagram shows an example of a user who is running the `AWS-RestartEC2Instances` runbook in multiple Regions and accounts from a central account\. The automation locates the instances by using the specified tags in the targeted Regions and accounts\.

**Note**  
When you run an automation across multiple Regions and accounts, you target resources by using tags or the name of an AWS resource group\. The resource group must exist in each target account and Region\. The resource group name must be the same in each target account and Region\. The automation fails to run on those resources that don't have the specified tag or that aren't included in the specified resource group\.

![\[Illustration showing Systems Manager Automation running in multiple Regions and multiple accounts.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/automation-multi-region-and-multi-account.png)

**Choose a central account for Automation**  
If you want to run automations across OUs, the central account must have permissions to list all of the accounts in the OUs\. This is only possible from a delegated administrator account, or the management account of the organization\. We recommend that you follow AWS Organizations best practices and use a delegated administrator account\. For more information about AWS Organizations best practices, see [Best practices for the management account](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_best-practices_mgmt-acct.html) in the *AWS Organizations User Guide*\. To create a delegated administrator account for Systems Manager, you can use the `register-delegated-administrator` command with the AWS CLI as shown in the following example\.

```
aws organizations register-delegated-administrator \
    --account-id delegated admin account ID \
    --service-principal ssm.amazonaws.com
```

If you want to run automations across multiple accounts that are not managed by AWS Organizations, we recommend creating a dedicated account for automation management\. Running all cross\-account automations from a dedicated account simplifies IAM permissions management, troubleshooting efforts, and creates a layer of separation between operations and administration\. This approach is also recommended if you use AWS Organizations, but only want to target individual accounts and not OUs\.

**How running automations works**  
Running automations across multiple Regions and accounts or OUs works as follows:

1. Verify that all resources on which you want to run the automation, in all Regions and accounts or OUs, use identical tags\. If they don't, you can add them to an AWS resource group and target that group\. For more information, see [What are resource groups?](https://docs.aws.amazon.com/ARG/latest/userguide/) in the *AWS Resource Groups and Tags User Guide*\.

1. Sign in to the account that you want to configure as the Automation central account\.

1. Use the [Setting up management account permissions for multi\-Region and multi\-account automation](#systems-manager-automation-multiple-accounts-and-regions-permissions) procedure in this topic to create the following IAM roles:
   + `AWS-SystemsManager-AutomationAdministrationRole` \- This role gives the user permission to run automations in multiple accounts and OUs\.
   + `AWS-SystemsManager-AutomationExecutionRole` \- This role gives the user permission to run automations in the targeted accounts\.

1. Choose the runbook, Regions, and accounts or OUs where you want to run the automation\.
**Note**  
Automations don't run recursively through OUs\. Be sure that the target OU contains the desired accounts\. If you choose a custom runbook, the runbook must be shared with all of the target accounts\. For information about sharing runbooks, see [Sharing SSM documents](ssm-sharing.md)\. For information about using shared runbooks, see [Using shared SSM documents](ssm-using-shared.md)\.

1. Run the automation\.
**Note**  
When running automations across multiple Regions, accounts, or OUs, the automation you run from the primary account starts child automations in each of the target accounts\. The automation in the primary account contains `aws:executeAutomation` steps for each of the target accounts\. If you start an automation from new Regions launched after March 20, 2019, and target a Region that is enabled by default, the automation fails\. If you start an automation from a Region that is enabled by default and target a Region you have enabled, the automation runs successfully\.

1. Use the [GetAutomationExecution](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetAutomationExecution.html), [DescribeAutomationStepExecutions](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeAutomationStepExecutions.html), and [DescribeAutomationExecutions](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeAutomationExecutions.html) API operations from the AWS Systems Manager console or the AWS CLI to monitor automation progress\. The output of the steps for the automation in your primary account will be the `AutomationExecutionId` of the child automations\. To view the output of the child automations created in your target accounts, be sure to specify the appropriate account, Region, and `AutomationExecutionId` in your request\.

## Setting up management account permissions for multi\-Region and multi\-account automation<a name="systems-manager-automation-multiple-accounts-and-regions-permissions"></a>

Use the following procedure to create the required IAM roles for Systems Manager Automation multi\-Region and multi\-account automation by using AWS CloudFormation\. This procedure describes how to create the `AWS-SystemsManager-AutomationAdministrationRole` role\. You only need to create this role in the Automation central account\. This procedure also describes how to create the `AWS-SystemsManager-AutomationExecutionRole` role\. You must create this role in *every* account that you want to target to run multi\-Region and multi\-account automations\. We recommend using AWS CloudFormation StackSets to create the `AWS-SystemsManager-AutomationExecutionRole` role in the accounts you want to target to run multi\-Region and multi\-account automations\.

**To create the required IAM administration role for multi\-Region and multi\-account automations by using AWS CloudFormation**

1. Download and unzip the [https://docs.aws.amazon.com/systems-manager/latest/userguide/samples/AWS-SystemsManager-AutomationAdministrationRole.zip](https://docs.aws.amazon.com/systems-manager/latest/userguide/samples/AWS-SystemsManager-AutomationAdministrationRole.zip)\. Or, if your accounts are managed by AWS Organizations [https://docs.aws.amazon.com/systems-manager/latest/userguide/samples/AWS-SystemsManager-AutomationAdministrationRole-org.zip](https://docs.aws.amazon.com/systems-manager/latest/userguide/samples/AWS-SystemsManager-AutomationAdministrationRole-org.zip)\. This file contains the `AWS-SystemsManager-AutomationAdministrationRole.json` AWS CloudFormation template file\.

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Choose **Create stack**\.

1. In the **Specify template** section, choose **Upload a template**\.

1. Choose **Choose file**, and then choose the `AWS-SystemsManager-AutomationAdministrationRole.json` AWS CloudFormation template file\.

1. Choose **Next**\.

1. On the **Specify stack details** page, in the **Stack name** field, enter a name\. 

1. Choose **Next**\.

1. On the **Configure stack options** page, enter values for any options you want to use\. Choose **Next**\.

1. On the **Review** page, scroll down and choose the **I acknowledge that AWS CloudFormation might create IAM resources with custom names** option\.

1. Choose **Create stack**\.

AWS CloudFormation shows the **CREATE\_IN\_PROGRESS** status for approximately three minutes\. The status changes to **CREATE\_COMPLETE**\.

You must repeat the following procedure in *every* account that you want to target to run multi\-Region and multi\-account automations\.

**To create the required IAM automation role for multi\-Region and multi\-account automations by using AWS CloudFormation**

1. Download the [https://docs.aws.amazon.com/systems-manager/latest/userguide/samples/AWS-SystemsManager-AutomationExecutionRole.zip](https://docs.aws.amazon.com/systems-manager/latest/userguide/samples/AWS-SystemsManager-AutomationExecutionRole.zip)\. Or, if your accounts are managed by AWS Organizations [https://docs.aws.amazon.com/systems-manager/latest/userguide/samples/AWS-SystemsManager-AutomationExecutionRole-org.zip](https://docs.aws.amazon.com/systems-manager/latest/userguide/samples/AWS-SystemsManager-AutomationExecutionRole-org.zip)\.This file contains the `AWS-SystemsManager-AutomationExecutionRole.json` AWS CloudFormation template file\.

1. Open the AWS CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Choose **Create stack**\.

1. In the **Specify template** section, choose **Upload a template**\.

1. Choose **Choose file**, and then choose the `AWS-SystemsManager-AutomationExecutionRole.json` AWS CloudFormation template file\.

1. Choose **Next**\.

1. On the **Specify stack details** page, in the **Stack name** field, enter a name\. 

1. In the **Parameters** section, in the **AdminAccountId** field, enter the ID for the Automation central account\.

1. If you are setting up this role for an AWS Organizations environment, there is another field in the section called **OrganizationID**\. Enter the ID of your AWS organization\.

1. Choose **Next**\.

1. On the **Configure stack options** page, enter values for any options you want to use\. Choose **Next**\.

1. On the **Review** page, scroll down and choose the **I acknowledge that AWS CloudFormation might create IAM resources with custom names** option\.

1. Choose **Create stack**\.

AWS CloudFormation shows the **CREATE\_IN\_PROGRESS** status for approximately three minutes\. The status changes to **CREATE\_COMPLETE**\.

## Run an automation in multiple Regions and accounts \(console\)<a name="systems-manager-automation-multiple-accounts-and-regions-executing"></a>

The following procedure describes how to use the Systems Manager console to run an automation in multiple Regions and accounts from the Automation management account\.

**Before you begin**  
Before you complete the following procedure, note the following information:
+ The IAM user or role you use to run a multi\-Region or multi\-account automation must have the `iam:PassRole` permission for the `AWS-SystemsManager-AutomationAdministrationRole` role\.
+ AWS account IDs or OUs where you want to run the automation\.
+ [Regions supported by Systems Manager](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) where you want to run the automation\.
+ The tag key and the tag value, or the name of the resource group, where you want to run the automation\.

**To run an automation in multiple Regions and accounts**

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

1. On the **Execute automation document** page, choose **Multi\-account and Region**\.

1. In the **Target accounts and Regions** section, use the **Accounts and organizational \(OUs\)** field to specify the different AWS accounts or AWS organizational units \(OUs\) where you want to run the automation\. Separate multiple accounts or OUs with a comma\. 

1. Use the **AWS Regions** list to choose one or more Regions where you want to run the automation\.

1. Use the **Multi\-Region and account rate control** options to restrict the automation to a limited number of accounts running in a limited number of Regions\. These options don't restrict the number of AWS resources that can run the automations\. 

   1. In the **Location \(account\-Region pair\) concurrency** section, choose an option to restrict the number of automations that can run in multiple accounts and Regions at the same time\. For example, if you choose to run an automation in five \(5\) AWS accounts, which are located in four \(4\) AWS Regions, then Systems Manager runs automations in a total of 20 account\-Region pairs\. You can use this option to specify an absolute number, such as **2**, so that the automation only runs in two account\-Region pairs at the same time\. Or you can specify a percentage of the account\-Region pairs that can run at the same time\. For example, with 20 account\-Region pairs, if you specify 20%, then the automation simultaneously runs in a maximum of five \(5\) account\-Region pairs\. 
      + Choose **targets** to enter an absolute number of account\-Region pairs that can run the automation simultaneously\.
      + Choose **percent** to enter a percentage of the total number of account\-Region pairs that can run the automation simultaneously\.

   1. In the **Error threshold** section, choose an option:
      + Choose **errors** to enter an absolute number of errors allowed before Automation stops sending the automation to other resources\.
      + Choose **percent** to enter a percentage of errors allowed before Automation stops sending the automation to other resources\.

1. In the **Targets** section, choose how you want to target the AWS resources where you want to run the Automation\. These options are required\.

   1. Use the **Parameter** list to choose a parameter\. The items in the **Parameter** list are determined by the parameters in the Automation runbook that you selected at the start of this procedure\. By choosing a parameter you define the type of resource on which the Automation workflow runs\. 

   1. Use the **Targets** list to choose how you want to target resources\.

      1. If you chose to target resources by using parameter values, then enter the parameter value for the parameter you chose in the **Input parameters** section\.

      1. If you chose to target resources by using AWS Resource Groups, then choose the name of the group from the **Resource Group** list\.

      1. If you chose to target resources by using tags, then enter the tag key and \(optionally\) the tag value in the fields provided\. Choose **Add**\.

      1. If you want to run an Automation runbook on all instances in the current AWS account and AWS Region, then choose **All instances**\.

1. In the **Input parameters** section, specify the required inputs\. Optionally, you can choose an IAM service role from the **AutomationAssumeRole** list\.
**Note**  
You might not need to choose some of the options in the **Input parameters** section\. This is because you targeted resources in multiple Regions and accounts by using tags or a resource group\. For example, if you chose the `AWS-RestartEC2Instance` runbook, then you don't need to specify or choose instance IDs in the **Input parameters** section\. The automation locates the instances to restart by using the tags you specified\. 

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

**Before you begin**  
Before you complete the following procedure, note the following information:
+ AWS account IDs or OUs where you want to run the automation\.
+ [Regions supported by Systems Manager](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) where you want to run the automation\.
+ The tag key and the tag value, or the name of the resource group, where you want to run the automation\.

**To run an automation in multiple Regions and accounts**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you haven't already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Use the following format to create a command to run an automation in multiple Regions and accounts\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm start-automation-execution \
           --document-name runbook name \
           --parameters AutomationAssumeRole=arn:aws:iam::management account ID:role/AWS-SystemsManager-AutomationAdministrationRole \
           --target-parameter-name parameter name \
           --targets Key=tag key,Values=value \
           --target-locations Accounts=account ID,account ID 2,Regions=Region,Region 2,ExecutionRoleName=AWS-SystemsManager-AutomationExecutionRole
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
           --document-name runbook name ^
           --parameters AutomationAssumeRole=arn:aws:iam::management account ID:role/AWS-SystemsManager-AutomationAdministrationRole ^
           --target-parameter-name parameter name ^
           --targets Key=tag key,Values=value ^
           --target-locations Accounts=account ID,account ID 2,Regions=Region,Region 2,ExecutionRoleName=AWS-SystemsManager-AutomationExecutionRole
   ```

------
#### [ PowerShell ]

   ```
   $Targets = New-Object Amazon.SimpleSystemsManagement.Model.Target
       $Targets.Key = "tag key"
       $Targets.Values = "value"
       
       Start-SSMAutomationExecution `
           -DocumentName "runbook name" `
           -Parameter @{
           "AutomationAssumeRole"="arn:aws:iam::management account ID:role/AWS-SystemsManager-AutomationAdministrationRole" } `
           -TargetParameterName "parameter name" `
           -Target $Targets `
           -TargetLocation @{
           "Accounts"="account ID","account ID 2";
           "Regions"="Region","Region 2";
           "ExecutionRoleName"="AWS-SystemsManager-AutomationExecutionRole" }
   ```

------

   Here are several examples\.

   **Example 1**: This example restarts EC2 instances in the `123456789012` and `987654321098` accounts, which are located in the `us-east-2` and `us-west-1` Regions\. The instances must be tagged with the tag key\-pair value `Env-PROD`\.

------
#### [ Linux & macOS ]

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
#### [ Linux & macOS ]

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
#### [ Linux & macOS ]

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
#### [ Linux & macOS ]

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

1. Run the following command to view details for the automation\. Replace *automation execution ID* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm describe-automation-executions \
           --filters Key=ExecutionId,Values=automation execution ID
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-automation-executions ^
           --filters Key=ExecutionId,Values=automation execution ID
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMAutomationExecutionList | `
           Where {$_.AutomationExecutionId -eq "automation execution ID"}
   ```

------

1. Run the following command to view details about the automation progress\.

------
#### [ Linux & macOS ]

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

**More info**  
[Centralized multi\-account and multi\-Region patching with AWS Systems Manager Automation](http://aws.amazon.com/blogs/mt/centralized-multi-account-and-multi-region-patching-with-aws-systems-manager-automation/)