# Editing and creating a new version of an association<a name="sysman-state-assoc-edit"></a>

You can edit a State Manager association to specify a new name, schedule, severity level, or targets\. You can also choose to write the output of the command to an Amazon Simple Storage Service \(Amazon S3\) bucket\. After you edit an association, State Manager creates a new version\. You can view different versions after editing, as described in the following procedures\. 

The following procedures describe how to edit and create a new version of an association using the Systems Manager console, AWS Command Line Interface \(AWS CLI\), and AWS Tools for PowerShell \(Tools for PowerShell\)\. 

**Important**  
State Manager doesn't support running associations that use a new version of a document if that document is shared from another account\. State Manager always runs the `default` version of a document if shared from another account, even though the Systems Manager console shows that a new version was processed\. If you want to run an association using a new version of a document shared form another account, you must set the document version to `default`\.

## Edit an association \(console\)<a name="sysman-state-assoc-edit-console"></a>

The following procedure describes how to use the Systems Manager console to edit and create a new version of an association\.

**Note**  
This procedure requires that you have write access to an existing Amazon S3 bucket\. If you haven't used Amazon S3 before, be aware that you will incur charges for using Amazon S3\. For information about how to create a bucket, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)\.

**To edit a State Manager association**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **State Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[The menu icon\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose ** State Manager**\.

1. Choose the association you created in [Create an association \(command line\)](sysman-state-assoc.md#create-state-manager-association-commandline) and then choose **Edit**\.

1. In the **Name** field, enter a new name\.

1. In the **Specify schedule** section, choose a new option\.

1. \(Optional\) For **Output options**, to save the command output to a file, select the **Enable writing output to S3** box\. Enter the bucket and prefix \(folder\) names in the boxes\.
**Note**  
The S3 permissions that grant the ability to write the data to an S3 bucket are those of the instance profile assigned to the managed node, not those of the user performing this task\. For more information, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md) or [Create an IAM service role for a hybrid environment](sysman-service-role.md)\. In addition, if the specified S3 bucket is in a different AWS account, verify that the instance profile or IAM service role associated with the managed node has the necessary permissions to write to that bucket\.

1. Choose **Edit association**\. Configure the association to meet your current requirements\.

1. In the **Associations** page, choose the name of the association you edited, and then choose the **Versions** tab\. The system lists each version of the association you created and edited\.

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the name of the Amazon S3 bucket you specified for storing command output, and then choose the folder named with the ID of the node that ran the association\. \(If you chose to store output in a folder in the bucket, open it first\.\)

1. Drill down several levels, through the `awsrunPowerShell` folder, to the `stdout` file\.

1. Choose **Open** or **Download** to view the host name\.

## Edit an association \(command line\)<a name="sysman-state-assoc-edit-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to edit and create a new version of an association\.

**To edit a State Manager association**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you haven't already\.

   For information, see [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) and [Installing the AWS Tools for PowerShell](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-set-up.html)\.

1. Use the following format to create a command to edit and create a new version of an existing State Manager association\. Replace each *example resource placeholder* with your own information\.
**Important**  
When you call `UpdateAssociation`, the system drops all optional parameters from the request and overwrites the association with null values for those parameters\. This is by design\. You must specify all optional parameters in the call, even if you are not changing the parameters\. This includes the `Name` parameter\. Before calling this API action, we recommend that you call the [DescribeAssociation](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeAssociation.html) API operation and make a note of all optional parameters required for your `UpdateAssociation` call\.

------
#### [ Linux & macOS ]

   ```
   aws ssm update-association \
       --name document_name \
       --document-version version_of_document_applied \
       --instance-id instances_to_apply_association_on \
       --parameters (if any) \
       --targets target_options \
       --schedule "cron_or_rate_expression" \
       --schedule-offset "number_between_1_and_6" \
       --output-location s3_bucket_to_store_output_details \
       --association-name association_name \
       --max-errors a_number_of_errors_or_a_percentage_of_target_set \
       --max-concurrency a_number_of_instances_or_a_percentage_of_target_set \
       --compliance-severity severity_level \
       --calendar-names change_calendar_names \
       --target-locations aws_region_or_account
   ```

------
#### [ Windows ]

   ```
   aws ssm update-association ^
       --name document_name ^
       --document-version version_of_document_applied ^
       --instance-id instances_to_apply_association_on ^
       --parameters (if any) ^
       --targets target_options ^
       --schedule "cron_or_rate_expression" ^
       --schedule-offset "number_between_1_and_6" ^
       --output-location s3_bucket_to_store_output_details ^
       --association-name association_name ^
       --max-errors a_number_of_errors_or_a_percentage_of_target_set ^
       --max-concurrency a_number_of_instances_or_a_percentage_of_target_set ^
       --compliance-severity severity_level ^
       --calendar-names change_calendar_names ^
       --target-locations aws_region_or_account
   ```

------
#### [ PowerShell ]

   ```
   Update-SSMAssociation `
       -Name document_name `
       -DocumentVersion version_of_document_applied `
       -InstanceId instances_to_apply_association_on `
       -Parameters (if any) `
       -Target target_options `
       -ScheduleExpression "cron_or_rate_expression" `
       -ScheduleOffset "number_between_1_and_6" `
       -OutputLocation s3_bucket_to_store_output_details `
       -AssociationName association_name `
       -MaxError  a_number_of_errors_or_a_percentage_of_target_set
       -MaxConcurrency a_number_of_instances_or_a_percentage_of_target_set `
       -ComplianceSeverity severity_level `
       -CalendarNames change_calendar_names `
       -TargetLocations aws_region_or_account
   ```

------

   The following example updates an existing association to change the name to `TestHostnameAssociation2`\. The new association version runs every hour and writes the output of commands to the specified Amazon S3 bucket\.

------
#### [ Linux & macOS ]

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

   The following example updates an existing association to change the name to `CalendarAssociation`\. The new association runs when the calendar is open and writes command output to the specified Amazon S3 bucket\. 

------
#### [ Linux & macOS ]

   ```
   aws ssm update-association \
     --association-id 8dfe3659-4309-493a-8755-01234EXAMPLE \
     --association-name CalendarAssociation \
     --parameters commands="echo Association" \
     --output-location S3Location='{OutputS3Region=us-east-1,OutputS3BucketName=DOC-EXAMPLE-BUCKET,OutputS3KeyPrefix=logs}' \
     --calendar-names "arn:aws:ssm:us-east-1:123456789012:document/testCalendar2"
   ```

------
#### [ Windows ]

   ```
   aws ssm update-association ^
     --association-id 8dfe3659-4309-493a-8755-01234EXAMPLE ^
     --association-name CalendarAssociation ^
     --parameters commands="echo Association" ^
     --output-location S3Location='{OutputS3Region=us-east-1,OutputS3BucketName=DOC-EXAMPLE-BUCKET,OutputS3KeyPrefix=logs}' ^
     --calendar-names "arn:aws:ssm:us-east-1:123456789012:document/testCalendar2"
   ```

------
#### [ PowerShell ]

   ```
   Update-SSMAssociation `
     -AssociationId b85ccafe-9f02-4812-9b81-01234EXAMPLE `
     -AssociationName CalendarAssociation `
     -AssociationName OneTimeAssociation `
     -Parameter @{"commands"="echo Association"} `
     -S3Location_OutputS3BucketName DOC-EXAMPLE-BUCKET `
     -CalendarNames "arn:aws:ssm:us-east-1:123456789012:document/testCalendar2"
   ```

------

   The following example updates an existing association to change the name to `MultiCalendarAssociation`\. The new association runs when the calendars are open and writes command output to the specified Amazon S3 bucket\. 

------
#### [ Linux & macOS ]

   ```
   aws ssm update-association \
     --association-id 8dfe3659-4309-493a-8755-01234EXAMPLE \
     --association-name MultiCalendarAssociation \
     --parameters commands="echo Association" \
     --output-location S3Location='{OutputS3Region=us-east-1,OutputS3BucketName=DOC-EXAMPLE-BUCKET,OutputS3KeyPrefix=logs}' \
     --calendar-names "arn:aws:ssm:us-east-1:123456789012:document/testCalendar1" "arn:aws:ssm:us-east-2:123456789012:document/testCalendar2"
   ```

------
#### [ Windows ]

   ```
   aws ssm update-association ^
     --association-id 8dfe3659-4309-493a-8755-01234EXAMPLE ^
     --association-name MultiCalendarAssociation ^
     --parameters commands="echo Association" ^
     --output-location S3Location='{OutputS3Region=us-east-1,OutputS3BucketName=DOC-EXAMPLE-BUCKET,OutputS3KeyPrefix=logs}' ^
     --calendar-names "arn:aws:ssm:us-east-1:123456789012:document/testCalendar1" "arn:aws:ssm:us-east-2:123456789012:document/testCalendar2"
   ```

------
#### [ PowerShell ]

   ```
   Update-SSMAssociation `
     -AssociationId b85ccafe-9f02-4812-9b81-01234EXAMPLE `
     -AssociationName MultiCalendarAssociation `
     -Parameter @{"commands"="echo Association"} `
     -S3Location_OutputS3BucketName DOC-EXAMPLE-BUCKET `
     -CalendarNames "arn:aws:ssm:us-east-1:123456789012:document/testCalendar1" "arn:aws:ssm:us-east-2:123456789012:document/testCalendar2"
   ```

------

1. To view the new version of the association, run the following command\.

------
#### [ Linux & macOS ]

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
#### [ Linux & macOS ]

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