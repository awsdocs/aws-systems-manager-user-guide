# Creating associations<a name="sysman-state-assoc"></a>

State Manager, a capability of AWS Systems Manager, helps you keep your AWS resources in a state that you define and reduce configuration drift\. To do this, State Manager uses associations\. An *association* is a configuration that you assign to your AWS resources\. The configuration defines the state that you want to maintain on your resources\. For example, an association can specify that antivirus software must be installed and running on a managed node, or that certain ports must be closed\.

An association specifies a schedule for when to apply the configuration and the targets for the association\. For example, an association for antivirus software might run once a day on all managed nodes in an AWS account\. If the software isn't installed on a node, then the association could instruct State Manager to install it\. If the software is installed, but the service isn't running, then the association could instruct State Manager to start the service\.

**Note**  
You can assign tags to an association when you create it by using a command line tool such as the AWS CLI or AWS Tools for PowerShell\. Adding tags to an association by using the Systems Manager console isn't supported\. For more information about tags, see [Tagging Systems Manager resources](tagging-resources.md)\. 

The following procedures describe how to create an association that uses either a `Command` or a `Policy` document to target managed nodes\. For information about creating an association that uses an Automation runbook to target nodes or other types of AWS resources, see [Scheduling automations with State Manager associations](scheduling-automations-state-manager-associations.md)\.

**Before you begin**  
When you create an association, you can either specify a schedule for the association or run the association immediately\. If you specify a schedule, you must enter the schedule in the form of either a cron or rate expression\. For more information about cron and rate expressions, see [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

An association also specifies which managed nodes, or targets, should receive the association\. State Manager includes several features to help you target your managed nodes and control how the association is deployed to those targets\. For more information about targets and rate controls, see [About targets and rate controls in State Manager associations](systems-manager-state-manager-targets-and-rate-controls.md)\.

## About creating associations<a name="state-manager-about-creating-associations"></a>

When you create an association, by default, the system immediately runs it on the specified resources\. After the initial run, the association runs in intervals according to the schedule that you defined and according to the following rules:
+ State Manager attempts to run the association on all specified or targeted nodes during an interval\.
+ If an association doesn't run during an interval \(because, for example, a concurrency value limited the number of nodes that could process the association at one time\), then State Manager attempts to run the association during the next interval\.
+ State Manager records history for all skipped intervals\. You can view the history on the **Execution History** tab\.

For associations that run according to a schedule, you can specify standard cron or rate expressions that define when the association runs\. State Manager also supports cron expressions that include a day of the week and the number sign \(\#\) to designate the *n*th day of a month to run an association\. Here is an example that runs a cron schedule on the third Tuesday of every month at 23:30 UTC:

`cron(30 23 ? * TUE#3 *)`

Here is an example that runs on the second Thursday of every month at midnight UTC:

`cron(0 0 ? * THU#2 *)`

State Manager also supports the \(L\) sign to indicate the last *X* day of the month\. Here is an example that runs a cron schedule on the last Tuesday of every month at midnight UTC:

`cron(0 0 ? * 3L *)`

To further control when an association runs, for example if you want to run an association two days after patch Tuesday, you can specify an offset\. An *offset* defines how many days to wait after the scheduled day to run an association\. For example, if you specified a cron schedule of `cron(0 0 ? * THU#2 *)`, you could specify the number 3 in the **Schedule offset** field to run the association each Sunday after the second Thursday of the month\.

**Note**  
To use offsets, you must either choose the **Apply association only at the next specified Cron interval** option in the console or you must specify the `ApplyOnlyAtCronInterval` parameter from the command line\. This option tells State Manager not to run an association immediately after you create it\.

## Create an association \(console\)<a name="sysman-state-assoc-console"></a>

The following procedure describes how to use the Systems Manager console to create a State Manager association\.

**Warning**  
When you create an association, you can choose an AWS resource group of managed nodes as the target for the association\. If an AWS Identity and Access Management \(IAM\) user, group, or role has permission to create an association that targets a resource group of managed nodes, then that user, group, or role automatically has root\-level control of all nodes in the group\. Permit only trusted administrators to create associations\. 

**To create a State Manager association**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **State Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose ** State Manager**\.

1. Choose **Create association**\.

1. In the **Name** field, specify a name\.

1. In the **Document** list, choose the option next to a document name\. Note the document type\. This procedure applies to `Command` and `Policy` documents\. For information about creating an association that uses an Automation runbook, see [Scheduling automations with State Manager associations](scheduling-automations-state-manager-associations.md)\.
**Important**  
State Manager doesn't support running associations that use a new version of a document if that document is shared from another account\. State Manager always runs the `default` version of a document if shared from another account, even though the Systems Manager console shows that a new version was processed\. If you want to run an association using a new version of a document shared form another account, you must set the document version to `default`\.

1. For **Parameters**, specify the required input parameters\.

1. \(Optional\) Choose a CloudWatch alarm to apply to your association for monitoring\. To attach a CloudWatch alarm to your command, the IAM principal that creates the association must have permission for the `iam:createServiceLinkedRole` action\. For more information about CloudWatch alarms, see [Using Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)\. Note that if your alarm activates, any pending command invocations or automations do not run\.

1. For **Targets**, choose an option\. For information about using targets, see [About targets and rate controls in State Manager associations](systems-manager-state-manager-targets-and-rate-controls.md)\.

1. In the **Specify schedule** section, choose either **On Schedule** or **No schedule**\. If you choose **On Schedule**, use the buttons provided to create a cron or rate schedule for the association\. 

   If you don't want the association to run immediately after you create it, choose **Apply association only at the next specified Cron interval**\.

1. \(Optional\) In the **Schedule offset** field, specify a number between 1 and 6\. 

1. In the **Advanced options** section use **Compliance severity** to choose a severity level for the association and use **Change Calendars** to choose a change calendar for the association\.

   Compliance reporting indicates whether the association state is compliant or noncompliant, along with the severity level you indicate here\. For more information, see [About State Manager association compliance](sysman-compliance-about.md#sysman-compliance-about-association)\.

   The change calendar determines when the association runs\. If the calendar is closed, the association isn't applied\. If the calendar is open, the association runs accordingly\. For more information, see [AWS Systems Manager Change Calendar](systems-manager-change-calendar.md)\.

1. In the **Rate control** section, choose options to control how the association runs on multiple nodes\. For more information about using rate controls, see [About targets and rate controls in State Manager associations](systems-manager-state-manager-targets-and-rate-controls.md)\.

   In the **Concurrency** section, choose an option: 
   + Choose **targets** to enter an absolute number of targets that can run the association simultaneously\.
   + Choose **percentage** to enter a percentage of the target set that can run the association simultaneously\.

   In the **Error threshold** section, choose an option:
   + Choose **errors** to enter an absolute number of errors that are allowed before State Manager stops running associations on additional targets\.
   + Choose **percentage** to enter a percentage of errors that are allowed before State Manager stops running associations on additional targets\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Enable writing output to S3** box\. Enter the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the managed node, not those of the IAM user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md) or [Create an IAM service role for a hybrid environment](sysman-service-role.md)\. In addition, if the specified S3 bucket is in a different AWS account, verify that the instance profile or IAM service role associated with the managed node has the necessary permissions to write to that bucket\.

   Following are the minimal permissions required to turn on Amazon S3 output for an association\. You might further restrict access to individual IAM users or roles within an account\. At minimum, an Amazon EC2 instance profile should have an IAM role with the `AmazonSSMManagedInstanceCore` managed policy and the following inline policy\. 

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "s3:PutObject",
                   "s3:GetObject",
                   "s3:PutObjectAcl"
               ],
               "Resource": "arn:aws:s3:::DOC-EXAMPLE-BUCKET/*"
           }
       ]
   }
   ```

   For minimal permissions, the Amazon S3 bucket you export to must have the default settings defined by the Amazon S3 console\. For more information about creating Amazon S3 buckets, see [Creating a bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html) in the *Amazon S3 User Guide*\. 
**Note**  
API operations that are initiated by the SSM document during an association run are not logged in AWS CloudTrail\.

1. Choose **Create Association**\.

**Note**  
If you delete the association you created, the association no longer runs on any targets of that association\.

## Create an association \(command line\)<a name="create-state-manager-association-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or Tools for PowerShell to create a State Manager association\. This section includes several examples that show how to use targets and rate controls\. Targets and rate controls allow you to assign an association to dozens or hundreds of nodes while controlling the execution of those associations\. For more information about targets and rate controls, see [About targets and rate controls in State Manager associations](systems-manager-state-manager-targets-and-rate-controls.md)\.

**Before you begin**  
The `targets` parameter is an array of search criteria that targets nodes using a `Key`,`Value` combination that you specify\. If you plan to create an association on dozens or hundreds of node by using the `targets` parameter, review the following targeting options before you begin the procedure\.

Target specific nodes by specifying IDs

```
--targets Key=InstanceIds,Values=instance-id-1,instance-id-2,instance-id-3
```

```
--targets Key=InstanceIds,Values=i-02573cafcfEXAMPLE,i-0471e04240EXAMPLE,i-07782c72faEXAMPLE
```

Target instances by using tags

```
--targets Key=tag:tag-key,Values=tag-value-1,tag-value-2,tag-value-3
```

```
--targets Key=tag:Environment,Values=Development,Test,Pre-production
```

**Note**  
When using tags, you can only use one tag key\. If you want to target your nodes using more than one tag key, use the resource group option\.

Target nodes by using AWS Resource Groups

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
Note the following information\.  
State Manager doesn't support running associations that use a new version of a document if that document is shared from another account\. State Manager always runs the `default` version of a document if shared from another account, even though the Systems Manager console shows that a new version was processed\. If you want to run an association using a new version of a document shared form another account, you must set the document version to `default`\.
When you create an association, you specify when the schedule runs\. Specify the schedule by using a cron or rate expression\. For more information about cron and rate expressions, see [Cron and rate expressions for associations](reference-cron-and-rate-expressions.md#reference-cron-and-rate-expressions-association)\.

**To create an association**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you haven't already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Use the following format to create a command that creates a State Manager association\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws ssm create-association \
       --name document_name \
       --document-version version_of_document_applied \
       --instance-id instances_to_apply_association_on \
       --parameters (if any) \
       --targets target_options \
       --schedule "cron_or_rate_expression" \
       --apply-only-at-cron-interval required_parameter_for_schedule_offsets \
       --schedule-offset number_between_1_and_6 \
       --output-location s3_bucket_to_store_output_details \
       --association-name association_name \
       --max-errors a_number_of_errors_or_a_percentage_of_target_set \
       --max-concurrency a_number_of_instances_or_a_percentage_of_target_set \
       --compliance-severity severity_level \
       --calendar-names change_calendar_names \
       --target-locations aws_region_or_account \
       --tags "Key=tag_key,Value=tag_value"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
       --name document_name ^
       --document-version version_of_document_applied ^
       --instance-id instances_to_apply_association_on ^
       --parameters (if any) ^
       --targets target_options ^
       --schedule "cron_or_rate_expression" ^
       --apply-only-at-cron-interval required_parameter_for_schedule_offsets ^
       --schedule-offset number_between_1_and_6 ^
       --output-location s3_bucket_to_store_output_details ^
       --association-name association_name ^
       --max-errors a_number_of_errors_or_a_percentage_of_target_set ^
       --max-concurrency a_number_of_instances_or_a_percentage_of_target_set ^
       --compliance-severity severity_level ^
       --calendar-names change_calendar_names ^
       --target-locations aws_region_or_account ^
       --tags "Key=tag_key,Value=tag_value"
   ```

------
#### [ PowerShell ]

   ```
   New-SSMAssociation `
       -Name document_name `
       -DocumentVersion version_of_document_applied `
       -InstanceId instances_to_apply_association_on `
       -Parameters (if any) `
       -Target target_options `
       -ScheduleExpression "cron_or_rate_expression" `
       -ApplyOnlyAtCronInterval required_parameter_for_schedule_offsets `
       -ScheduleOffSet number_between_1_and_6 `
       -OutputLocation s3_bucket_to_store_output_details `
       -AssociationName association_name `
       -MaxError  a_number_of_errors_or_a_percentage_of_target_set
       -MaxConcurrency a_number_of_instances_or_a_percentage_of_target_set `
       -ComplianceSeverity severity_level `
       -CalendarNames change_calendar_names `
       -TargetLocations aws_region_or_account `
       -Tags "Key=tag_key,Value=tag_value"
   ```

------

   The following example creates an association on nodes tagged with `"Environment,Linux"`\. The association uses the `AWS-UpdateSSMAgent` document to update the SSM Agent on the targeted nodes at 2:00 UTC every Sunday morning\. This association runs simultaneously on 10 nodes maximum at any given time\. Also, this association stops running on more nodes for a particular execution interval if the error count exceeds 5\. For compliance reporting, this association is assigned a severity level of Medium\.

------
#### [ Linux & macOS ]

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
     -ComplianceSeverity MEDIUM `
     -ScheduleExpression "cron(0 2 ? * SUN *)" `
     -MaxConcurrency 10 `
     -MaxError 5
   ```

------

   The following example targets node IDs by specifying a wildcard value \(\*\)\. This allows Systems Manager to create an association on *all* nodes in the current AWS account and AWS Region\. This association runs simultaneously on 10 nodes maximum at any given time\. Also, this association stops running on more nodes for a particular execution interval if the error count exceeds 5\. For compliance reporting, this association is assigned a severity level of Medium\. This association uses a schedule offset, which means it runs two days after the specified cron schedule\. It also includes the `ApplyOnlyAtCronInterval` parameter, which is required to use the schedule offset and which means the association won't run immediately after it is created\.

------
#### [ Linux & macOS ]

   ```
   aws ssm create-association \
     --association-name Update_SSM_Agent_Linux \
     --name "AWS-UpdateSSMAgent" \
     --targets "Key=instanceids,Values=*" \
     --compliance-severity "MEDIUM" \
     --schedule "cron(0 2 ? * SUN#2 *)" \
     --apply-only-at-cron-interval \
     --schedule-offset 2 \
     --max-errors "5" \
     --max-concurrency "10" \
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
     --association-name Update_SSM_Agent_Linux ^
     --name "AWS-UpdateSSMAgent" ^
     --targets "Key=instanceids,Values=*" ^
     --compliance-severity "MEDIUM" ^
     --schedule "cron(0 2 ? * SUN#2 *)" ^
     --apply-only-at-cron-interval ^
     --schedule-offset 2 ^
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
     -ScheduleExpression "cron(0 2 ? * SUN#2 *)" `
     -ApplyOnlyAtCronInterval `
     -ScheduleOffset 2 `
     -MaxConcurrency 10 `
     -MaxError 5 `
     -ComplianceSeverity MEDIUM `
     -ApplyOnlyAtCronInterval
   ```

------

   The following example creates an association on nodes in Resource Groups\. The group is named "HR\-Department"\. The association uses the `AWS-UpdateSSMAgent` document to update SSM Agent on the targeted nodes at 2:00 UTC every Sunday morning\. This association runs simultaneously on 10 nodes maximum at any given time\. Also, this association stops running on more nodes for a particular execution interval if the error count exceeds 5\. For compliance reporting, this association is assigned a severity level of Medium\. This association runs at the specified cron schedule\. It doesn't run immediately after the association is created\.

------
#### [ Linux & macOS ]

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

   The following example creates an association that runs on nodes tagged with a specific node ID\. The association uses the SSM Agent document to update SSM Agent on the targeted nodes once when the change calendar is open\. The association checks the calendar state when it runs\. If the calendar is closed at launch time and the association is only run once, it won't run again because the association run window has passed\. If the calendar is open, the association runs accordingly\.
**Note**  
If you add new nodes to the tags or resource groups that an association acts on when the change calendar is closed, the association is applied to those nodes once the change calendar opens\.

------
#### [ Linux & macOS ]

   ```
   aws ssm create-association \
     --association-name CalendarAssociation \
     --targets "Key=instanceids,Values=i-0cb2b964d3e14fd9f" \
     --name AWS-UpdateSSMAgent  \
     --calendar-names "arn:aws:ssm:us-east-1:123456789012:document/testCalendar1" \
     --schedule "rate(1day)"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
     --association-name CalendarAssociation ^
     --targets "Key=instanceids,Values=i-0cb2b964d3e14fd9f" ^
     --name AWS-UpdateSSMAgent  ^
     --calendar-names "arn:aws:ssm:us-east-1:123456789012:document/testCalendar1" ^
     --schedule "rate(1day)"
   ```

------
#### [ PowerShell ]

   ```
   New-SSMAssociation `
     -AssociationName CalendarAssociation `
     -Target @{
         "Key"="tag:instanceids"
         "Values"="i-0cb2b964d3e14fd9f"
       } `
     -Name AWS-UpdateSSMAgent `
     -CalendarNames "arn:aws:ssm:us-east-1:123456789012:document/testCalendar1" `
     -ScheduleExpression "rate(1day)"
   ```

------

   The following example creates an association that runs on nodes tagged with a specific node ID\. The association uses the SSM Agent document to update SSM Agent on the targeted nodes on the targeted nodes at 2:00 AM every Sunday\. This association runs only at the specified cron schedule when the change calendar is open\. When the association is created, it checks the calendar state\. If the calendar is closed, the association isn't applied\. When the interval to apply the association starts at 2:00 AM on Sunday, the association checks to see if the calendar is open\. If the calendar is open, the association runs accordingly\.
**Note**  
If you add new nodes to the tags or resource groups that an association acts on when the change calendar is closed, the association is applied to those nodes once the change calendar opens\.

------
#### [ Linux & macOS ]

   ```
   aws ssm create-association \
     --association-name MultiCalendarAssociation \
     --targets "Key=instanceids,Values=i-0cb2b964d3e14fd9f" \
     --name AWS-UpdateSSMAgent  \
     --calendar-names "arn:aws:ssm:us-east-1:123456789012:document/testCalendar1" "arn:aws:ssm:us-east-2:123456789012:document/testCalendar2" \
     --schedule "cron(0 2 ? * SUN *)"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
     --association-name MultiCalendarAssociation ^
     --targets "Key=instanceids,Values=i-0cb2b964d3e14fd9f" ^
     --name AWS-UpdateSSMAgent  ^
     --calendar-names "arn:aws:ssm:us-east-1:123456789012:document/testCalendar1" "arn:aws:ssm:us-east-2:123456789012:document/testCalendar2" ^
     --schedule "cron(0 2 ? * SUN *)"
   ```

------
#### [ PowerShell ]

   ```
   New-SSMAssociation `
     -AssociationName MultiCalendarAssociation `
     -Name AWS-UpdateSSMAgent `
     -Target @{
         "Key"="tag:instanceids"
         "Values"="i-0cb2b964d3e14fd9f"
       } `
     -CalendarNames "arn:aws:ssm:us-east-1:123456789012:document/testCalendar1" "arn:aws:ssm:us-east-2:123456789012:document/testCalendar2" `
     -ScheduleExpression "cron(0 2 ? * SUN *)"
   ```

------

**Note**  
If you delete the association you created, the association no longer runs on any targets of that association\. Also, if you specified the `apply-only-at-cron-interval` parameter, you can reset this option\. To do so, specify the `no-apply-only-at-cron-interval` parameter when you update the association from the command line\. This parameter forces the association to run immediately after updating the assocation and according to the interval specified\.