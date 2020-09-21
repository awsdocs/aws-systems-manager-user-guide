# Edit and create a new version of an association<a name="sysman-state-assoc-edit"></a>

You can edit an association to specify a new name, schedule, severity level, or targets\. You can also choose to write the output of the command to an Amazon Simple Storage Service \(Amazon S3\) bucket\. After you edit an association, Systems Manager creates a new version\. You can view different versions after editing, as described in the following procedures\. 

The following procedures describe how to edit and create a new version of an association using the AWS Systems Manager console, AWS Command Line Interface \(AWS CLI\), and AWS Tools for PowerShell\. 

## Edit an association \(console\)<a name="sysman-state-assoc-edit-console"></a>

The following procedure describes how to use the Systems Manager console to edit and create a new version of an association\.

**Note**  
This procedure requires that you have write access to an existing S3 bucket\. If you have not used Amazon S3 before, be aware that you will incur charges for using Amazon S3\. For information about how to create a bucket, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)\.

**To edit a State Manager association**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **State Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose ** State Manager**\.

1. Choose the association you created in [ Create an association \(command line\)  The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to create a State Manager association\. This section includes several examples that show how to use targets and rate controls\. Targets and rate controls enable you to assign an association to dozens or hundreds of instances while controlling how those associations run\. For more information about targets and rate controls, see [About targets and rate controls in State Manager associations](systems-manager-state-manager-targets-and-rate-controls.md)\.  Before you begin The `targets` parameter is an array of search criteria that targets instances using a `Key`,`Value` combination that you specify\. If you plan to create an association on dozens or hundreds of instance by using the `targets` parameter, review the following targeting options before you begin the procedure\.  Target a few instances by specifying IDs 

```
--targets Key=InstanceIds,Values=instance-id-1,instance-id-2,instance-id-3
``` 

```
--targets Key=InstanceIds,Values=i-02573cafcfEXAMPLE,i-0471e04240EXAMPLE,i-07782c72faEXAMPLE
``` Target instances by using Amazon EC2 tags 

```
--targets Key=tag:tag-key,Values=tag-value-1,tag-value-2,tag-value-3
``` 

```
--targets Key=tag:Environment,Values=Development,Test,Pre-production
``` Target instances by using AWS Resource Groups 

```
--targets Key=resource-groups:Name,Values=resource-group-name
``` 

```
--targets Key=resource-groups:Name,Values=WindowsInstancesGroup
``` Target all instances in the current AWS account and AWS Region 

```
--targets Key=InstanceIds,Values=*
```  When you create an association, you specify when the schedule runs\. You must specify the schedule by using a cron or rate expression\. For more information about cron and rate expressions, see [Cron and rate expressions for associations](reference-cron-and-rate-expressions.md#reference-cron-and-rate-expressions-association)\.  To create an association Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\. For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.  Use the following format to create a command that creates a State Manager association\.   Linux  

   ```
   aws ssm create-association \
     --targets target_options \
     --name document_name \
     --schedule "cron_or_rate_expression" \
     --parameters (if any) \
     --max-concurrency a_number_of_instances_or_a_percentage_of_target_set \
     --max-errors a_number_of_errors_or_a_percentage_of_target_set
   ```    Windows  

   ```
   aws ssm create-association ^
     --targets target_options ^
     --name document_name ^
     --schedule "cron_or_rate_expression" ^
     --parameters (if any) ^
     --max-concurrency a_number_of_instances_or_a_percentage_of_target_set ^
     --max-errors a_number_of_errors_or_a_percentage_of_target_set
   ```    PowerShell  

   ```
   New-SSMAssociation `
     -AssociationName document_name `
     -Target target_options `
     -ScheduleExpression "cron_or_rate_expression" `
     -Parameters (if any) `
     -MaxConcurrency a_number_of_instances_or_a_percentage_of_target_set `
     -MaxError  a_number_of_errors_or_a_percentage_of_target_set
   ```    The following example creates an association on instances tagged with `"Environment,Linux"`\. The association uses the AWS\-UpdateSSMAgent document to update SSM Agent on the targeted instances at 2:00 AM every Sunday\. This association runs simultaneously on 10 instances maximum at any given time\. Also, this association stops running on more instances for a particular run interval if the error count exceeds 5\. For compliance reporting, this association is assigned a severity level of Medium\.   Linux  

   ```
   aws ssm create-association \
     --association-name Update_SSM_Agent_Linux \
     --targets Key=tag:Environment,Values=Linux \
     --name AWS-UpdateSSMAgent  \
     --compliance-severity "MEDIUM" \
     --schedule "cron(0 2 ? * SUN *)" \
     --max-errors "5" \
     --max-concurrency "10"
   ```    Windows  

   ```
   aws ssm create-association ^
     --association-name Update_SSM_Agent_Linux ^
     --targets Key=tag:Environment,Values=Linux ^
     --name AWS-UpdateSSMAgent  ^
     --compliance-severity "MEDIUM" ^
     --schedule "cron(0 2 ? * SUN *)" ^
     --max-errors "5" ^
     --max-concurrency "10"
   ```    PowerShell  

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
   ```    The following commands creates an association that scans instances for missing patch updates by using the AWS\-RunPatchBaseline document\. This association targets all managed instances in the account in the us\-east\-2 Region\. The association specifies the `Operation` and `RebootOption` parameters\.   Linux  

   ```
   aws ssm create-association \
   --name "AWS-RunPatchBaseline" \
   --association-name "ScanningInstancesForMissingUpdate" \
   --targets "Key=instanceids,Values=*" \
   --parameters "Operation=Scan,RebootOption=NoReboot" \
   --region us-east-2
   ```    Windows  

   ```
   aws ssm create-association ^
   --name "AWS-RunPatchBaseline" ^
   --association-name "ScanningInstancesForMissingUpdate" ^
   --targets "Key=instanceids,Values=*" ^
   --parameters "Operation=Scan,RebootOption=NoReboot" ^
   --region us-east-2
   ```    PowerShell  

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
   ```    The following example targets instance IDs by specifying a wildcard value \(\*\)\. This enables Systems Manager to create an association on *all* instances in the current account and Region\. This association runs simultaneously on 10 instances maximum at any given time\. Also, this association stops running on more instances for a particular interval if the error count exceeds 5\. For compliance reporting, this association is assigned a severity level of Medium\. This association runs only at the specified Cron schedule\. It doesn't run immediately after the association is created\.   Linux  

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
   ```    Windows  

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
   ```    PowerShell  

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
   ```    The following example creates an association on instances in AWS Resource Groups\. The group is named "HR\-Department"\. The association uses the AWS\-UpdateSSMAgent document to update SSM Agent on the targeted instances at 2:00 AM every Sunday\. This association runs simultaneously on 10 instances maximum at any given time\. Also, this association stops running on more instances for a particular execution interval if the error count exceeds 5\. For compliance reporting, this association is assigned a severity level of Medium\. This association runs only at the specified Cron schedule\. It doesn't run immediately after the association is created\.   Linux  

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
   ```    Windows  

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
   ```    PowerShell  

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
   ```      The following example creates an association that runs on instances tagged with a specific instance id\. The association uses the SSM Agent document to update SSM Agent on the targeted instances just once, at 3:55 PM UTC time on July 7, 2020\. This association runs simultaneously on 10 instances maximum at any given time\. Also, this association stops running on more instances for a particular execution interval if the error count exceeds 5\. For compliance reporting, this association is assigned a severity level of Medium\. This association runs only at the specified Cron schedule\. It doesn't run immediately after the association is created\.  If you use an `at()` expression the association will only run once\. It will not be applied to any new instances coming online into the tags or resource group of that association\. Associations created with a date in the past or present \(by the time it is processed the date is in the past\) run immediately\.    Linux  

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
```    Windows  

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
```    PowerShell  

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
```     If you delete the association you created, the association no longer runs on any targets of that association\. Also, if you specified the `apply-only-at-cron-interval` parameter, you can reset this option\. To do so, specify the `no-apply-only-at-cron-interval` parameter when you update the association from the command line\. This parameter forces the association to run immediately after updating the association and according to the interval specified\.  ](sysman-state-assoc.md#create-state-manager-association-commandline) and then choose **Edit**\.

1. In the **Name** field, type a new name\. For example, type **TestHostnameAssociation2**\.

1. In the **Specify schedule** section, choose a new option\. For example, choose **CRON schedule builder**, and then choose **Every 1 hour**\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Enable writing output to S3** box\. Type the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the instance, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\. In addition, if the specified S3 bucket is in a different AWS account, ensure that the instance profile associated with the instance has the necessary permissions to write to that bucket\.

1. Choose **Edit association**\.

1. In the **Associations** page, choose the name of the association you just edited, and then choose the **Versions** tab\. The system lists each version of the association you created and edited\.

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the name of the S3 bucket you specified for storing command output, and then choose the folder named with the ID of the instance that ran the association\. \(If you chose to store output in a folder in the bucket, open it first\.\)

1. Drill down several levels, through the `awsrunPowerShell` folder, to the `stdout` file\.

1. Choose **Open** or **Download** to view the host name\.

## Edit an association \(command line\)<a name="sysman-state-assoc-edit-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to edit and create a new version of an association\.

**To edit a State Manager association**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Use the following format to create a command to edit and create a new version of an existing State Manager association\.

------
#### [ Linux ]

   ```
   aws ssm update-association \
     --association-id b85ccafe-9f02-4812-9b81-01234EXAMPLE \
     --association-name association_name \
     --parameters (if any) \
     --output-location S3Location='{OutputS3Region=region,OutputS3BucketName=DOC-EXAMPLE-BUCKET,OutputS3KeyPrefix=keyprefix}' \
     --scheduleexpression "cron_or_rate_expression"
   ```

**Important**  
To retain existing parameter values of your association, such as association name or compliance severity, you must specify these values when you update the association\. If you don't specify these parameter values when you update an association, the new association version uses no values\. For example, if your existing association has a cron schedule but you don't specify `--schedule-expression` when updating, the new association version will not have a schedule expression\.

------
#### [ Windows ]

   ```
   aws ssm update-association ^
     --association-id b85ccafe-9f02-4812-9b81-01234EXAMPLE ^
     --association-name association_name ^
     --parameters (if any) ^
     --output-location S3Location='{OutputS3Region=region,OutputS3BucketName=DOC-EXAMPLE-BUCKET,OutputS3KeyPrefix=keyprefix}' ^
     --scheduleexpression "cron_or_rate_expression"
   ```

**Important**  
To retain existing parameter values of your association, such as association name or compliance severity, you must specify these values when you update the association\. If you don't specify these parameters when you update an association, the new association version uses the default values \(none\)\. For example, if your existing association has a cron schedule but you don't specify `--schedule-expression` when updating, the new association version will not have a schedule expression\.

------
#### [ PowerShell ]

   ```
   Update-SSMAssociation `
     -AssociationId b85ccafe-9f02-4812-9b81-01234EXAMPLE `
     -AssociationName document_name `
     -Parameter (if any) `
     -S3Location_OutputS3BucketName DOC-EXAMPLE-BUCKET `
     -S3Location_OutputS3KeyPrefix key_prefix `
     -S3Location_OutputS3Region region `
     -ScheduleExpression "cron_or_rate_expression"
   ```

**Important**  
To retain existing parameter values of your association, such as association name or compliance severity, you must specify these values when you update the association\. If you don't specify these parameters when you update an association, the new association version uses no values\. For example, if your existing association has a cron schedule but you don't specify `-ScheduleExpression` when updating, the new association version will not have a schedule expression\.

------

   The following example updates an existing association to change the name to `TestHostnameAssociation2`\. The new association version runs every hour and writes the output of commands to the specified Amazon S3 bucket\.

------
#### [ Linux ]

   ```
   aws ssm update-association \
     --association-id 8dfe3659-4309-493a-8755-01234EXAMPLE \
     --association-name TestHostnameAssociation2 \
     --parameters commands="echo Association" \
     --output-location S3Location='{OutputS3Region=us-east-1,OutputS3BucketName=DOC-EXAMPLE-BUCKET,OutputS3KeyPrefix=logs}' \
     --schedule-expression "cron(0 */1 * * ? *)"
   ```

------
#### [ Windows ]

   ```
   aws ssm update-association ^
     --association-id 8dfe3659-4309-493a-8755-01234EXAMPLE ^
     --association-name TestHostnameAssociation2 ^
     --parameters commands="echo Association" ^
     --output-location S3Location='{OutputS3Region=us-east-1,OutputS3BucketName=DOC-EXAMPLE-BUCKET,OutputS3KeyPrefix=logs}' ^
     --schedule-expression "cron(0 */1 * * ? *)"
   ```

------
#### [ PowerShell ]

   ```
   Update-SSMAssociation `
     -AssociationId b85ccafe-9f02-4812-9b81-01234EXAMPLE `
     -AssociationName TestHostnameAssociation2 `
     -Parameter @{"commands"="echo Association"} `
     -S3Location_OutputS3BucketName DOC-EXAMPLE-BUCKET `
     -S3Location_OutputS3KeyPrefix logs `
     -S3Location_OutputS3Region us-east-1 `
     -ScheduleExpression "cron(0 */1 * * ? *)"
   ```

------

   The following example updates an existing association to change the name to `OneTimeAssociation`\. The new association version runs just once, at 3:55 PM UTC time on July 7, 2020, and writes the output of commands to the specified Amazon S3 bucket\. 

------
#### [ Linux ]

   ```
   aws ssm update-association \
     --association-id 8dfe3659-4309-493a-8755-01234EXAMPLE \
     --association-name OneTimeAssociation \
     --parameters commands="echo Association" \
     --output-location S3Location='{OutputS3Region=us-east-1,OutputS3BucketName=DOC-EXAMPLE-BUCKET,OutputS3KeyPrefix=logs}' \
     --schedule "at(2020-07-07T15:55:00)" \
     --apply-only-at-cron-interval
   ```

------
#### [ Windows ]

   ```
   aws ssm update-association ^
     --association-id 8dfe3659-4309-493a-8755-01234EXAMPLE ^
     --association-name OneTimeAssociation ^
     --parameters commands="echo Association" ^
     --output-location S3Location='{OutputS3Region=us-east-1,OutputS3BucketName=DOC-EXAMPLE-BUCKET,OutputS3KeyPrefix=logs}' ^
     --schedule "at(2020-07-07T15:55:00)" ^
     --apply-only-at-cron-interval
   ```

------
#### [ PowerShell ]

   ```
   Update-SSMAssociation `
     -AssociationId b85ccafe-9f02-4812-9b81-01234EXAMPLE `
     -AssociationName OneTimeAssociation `
     -Parameter @{"commands"="echo Association"} `
     -S3Location_OutputS3BucketName DOC-EXAMPLE-BUCKET `
     -S3Location_OutputS3KeyPrefix logs `
     -S3Location_OutputS3Region us-east-1 `
     -ScheduleExpression "at(2020-07-07T15:55:00) `
     -ApplyOnlyAtCronInterval
   ```

------

1. To view the new version of the association, run the following command\.

------
#### [ Linux ]

   ```
   aws ssm describe-association \
     --association-id b85ccafe-9f02-4812-9b81-01234EXAMPLE
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-association ^
     --association-id b85ccafe-9f02-4812-9b81-01234EXAMPLE
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMAssociation `
     -AssociationId b85ccafe-9f02-4812-9b81-01234EXAMPLE | Select-Object *
   ```

------

   The system returns information like the following\.

------
#### [ Linux ]

   ```
   {
       "AssociationDescription": {
           "ScheduleExpression": "cron(0 */1 * * ? *)",
           "OutputLocation": {
               "S3Location": {
                   "OutputS3KeyPrefix": "logs",
                   "OutputS3BucketName": "DOC-EXAMPLE-BUCKET",
                   "OutputS3Region": "us-east-1"
               }
           },
           "Name": "AWS-RunPowerShellScript",
           "Parameters": {
               "commands": [
                   "echo Association"
               ]
           },
           "LastExecutionDate": 1559316400.338,
           "Overview": {
               "Status": "Success",
               "DetailedStatus": "Success",
               "AssociationStatusAggregatedCount": {}
           },
           "AssociationId": "b85ccafe-9f02-4812-9b81-01234EXAMPLE",
           "DocumentVersion": "$DEFAULT",
           "LastSuccessfulExecutionDate": 1559316400.338,
           "LastUpdateAssociationDate": 1559316389.753,
           "Date": 1559314038.532,
           "AssociationVersion": "2",
           "AssociationName": "TestHostnameAssociation2",
           "Targets": [
               {
                   "Values": [
                       "Windows"
                   ],
                   "Key": "tag:Environment"
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
           "ScheduleExpression": "cron(0 */1 * * ? *)",
           "OutputLocation": {
               "S3Location": {
                   "OutputS3KeyPrefix": "logs",
                   "OutputS3BucketName": "DOC-EXAMPLE-BUCKET",
                   "OutputS3Region": "us-east-1"
               }
           },
           "Name": "AWS-RunPowerShellScript",
           "Parameters": {
               "commands": [
                   "echo Association"
               ]
           },
           "LastExecutionDate": 1559316400.338,
           "Overview": {
               "Status": "Success",
               "DetailedStatus": "Success",
               "AssociationStatusAggregatedCount": {}
           },
           "AssociationId": "b85ccafe-9f02-4812-9b81-01234EXAMPLE",
           "DocumentVersion": "$DEFAULT",
           "LastSuccessfulExecutionDate": 1559316400.338,
           "LastUpdateAssociationDate": 1559316389.753,
           "Date": 1559314038.532,
           "AssociationVersion": "2",
           "AssociationName": "TestHostnameAssociation2",
           "Targets": [
               {
                   "Values": [
                       "Windows"
                   ],
                   "Key": "tag:Environment"
               }
           ]
       }
   }
   ```

------
#### [ PowerShell ]

   ```
   AssociationId                 : b85ccafe-9f02-4812-9b81-01234EXAMPLE
   AssociationName               : TestHostnameAssociation2
   AssociationVersion            : 2
   AutomationTargetParameterName : 
   ComplianceSeverity            : 
   Date                          : 5/31/2019 2:47:18 PM
   DocumentVersion               : $DEFAULT
   InstanceId                    : 
   LastExecutionDate             : 5/31/2019 3:26:40 PM
   LastSuccessfulExecutionDate   : 5/31/2019 3:26:40 PM
   LastUpdateAssociationDate     : 5/31/2019 3:26:29 PM
   MaxConcurrency                : 
   MaxErrors                     : 
   Name                          : AWS-RunPowerShellScript
   OutputLocation                : Amazon.SimpleSystemsManagement.Model.InstanceAssociationOutputLocation
   Overview                      : Amazon.SimpleSystemsManagement.Model.AssociationOverview
   Parameters                    : {[commands, Amazon.Runtime.Internal.Util.AlwaysSendList`1[System.String]]}
   ScheduleExpression            : cron(0 */1 * * ? *)
   Status                        : 
   Targets                       : {tag:Environment}
   ```

------