# Running automations with triggers using State Manager<a name="automation-sm-target"></a>

You can start an automation by creating a State Manager association with an Automation document\. By creating a State Manager association with an Automation document, you can target different types of AWS resources\. For example, you can create associations that enforce a desired state on an AWS resource, including the following:
+ Attach a Systems Manager role to EC2 instances to make them *managed instances*\.
+ Enforce desired ingress and egress rules for a security group\.
+ Create or delete Amazon DynamoDB \(DynamoDB\) backups\.
+ Create or delete Amazon Elastic Block Store \(Amazon EBS\) snapshots\.
+ Disable read and write permissions on Amazon Simple Storage Service \(Amazon S3\) buckets\.
+ Start, restart, or stop managed instances and Amazon Relational Database Service \(Amazon RDS\) instances\.
+ Patch Windows and Linux AMIs\.

Use the following procedures to create a State Manager association that runs an automation using the AWS Systems Manager console, AWS Command Line Interface \(AWS CLI\), or AWS Tools for Windows PowerShell\.

**Before You Begin**  
Be aware of the following important details before you run automation by using State Manager\.
+ Before you can create an association that runs an Automation document, verify that you configured permissions for Systems Manager Automation\. For more information, see [Getting started with Automation](automation-setup.md)\.
+ State Manager associations that run Automation documents contribute to the maximum number of concurrently running Automations in your AWS account\. You can have a maximum of 100 concurrent automations running\. For information, see [Systems Manager service quotas](https://docs.aws.amazon.com/general/latest/gr/ssm.html#limits_ssm) in the *Amazon Web Services General Reference*\.
+ Systems Manager automatically creates a service\-linked role so that State Manager has permission to call Systems Manager Automation API actions\. If you want, you can create the service\-linked role yourself by running the following command from the AWS CLI or AWS Tools for PowerShell\.

------
#### [ Linux ]

  ```
  aws iam create-service-linked-role \
      --aws-service-name ssm.amazonaws.com
  ```

------
#### [ Windows ]

  ```
  aws iam create-service-linked-role ^
      --aws-service-name ssm.amazonaws.com
  ```

------
#### [ PowerShell ]

  ```
  New-IAMServiceLinkedRole `
      -AWSServiceName ssm.amazonaws.com
  ```

------

  For more information about service\-linked roles, see [Using service\-linked roles for Systems Manager](using-service-linked-roles.md)\.

## Creating an association that runs an automation \(console\)<a name="automation-sm-target-console"></a>

The following procedure describes how to use the Systems Manager console to create a State Manager association that runs an automation\.

**To create a State Manager association that runs an automation**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **State Manager**, and then choose **Create association**\.

1. In the **Name** field, specify a name\. This is optional, but recommended\.

1. In the **Document** list, choose a document\. Use the Search bar to filter on **Document type : Equal : Automation** documents\. To view more Automation documents, use the numbers to the right of the Search bar\. 
**Note**  
You can view information about a document by choosing the document name\.

1. Choose **Simple execution** to run the automation on one or more targets by specifying the resource ID for those targets\. Choose **Rate control** to run the automation across a fleet of AWS resources by specifying a targeting option such as tags or AWS Resource Groups\. You can also control the execution of the automation across your resources by specifying concurrency and error thresholds\.

   If you chose **Rate control**, the **Targets** section appears\.

1. In the **Targets** section, choose a method for targeting resources\.

   1. \(Required\) In the **Parameter** list, choose a parameter\. The items in the **Parameter** list are determined by the parameters in the Automation document that you selected at the start of this procedure\. By choosing a parameter, you define the type of resource on which the automation runs\. 

   1. \(Required\) In the **Targets** list, choose a method for targeting the resources\.
      + **Resource Group**: Choose the name of the group from the **Resource Group** list\. For more information about targeting AWS Resource Groups in Automation documents, see [Targeting AWS Resource Groups](automation-working-targets.md#automation-working-targets-resource-groups)\.
      + **Tags**: Enter the tag key and \(optionally\) the tag value in the fields provided\. Choose **Add**\. For more information about targeting tags in Automation documents, see [Targeting tags](automation-working-targets.md#automation-working-targets-tags)\.
      + **Parameter Values**: Enter values in the **Input parameters** section\. If you specify multiple values, Systems Manager runs a child automation on each value specified\.

        For example, say that your Automation document includes an **InstanceID** parameter\. If you target the values of the **InstanceID** parameter when you run the automation, then Systems Manager runs a child automation for each instance ID value specified\. The parent automation is complete when the automation finishes running each specified instance, or if the automation fails\. You can target a maximum of 50 parameter values\. For more information about targeting parameter values in Automation documents, see [Targeting parameter values](automation-working-targets.md#automation-working-targets-parameter-values)\.

1. In the **Input parameters** section, specify the required input parameters\.

   If you chose to target resources by using tags or a resource group, then you may not need to choose some of the options in the **Input parameters** section\. For example, if you chose the AWS\-RestartEC2Instance document, and you chose to target instances by using tags, then you don't need to specify or choose instance IDs in the **Input parameters** section\. The automation locates the instances to restart by using the tags you specified\. 
**Important**  
You must specify a role ARN in the **AutomationAssumeRole** field\. State Manager uses the assume role to call AWS services specified in the Automation document and run Automation associations on your behalf\. For more information, see [Running an automation by using an IAM service role](automation-walk-security-assume.md)\. 

1. In the **Specify schedule** section, choose **On Schedule** if you want to run the association at regular intervals\. If you choose this option, then use the options provided to create the schedule using Cron or Rate expressions\. For more information about Cron and Rate expressions for State Manager, see [Cron and rate expressions for associations](reference-cron-and-rate-expressions.md#reference-cron-and-rate-expressions-association)\. 
**Note**  
Rate expressions are the preferred scheduling mechanism for State Manager associations that run Automation documents\. Rate expressions allow more flexibility for running associations in the event that you reach the maximum number of concurrently running automations\. With a rate schedule, Systems Manager can retry the automation shortly after receiving notification that concurrent automations have reached their maximum and have been throttled\.

   Choose **No schedule** if you want to run the association one time\. 

1. \(Optional\) In the **Rate Control** section, choose **Concurrency** and **Error threshold** options to control the automation deployment across your AWS resources\.

   1. In the **Concurrency** section, choose an option: 
      + Choose **targets** to enter an absolute number of targets that can run the automation simultaneously\.
      + Choose **percentage** to enter a percentage of the target set that can run the automation simultaneously\.

   1. In the **Error threshold** section, choose an option:
      + Choose **errors** to enter an absolute number of errors allowed before Automation stops sending the workflow to other resources\.
      + Choose **percentage** to enter a percentage of errors allowed before Automation stops sending the workflow to other resources\.

   For more information about using targets and rate controls with Automation, see [Running automations that use targets and rate controls](automation-working-targets-and-rate-controls.md)\.

1. Choose **Create Association**\. 
**Important**  
When you create an association, the association immediately runs against the specified targets\. The association then runs based on the cron or rate expression you chose\. If you chose **No schedule**, the association does not run again\.

## Creating an association that runs an automation \(command line\)<a name="automation-sm-target-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to create a State Manager association that runs an automation\.

**To create an association that runs an automation**

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

   Note the name of the Automation document that you want to use for the association\.

1. Run the following command to view details about the Automation document\.

------
#### [ Linux ]

   ```
   aws ssm describe-document \
       --name document_name
   ```

   Note a parameter name \(for example, `InstanceId`\) that you want to use for the `--automation-target-parameter-name` option\. This parameter determines the type of resource on which the automation runs\.

------
#### [ Windows ]

   ```
   aws ssm describe-document ^
       --name document_name
   ```

   Note a parameter name \(for example, `InstanceId`\) that you want to use for the `--automation-target-parameter-name` option\. This parameter determines the type of resource on which the automation runs\.

------
#### [ PowerShell ]

   ```
   Get-SSMDocumentDescription `
       -Name document_name
   ```

   Note a parameter name \(for example, `InstanceId`\) that you want to use for the `AutomationTargetParameterName` option\. This parameter determines the type of resource on which the automation runs\.

------

1. Create a command that runs an automation using a State Manager association\. Here are some template commands to help\.

   *Targeting using tags*

------
#### [ Linux ]

   ```
   aws ssm create-association \
       --association-name AssociationName \
       --targets Key=tag:TagKey,Values=TagValue \
       --name AutomationDocumentName \
       --parameters AutomationAssumeRole=arn:aws:iam::123456789012:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM,(Additional parameters, if any) \
       --automation-target-parameter-name (parameter to target) \
       --schedule "cron_or_rate_expression"
   ```

**Note**  
If you create an association by using the AWS CLI, use the `--targets` parameter to target instances for the association\. Don't use the `--instance-id` parameter\. The `--instance-id` parameter is a legacy parameter\. 

------
#### [ Windows ]

   ```
   aws ssm create-association ^
       --association-name AssociationName ^
       --targets Key=tag:TagKey,Values=TagValue ^
       --name AutomationDocumentName ^
       --parameters AutomationAssumeRole=arn:aws:iam::123456789012:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM,(Additional parameters, if any) ^
       --automation-target-parameter-name (parameter to target) ^
       --schedule "cron_or_rate_expression"
   ```

**Note**  
If you create an association by using the AWS CLI, use the `--targets` parameter to target instances for the association\. Don't use the `--instance-id` parameter\. The `--instance-id` parameter is a legacy parameter\. 

------
#### [ PowerShell ]

   ```
   $Targets = New-Object Amazon.SimpleSystemsManagement.Model.Target
   $Targets.Key = "tag:TagKey"
   $Targets.Values = "TagValue"
   
   New-SSMAssociation `
       -AssociationName "AssociationName" `
       -Target $Targets `
       -Name "AutomationDocumentName" `
       -Parameters @{
       "AutomationAssumeRole"="arn:aws:iam::123456789012:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM;
       (Additional parameters, if any)" } `
       -AutomationTargetParameterName "parameter_to_target" `
       -ScheduleExpression "cron_or_rate_expression"
   ```

**Note**  
If you create an association by using the AWS Tools for PowerShell, use the `Target` parameter to target instances for the association\. Don't use the `InstanceId` parameter\. The `InstanceId` parameter is a legacy parameter\. 

------

   *Targeting using parameter values*

------
#### [ Linux ]

   ```
   aws ssm create-association \
       --association-name AssociationName \
       --targets Key=ParameterValues,Values=value_1,value_2,value_3 \
       --name AutomationDocumentName \
       --parameters AutomationAssumeRole=arn:aws:iam::123456789012:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM,(Additional parameters, if any) \
       --automation-target-parameter-name (parameter to target) \
       --schedule "cron_or_rate_expression"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
       --association-name AssociationName ^
       --targets Key=ParameterValues,Values=value_1,value_2,value_3 ^
       --name AutomationDocumentName ^
       --parameters AutomationAssumeRole=arn:aws:iam::123456789012:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM,(Additional parameters, if any) ^
       --automation-target-parameter-name (parameter to target) ^
       --schedule "cron_or_rate_expression"
   ```

------
#### [ PowerShell ]

   ```
   $Targets = New-Object Amazon.SimpleSystemsManagement.Model.Target
   $Targets.Key = "ParameterValues"
   $Targets.Values = "value_1","value_2","value_3"
   
   New-SSMAssociation `
       -AssociationName "AssociationName" `
       -Target $Targets `
       -Name "AutomationDocumentName" `
       -Parameters @{
       "AutomationAssumeRole"="arn:aws:iam::123456789012:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM;
       (Additional parameters, if any)"} `
       -AutomationTargetParameterName "parameter_to_target" `
       -ScheduleExpression "cron_or_rate_expression"
   ```

------

   *Targeting using AWS Resource Groups*

------
#### [ Linux ]

   ```
   aws ssm create-association \
       --association-name AssociationName \
       --targets Key=ResourceGroup,Values=Resource_Group_name \
       --name AutomationDocumentName \
       --parameters AutomationAssumeRole=arn:aws:iam::123456789012:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM,(Additional parameters, if any) \
       --automation-target-parameter-name (parameter to target) \
       --schedule "cron_or_rate_expression"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
       --association-name AssociationName ^
       --targets Key=ResourceGroup,Values=Resource_Group_name ^
       --name AutomationDocumentName ^
       --parameters AutomationAssumeRole=arn:aws:iam::123456789012:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM,(Additional parameters, if any) ^
       --automation-target-parameter-name (parameter to target) ^
       --schedule "cron_or_rate_expression"
   ```

------
#### [ PowerShell ]

   ```
   $Targets = New-Object Amazon.SimpleSystemsManagement.Model.Target
   $Targets.Key = "ResourceGroup"
   $Targets.Values = "Resource_Group_Name"
   
   New-SSMAssociation `
       -AssociationName "AssociationName" `
       -Target $Targets `
       -Name "AutomationDocumentName" `
       -Parameters @{
       "AutomationAssumeRole"="arn:aws:iam::123456789012:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM;
       (Additional parameters, if any)"} `
       -AutomationTargetParameterName "parameter_to_target" `
       -ScheduleExpression "cron_or_rate_expression"
   ```

------

   The command returns details for the new association similar to the following\.

------
#### [ Linux ]

   ```
   {
       "AssociationDescription": {
           "ScheduleExpression": "cron(0 7 ? * MON *)",
           "Name": "AWS-StartEC2Instance",
           "Parameters": {
               "AutomationAssumeRole": [
                   "arn:aws:iam::123456789012:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM"
               ]
           },
           "Overview": {
               "Status": "Pending",
               "DetailedStatus": "Creating"
           },
           "AssociationId": "1450b4b7-bea2-4e4b-b340-01234EXAMPLE",
           "DocumentVersion": "$DEFAULT",
           "AutomationTargetParameterName": "InstanceId",
           "LastUpdateAssociationDate": 1564686638.498,
           "Date": 1564686638.498,
           "AssociationVersion": "1",
           "AssociationName": "CLI",
           "Targets": [
               {
                   "Values": [
                       "DEV"
                   ],
                   "Key": "tag:ENV"
               }
           ]
       }
   }
   ```

------
#### [ Windows ]

   ```
   {
       "AssociationDescription": {
           "ScheduleExpression": "cron(0 7 ? * MON *)",
           "Name": "AWS-StartEC2Instance",
           "Parameters": {
               "AutomationAssumeRole": [
                   "arn:aws:iam::123456789012:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM"
               ]
           },
           "Overview": {
               "Status": "Pending",
               "DetailedStatus": "Creating"
           },
           "AssociationId": "1450b4b7-bea2-4e4b-b340-01234EXAMPLE",
           "DocumentVersion": "$DEFAULT",
           "AutomationTargetParameterName": "InstanceId",
           "LastUpdateAssociationDate": 1564686638.498,
           "Date": 1564686638.498,
           "AssociationVersion": "1",
           "AssociationName": "CLI",
           "Targets": [
               {
                   "Values": [
                       "DEV"
                   ],
                   "Key": "tag:ENV"
               }
           ]
       }
   }
   ```

------
#### [ PowerShell ]

   ```
   Name                  : AWS-StartEC2Instance
   InstanceId            : 
   Date                  : 8/1/2019 7:31:38 PM
   Status.Name           : 
   Status.Date           : 
   Status.Message        : 
   Status.AdditionalInfo :
   ```

------

**Note**  
If you use tags to create an association on one or more target instances, and then you remove the tags from an instance, that instance no longer runs the association\. The instance is disassociated from the State Manager document\. 

## Troubleshooting State Manager automations<a name="systems-manager-state-manager-automation-documents-troubleshooting"></a>

Systems Manager Automation enforces a limit of 100 concurrent automations, and 1,000 queued automations per account, per Region\. If a State Manager association that runs an Automation document shows a status of **Failed** and a detailed status of **AutomationExecutionLimitExceeded**, then your execution may have reached the limit\. As a result, Systems Manager throttles the executions\. To resolve this issue, do the following:
+ Use a different rate or cron expression for your association\. For example, if the association is scheduled to run every 30 minutes, then change the expression so that it runs every hour or two\.
+ Delete existing automations that have a status of **Pending**\. By deleting these automations, you clear the current queue\.