# Create an association<a name="sysman-state-assoc"></a>

The following procedures describe how to create a State Manager association by using the AWS Systems Manager console, AWS Command Line Interface \(AWS CLI\), and AWS Tools for Windows PowerShell\.

**Important**  
The following procedures describe how to create an association that uses either a `Command` or a `Policy` document\. For information about creating an association that uses an `Automation` document, see [Running automations with triggers using State Manager](automation-sm-target.md)\.

When you create a State Manager association, by default, the system immediately runs it on the specified instances or targets\. After the initial run, the association runs in intervals according to the schedule that you defined and according to the following rules:
+ Associations only run on instances that are online when the interval starts\. Offline instances are skipped\.
+ State Manager attempts to run the association on all specified or targeted instances during an interval\.
+ If an association doesn't run during an interval \(because, for example, a concurrency value limited the number of instances that could process the association at one time\), then State Manager attempts to run the association during the next interval\.
+ State Manager records history for all skipped intervals\. You can view the history on the **Execution History** tab\.

**Note**  
If you don't want an association to run immediately after you create it, you can choose the **Apply association only at the next specified Cron interval** option in the Systems Manager console\. 

The following procedure describes how to use targets and rate controls when creating an association\. For more information about these features, see [About targets and rate controls in State Manager associations](systems-manager-state-manager-targets-and-rate-controls.md)\.

**Warning**  
An AWS Identity and Access Management \(IAM\) user, group, or role with permission to create an association that targets a resource group of Amazon Elastic Compute Cloud \(Amazon EC2\) instances automatically has root\-level control of all instances in the group\. Only trusted administrators should be permitted to create associations\. 

## Create an association \(console\)<a name="sysman-state-assoc-console"></a>

The following procedure describes how to use the Systems Manager console to create a State Manager association\.

**To create a State Manager association**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **State Manager**, and then choose **Create association**\.

1. In the **Name** field, specify a name\. This is optional, but recommended\. A name helps you remember the purpose of the association\. For example, you could specify **Automatically\_update\_AWSPVDrivers\_on\_us\-west\-2\_instances** for an association with that purpose\. Spaces aren't allowed in the name\.

1. In the **Document** list, choose the option next to a document name\. Note the document type\. This procedure applies only to `Command` and `Policy` documents\. For information about creating an association that uses an `Automation` document, see [Running automations with triggers using State Manager](automation-sm-target.md)\.

1. For **Parameters**, specify the required input parameters\.

1. For **Targets**, choose an option\. For information about using targets, see [About targets and rate controls in State Manager associations](systems-manager-state-manager-targets-and-rate-controls.md)\.

1. In the **Specify schedule** section, choose either **On Schedule** or **No schedule**\. If you choose **On Schedule**, use the buttons provided to create a cron or rate schedule for the association\. 

   If you don't want the association to run immediately after you create it, choose **Apply association only at the next specified Cron interval**\. 

1. In the **Advanced options** section use **Compliance severity** to choose a severity level for the association\. Compliance reporting indicates whether the association state is compliant or noncompliant, along with the severity level you indicate here\. For more information, see [About State Manager association compliance](sysman-compliance-about.md#sysman-compliance-about-association)\.

1. In the **Rate control** section, choose options to control how the association runs on multiple instances\. For more information about using rate controls, see [About targets and rate controls in State Manager associations](systems-manager-state-manager-targets-and-rate-controls.md)\.

   In the **Concurrency** section, choose an option: 
   + Choose **targets** to enter an absolute number of targets that can run the association simultaneously\.
   + Choose **percentage** to enter a percentage of the target set that can run the association simultaneously\.

   In the **Error threshold** section, choose an option:
   + Choose **errors** to enter an absolute number of errors that are allowed before State Manager stops running associations on additional targets\.
   + Choose **percentage** to enter a percentage of errors that are allowed before State Manager stops running associations on additional targets\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Enable writing output to S3** box\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. In addition, if the specified S3 bucket is in a different AWS account, ensure that the instance profile associated with the instance has the necessary permissions to write to that bucket\.

1. Choose **Create Association**\.

**Note**  
If you delete the association you created, the association no longer runs on any targets of that association\.

## Create an association \(command line\)<a name="create-state-manager-association-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to create a State Manager association\. This section includes several examples that show how to use targets and rate controls\. Targets and rate controls enable you to assign an association to dozens or hundreds of instances while controlling how those associations run\. For more information about targets and rate controls, see [About targets and rate controls in State Manager associations](systems-manager-state-manager-targets-and-rate-controls.md)\.

**Before you begin**  
The `targets` parameter is an array of search criteria that targets instances using a `Key`,`Value` combination that you specify\. If you plan to create an association on dozens or hundreds of instance by using the `targets` parameter, review the following targeting options before you begin the procedure\.

Target a few instances by specifying IDs

```
--targets Key=InstanceIds,Values=instance-id-1,instance-id-2,instance-id-3
```

```
--targets Key=InstanceIds,Values=i-02573cafcfEXAMPLE,i-0471e04240EXAMPLE,i-07782c72faEXAMPLE
```

Target instances by using Amazon EC2 tags

```
--targets Key=tag:tag-key,Values=tag-value-1,tag-value-2,tag-value-3
```

```
--targets Key=tag:Environment,Values=Development,Test,Pre-production
```

Target instances by using AWS Resource Groups

```
--targets Key=resource-groups:Name,Values=resource-group-name
```

```
--targets Key=resource-groups:Name,Values=WindowsInstancesGroup
```

Target all instances in the current AWS account and AWS Region

```
--targets Key=InstanceIds,Values=*
```

**Note**  
When you create an association, you specify when the schedule runs\. You must specify the schedule by using a cron or rate expression\. For more information about cron and rate expressions, see [Cron and rate expressions for associations](reference-cron-and-rate-expressions.md#reference-cron-and-rate-expressions-association)\.

**To create an association**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Use the following format to create a command that creates a State Manager association\.

------
#### [ Linux ]

   ```
   aws ssm create-association \
     --targets target_options \
     --name document_name \
     --schedule "cron_or_rate_expression" \
     --parameters (if any) \
     --max-concurrency a_number_of_instances_or_a_percentage_of_target_set \
     --max-errors a_number_of_errors_or_a_percentage_of_target_set
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
     --targets target_options ^
     --name document_name ^
     --schedule "cron_or_rate_expression" ^
     --parameters (if any) ^
     --max-concurrency a_number_of_instances_or_a_percentage_of_target_set ^
     --max-errors a_number_of_errors_or_a_percentage_of_target_set
   ```

------
#### [ PowerShell ]

   ```
   New-SSMAssociation `
     -AssociationName document_name `
     -Target target_options `
     -ScheduleExpression "cron_or_rate_expression" `
     -Parameters (if any) `
     -MaxConcurrency a_number_of_instances_or_a_percentage_of_target_set `
     -MaxError  a_number_of_errors_or_a_percentage_of_target_set
   ```

------

   The following example creates an association on instances tagged with `"Environment,Linux"`\. The association uses the AWS\-UpdateSSMAgent document to update SSM Agent on the targeted instances at 2:00 AM every Sunday\. This association runs simultaneously on 10 instances maximum at any given time\. Also, this association stops running on more instances for a particular run interval if the error count exceeds 5\. For compliance reporting, this association is assigned a severity level of Medium\.

------
#### [ Linux ]

   ```
   aws ssm create-association \
     --association-name Update_SSM_Agent_Linux \
     --targets Key=tag:Environment,Values=Linux \
     --name AWS-UpdateSSMAgent  \
     --compliance-severity "MEDIUM" \
     --schedule "cron(0 2 ? * SUN *)" \
     --max-errors "5" \
     --max-concurrency "10"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
     --association-name Update_SSM_Agent_Linux ^
     --targets Key=tag:Environment,Values=Linux ^
     --name AWS-UpdateSSMAgent  ^
     --compliance-severity "MEDIUM" ^
     --schedule "cron(0 2 ? * SUN *)" ^
     --max-errors "5" ^
     --max-concurrency "10"
   ```

------
#### [ PowerShell ]

   ```
   New-SSMAssociation `
     -AssociationName Update_SSM_Agent_Linux `
     -Name AWS-UpdateSSMAgent `
     -Target @{
         "Key"="tag:Environment"
         "Values"="Linux"
       } `
     -ScheduleExpression "cron(0 2 ? * SUN *)" `
     -MaxConcurrency 10 `
     -MaxError 5 `
     -ComplianceSeverity MEDIUM
   ```

------

   The following commands creates an association that scans instances for missing patch updates by using the AWS\-RunPatchBaseline document\. This association targets all managed instances in the account in the us\-east\-2 Region\. The association specifies the `Operation` and `RebootOption` parameters\.

------
#### [ Linux ]

   ```
   aws ssm create-association \
   --name "AWS-RunPatchBaseline" \
   --association-name "ScanningInstancesForMissingUpdate" \
   --targets "Key=instanceids,Values=*" \
   --parameters "Operation=Scan,RebootOption=NoReboot" \
   --region us-east-2
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
   --name "AWS-RunPatchBaseline" ^
   --association-name "ScanningInstancesForMissingUpdate" ^
   --targets "Key=instanceids,Values=*" ^
   --parameters "Operation=Scan,RebootOption=NoReboot" ^
   --region us-east-2
   ```

------
#### [ PowerShell ]

   ```
   New-SSMAssociation `
   -AssociationName ScanningInstancesForMissingUpdate `
   -Name AWS-RunPatchBaseline `
   -Target @{
         "Key"="instanceids"
         "Values"="*"
       } `
   -Parameters "Operation=Scan,RebootOption=NoReboot" `
   -Region us-east-2
   ```

------

   The following example targets instance IDs by specifying a wildcard value \(\*\)\. This enables Systems Manager to create an association on *all* instances in the current account and Region\. This association runs simultaneously on 10 instances maximum at any given time\. Also, this association stops running on more instances for a particular interval if the error count exceeds 5\. For compliance reporting, this association is assigned a severity level of Medium\. This association runs only at the specified Cron schedule\. It doesn't run immediately after the association is created\.

------
#### [ Linux ]

   ```
   aws ssm create-association \
     --association-name Update_SSM_Agent_Linux \
     --name "AWS-UpdateSSMAgent" \
     --targets "Key=instanceids,Values=*" \
     --compliance-severity "MEDIUM" \
     --schedule "cron(0 2 ? * SUN *)" \
     --max-errors "5" \
     --max-concurrency "10" \
     --apply-only-at-cron-interval
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
         --association-name Update_SSM_Agent_Linux ^
         --name "AWS-UpdateSSMAgent" ^
         --targets "Key=instanceids,Values=*" ^
         --compliance-severity "MEDIUM" ^
         --schedule "cron(0 2 ? * SUN *)" ^
         --max-errors "5" ^
         --max-concurrency "10" ^
         --apply-only-at-cron-interval
   ```

------
#### [ PowerShell ]

   ```
   New-SSMAssociation `
     -AssociationName Update_SSM_Agent_All `
     -Name AWS-UpdateSSMAgent `
     -Target @{
         "Key"="InstanceIds"
         "Values"="*"
       } `
     -ScheduleExpression "cron(0 2 ? * SUN *)" `
     -MaxConcurrency 10 `
     -MaxError 5 `
     -ComplianceSeverity MEDIUM `
     -ApplyOnlyAtCronInterval
   ```

------

   The following example creates an association on instances in AWS Resource Groups\. The group is named "HR\-Department"\. The association uses the AWS\-UpdateSSMAgent document to update SSM Agent on the targeted instances at 2:00 AM every Sunday\. This association runs simultaneously on 10 instances maximum at any given time\. Also, this association stops running on more instances for a particular execution interval if the error count exceeds 5\. For compliance reporting, this association is assigned a severity level of Medium\. This association runs only at the specified Cron schedule\. It doesn't run immediately after the association is created\.

------
#### [ Linux ]

   ```
   aws ssm create-association \
     --association-name Update_SSM_Agent_Linux \
     --targets Key=resource-groups:Name,Values=HR-Department \
     --name AWS-UpdateSSMAgent  \
     --compliance-severity "MEDIUM" \
     --schedule "cron(0 2 ? * SUN *)" \
     --max-errors "5" \
     --max-concurrency "10" \
     --apply-only-at-cron-interval
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
     --association-name Update_SSM_Agent_Linux ^
     --targets Key=resource-groups:Name,Values=HR-Department ^
     --name AWS-UpdateSSMAgent  ^
     --compliance-severity "MEDIUM" ^
     --schedule "cron(0 2 ? * SUN *)" ^
     --max-errors "5" ^
     --max-concurrency "10" ^
     --apply-only-at-cron-interval
   ```

------
#### [ PowerShell ]

   ```
   New-SSMAssociation `
     -AssociationName Update_SSM_Agent_Linux `
     -Name AWS-UpdateSSMAgent `
     -Target @{
         "Key"="resource-groups:Name"
         "Values"="HR-Department"
       } `
     -ScheduleExpression "cron(0 2 ? * SUN *)" `
     -MaxConcurrency 10 `
     -MaxError 5 `
     -ComplianceSeverity MEDIUM `
     -ApplyOnlyAtCronInterval
   ```

------

The following example creates an association that runs on instances tagged with a specific instance id\. The association uses the SSM Agent document to update SSM Agent on the targeted instances just once, at 3:55 PM UTC time on July 7, 2020\. This association runs simultaneously on 10 instances maximum at any given time\. Also, this association stops running on more instances for a particular execution interval if the error count exceeds 5\. For compliance reporting, this association is assigned a severity level of Medium\. This association runs only at the specified Cron schedule\. It doesn't run immediately after the association is created\.

**Note**  
If you use an `at()` expression the association will only run once\. It will not be applied to any new instances coming online into the tags or resource group of that association\. Associations created with a date in the past or present \(by the time it is processed the date is in the past\) run immediately\.

------
#### [ Linux ]

```
aws ssm create-association \
  --association-name One-Time-Association \
  --targets "Key=instanceids,Values=i-0cb2b964d3e14fd9f" \
  --name AWS-UpdateSSMAgent  \
  --compliance-severity "MEDIUM" \
  --schedule "at(2020-07-07T15:55:00)" \
  --max-errors "5" \
  --max-concurrency "10" \
  --apply-only-at-cron-interval
```

------
#### [ Windows ]

```
aws ssm create-association ^
  --association-name Update_SSM_Agent_Linux ^
  --targets "Key=instanceids,Values=i-0cb2b964d3e14fd9f" ^
  --name AWS-UpdateSSMAgent  ^
  --compliance-severity "MEDIUM" ^
  --schedule "at(2020-07-07T15:55:00)" ^
  --max-errors "5" ^
  --max-concurrency "10" ^
  --apply-only-at-cron-interval
```

------
#### [ PowerShell ]

```
New-SSMAssociation `
  -AssociationName Update_SSM_Agent_Linux `
  -Name AWS-UpdateSSMAgent `
  -Target @{
      "Key"="tag:instanceids"
      "Values"="i-0cb2b964d3e14fd9f"
    } `
  -ScheduleExpression "at(2020-07-07T15:55:00)" `
  -MaxConcurrency 10 `
  -MaxError 5 `
  -ComplianceSeverity MEDIUM `
  -ApplyOnlyAtCronInterval
```

------

**Note**  
If you delete the association you created, the association no longer runs on any targets of that association\. Also, if you specified the `apply-only-at-cron-interval` parameter, you can reset this option\. To do so, specify the `no-apply-only-at-cron-interval` parameter when you update the association from the command line\. This parameter forces the association to run immediately after updating the association and according to the interval specified\.