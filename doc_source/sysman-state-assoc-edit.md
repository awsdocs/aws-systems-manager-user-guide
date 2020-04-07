# Edit and Create a New Version of an Association<a name="sysman-state-assoc-edit"></a>

You can edit an association to specify a new name, schedule, severity level, or targets\. You can also choose to write the output of the command to an S3 bucket\. After you edit an association, Systems Manager creates a new version\. You can view different versions after editing, as described in the following procedures\. 

The following procedures describe how to edit and create a new version of an association using the AWS Systems Manager console, AWS Command Line Interface \(AWS CLI\), and AWS Tools for PowerShell\. 

## Edit and Create a New Version of an Association \(Console\)<a name="sysman-state-assoc-edit-console"></a>

The following procedure describes how to use the Systems Manager console to edit and create a new version of an association\.

**Note**  
This procedure requires that you have write access to an existing S3 bucket\. If you have not used Amazon S3 before, be aware that you are charged for storage, data transfer, and more\. For more information, see [Amazon S3 Pricing](http://aws.amazon.com/s3/pricing/)\. For information about how to create a bucket, see [Create a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)\.

**To edit a State Manager association**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **State Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose ** State Manager**\.

1. Choose the association you created in the previous procedure, and then choose **Edit**\.

1. In the **Name** field, enter a new name \(for example, **TestHostnameAssociation2**\)\.

1. In **Specify schedule**, choose a new option\. For example, choose **CRON schedule builder**, and then choose **Every 1 hour**\.

1. \(Optional\) To write the command output to an S3 bucket, in **Output options**, do the following:
   + Choose **Enable writing output to S3**\.
   + In the **S3 bucket name** field, enter the name of an S3 bucket you have write access to\.
   + \(Optional\) To write output to a folder in the bucket, enter its name in **S3 key prefix**\. If no folder exists with the name you specify, State Manager creates it for you\.

1. Choose **Edit association**\.

1. In the **Associations** page, choose the name of the association you just edited, and then choose the **Versions** tab\. The system lists each version of the association you created and edited\.

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose the name of the S3 bucket you specified for storing command output, and then choose the folder named with the ID of the instance that ran the association\. \(If you chose to store output in a folder in the bucket, open it first\.\)

1. Drill down several levels, through the `awsrunPowerShell` folder, to the `stdout` file\.

1. Choose **Open** or **Download** to view the host name\.

## Edit and Create a New Version of an Association \(Command Line\)<a name="sysman-state-assoc-edit-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to edit and create a new version of an association\.

**To edit a State Manager association**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or Upgrade AWS Command Line Tools](getting-started-cli.md)\.

1. Use the following format to create a command to edit and create a new version of an existing State Manager association\.

------
#### [ Linux ]

   ```
   aws ssm update-association \
     --association-id b85ccafe-9f02-4812-9b81-01234EXAMPLE \
     --association-name association_name \
     --parameters (if any) \
     --output-location S3Location='{OutputS3Region=region,OutputS3BucketName=bucketname,OutputS3KeyPrefix=keyprefix}' \
     --scheduleexpression "cron_or_rate_expression"
   ```

**Important**  
To retain existing parameter values of your association, such as association name or compliance severity, you must specify these values when you update the association\. If you don't specify these parameter values when you update an association, the new association version uses no values\. For example, if your existing association has a cron schedule, but you don't specify `--schedule-expression` when updating, the new association version does not have a schedule expression\.

------
#### [ Windows ]

   ```
   aws ssm update-association ^
     --association-id b85ccafe-9f02-4812-9b81-01234EXAMPLE ^
     --association-name association_name ^
     --parameters (if any) ^
     --output-location S3Location='{OutputS3Region=region,OutputS3BucketName=bucketname,OutputS3KeyPrefix=keyprefix}' ^
     --scheduleexpression "cron_or_rate_expression"
   ```

**Important**  
To retain existing parameter values of your association, such as association name or compliance severity, you must specify these values when you update the association\. If you don't specify these parameters when you update an association, the new association version uses the default values \(none\)\. For example, if your existing association has a cron schedule, but you don't specify `--schedule-expression` when updating, the new association version does not have a schedule expression\.

------
#### [ PowerShell ]

   ```
   Update-SSMAssociation `
     -AssociationId b85ccafe-9f02-4812-9b81-01234EXAMPLE `
     -AssociationName document_name `
     -Parameter (if any) `
     -S3Location_OutputS3BucketName bucket_name `
     -S3Location_OutputS3KeyPrefix key_prefix `
     -S3Location_OutputS3Region region `
     -ScheduleExpression "cron_or_rate_expression"
   ```

**Important**  
To retain existing parameter values of your association, such as association name or compliance severity, you must specify these values when you update the association\. If you don't specify these parameters when you update an association, the new association version uses no values\. For example, if your existing association has a cron schedule, but you don't specify `-ScheduleExpression` when updating, the new association version does not have a schedule expression\.

------

   The following example updates an existing association to change the name to `TestHostnameAssociation2`\. The new association version runs every hour and writes the output of commands to the specified S3 bucket\.

------
#### [ Linux ]

   ```
   aws ssm update-association \
     --association-id 8dfe3659-4309-493a-8755-01234EXAMPLE \
     --association-name TestHostnameAssociation2 \
     --parameters commands="echo Association" \
     --output-location S3Location='{OutputS3Region=us-east-1,OutputS3BucketName=statemanager,OutputS3KeyPrefix=logs}' \
     --schedule-expression "cron(0 */1 * * ? *)"
   ```

------
#### [ Windows ]

   ```
   aws ssm update-association ^
     --association-id 8dfe3659-4309-493a-8755-01234EXAMPLE ^
     --association-name TestHostnameAssociation2 ^
     --parameters commands="echo Association" ^
     --output-location S3Location='{OutputS3Region=us-east-1,OutputS3BucketName=statemanager,OutputS3KeyPrefix=logs}' ^
     --schedule-expression "cron(0 */1 * * ? *)"
   ```

------
#### [ PowerShell ]

   ```
   Update-SSMAssociation `
     -AssociationId b85ccafe-9f02-4812-9b81-01234EXAMPLE `
     -AssociationName TestHostnameAssociation2 `
     -Parameter @{"commands"="echo Association"} `
     -S3Location_OutputS3BucketName statemanager `
     -S3Location_OutputS3KeyPrefix logs `
     -S3Location_OutputS3Region us-east-1 `
     -ScheduleExpression "cron(0 */1 * * ? *)"
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
                   "OutputS3BucketName": "statemanager",
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
                   "OutputS3BucketName": "statemanager",
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