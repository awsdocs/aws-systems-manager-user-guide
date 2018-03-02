# Systems Manager Inventory Manager Walkthroughs<a name="sysman-inventory-walk"></a>

Use the following walkthroughs to collect and manage Inventory data\. We recommend that you initially perform these walkthroughs with managed instances in a test environment\. 

**Before You Begin**  
Before you start these walkthroughs, complete the following tasks\.

+ Update SSM Agent on the instances you want to inventory\. By running the latest version of SSM Agent, you ensure that you can collect metadata for all supported inventory types\. For information about how to update SSM Agent by using State Manager, see [Walkthrough: Automatically Update the SSM Agent \(CLI\)](sysman-state-cli.md)\.

+ Verify that your instances meet Systems Manager prerequisites\. For more information, see [Systems Manager Prerequisites](systems-manager-setting-up.md#systems-manager-prereqs)\.

+ \(Optional\) Create a JSON file to collect custom inventory\. For more information, see [Working with Custom Inventory](sysman-inventory-about.md#sysman-inventory-custom)\.


+ [Assigning Custom Inventory Metadata to an Instance](#sysman-inventory-walk-custom)
+ [Collecting Inventory by Using the AWS CLI](#sysman-inventory-cliwalk)
+ [Using Resource Data Sync to Aggregate Inventory Data](#sysman-inventory-resource-data-sync)

## Assigning Custom Inventory Metadata to an Instance<a name="sysman-inventory-walk-custom"></a>

The following procedure walks you through the process of using the [PutInventory](http://docs.aws.amazon.com/ssm/latest/APIReference/API_PutInventory.html) API action to assign custom Inventory metadata to a managed instance\. This example assigns rack location information to an instance\. For more information about custom Inventory, see [Working with Custom Inventory](sysman-inventory-about.md#sysman-inventory-custom)\.

**To assign custom Inventory metadata to an instance**

1. [Download](https://aws.amazon.com/cli/) the latest version of the AWS CLI to your local machine\.

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in Amazon EC2, or you must have been granted the appropriate permission in AWS Identity and Access Management \(IAM\)\.

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

1. Execute the following command to assign rack location information to an instance\.

   ```
   aws ssm put-inventory --instance-id "ID" --items '[{"CaptureTime": "2016-08-22T10:01:01Z", "TypeName": "Custom:RackInfo", "Content":[{"RackLocation": "Bay B/Row C/Rack D/Shelf E"}], "SchemaVersion": "1.0"}]'
   ```

1. Execute the following command to view custom inventory entries for this instance\.

   ```
   aws ssm list-inventory-entries --instance-id ID --type-name "Custom:RackInfo"
   ```

   The system responds with information like the following\.

   ```
   {
       "InstanceId": "ID", 
       "TypeName": "Custom:RackInfo", 
       "Entries": [
           {
               "RackLocation": "Bay B/Row C/Rack D/Shelf E"
           }
       ], 
       "SchemaVersion": "1.0", 
       "CaptureTime": "2016-08-22T10:01:01Z"
   }
   ```

1. Execute the following command to view the custom metadata\.

   ```
   aws ssm get-inventory
   ```

   The system responds with information like the following\.

   ```
   {
       "Entities": [
           {
               "Data": {
                   "AWS:InstanceInformation": {
                       "Content": [
                           {
                               "ComputerName": "WIN-9JHCEPEGORG.WORKGROUP", 
                               "InstanceId": "ID", 
                               "ResourceType": "EC2Instance", 
                               "AgentVersion": "3.19.1153", 
                               "PlatformVersion": "6.3.9600", 
                               "PlatformName": "Windows Server 2012 R2 Standard", 
                               "PlatformType": "Windows"
                           }
                       ], 
                       "TypeName": "AWS:InstanceInformation", 
                       "SchemaVersion": "1.0"
                   }
               }, 
               "Id": "ID"
           }
       ]
   }
   ```

## Collecting Inventory by Using the AWS CLI<a name="sysman-inventory-cliwalk"></a>

The following procedure walks you through the process of using Inventory to collect metadata from an Amazon EC2 instance\. When you configure Inventory collection, you start by creating a Systems Manager State Manager association\. Systems Manager collects the Inventory data when the association is run\. If you don't create the association first, and attempt to invoke the aws:softwareInventory plugin by using, for example, Run Command, the system returns the following error:

`The aws:softwareInventory plugin can only be invoked via ssm-associate`\.

**Note**  
An instance can have only have one Inventory association configured at a time\. If you configure an instance with two or more Inventory associations, the association doesn't run and no inventory data is collected\.

**To gather inventory from an instance**

1. [Download](https://aws.amazon.com/cli/) the latest version of the AWS CLI to your local machine\.

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in Amazon EC2, or you must have been granted the appropriate permission in AWS Identity and Access Management \(IAM\)\.

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

1. Execute the following command to create a State Manager association that runs Inventory on the instance\. This command configures the service to run every six hours and to collect network configuration, Windows Update, and application metadata from an instance\.

   ```
   aws ssm create-association --name "AWS-GatherSoftwareInventory" --targets "Key=instanceids,Values=an instance ID" --schedule-expression "cron(0 0/30 * 1/1 * ? *)" --output-location "{ \"S3Location\": { \"OutputS3Region\": \"us-east-1\", \"OutputS3BucketName\": \"Test bucket\", \"OutputS3KeyPrefix\": \"Test\" } }" --parameters "networkConfig=Enabled,windowsUpdates=Enabled,applications=Enabled"
   ```

   The system responds with information like the following\.

   ```
   {
       "AssociationDescription": {
           "ScheduleExpression": "cron(0 0/30 * 1/1 * ? *)",
           "OutputLocation": {
               "S3Location": {
                   "OutputS3KeyPrefix": "Test",
                   "OutputS3BucketName": "Test bucket",
                   "OutputS3Region": "us-east-1"
               }
           },
           "Name": "The name you specified",
           "Parameters": {
               "applications": [
                   "Enabled"
               ],
               "networkConfig": [
                   "Enabled"
               ],
               "windowsUpdates": [
                   "Enabled"
               ]
           },
           "Overview": {
               "Status": "Pending",
               "DetailedStatus": "Creating"
           },
           "AssociationId": "1a2b3c4d5e6f7g-1a2b3c-1a2b3c-1a2b3c-1a2b3c4d5e6f7g",
           "DocumentVersion": "$DEFAULT",
           "LastUpdateAssociationDate": 1480544990.06,
           "Date": 1480544990.06,
           "Targets": [
               {
                   "Values": [
                      "i-1a2b3c4d5e6f7g"
                   ],
                   "Key": "InstanceIds"
               }
           ]
       }
   }
   ```

   You can target large groups of instances by using the `Targets` parameter with EC2 tags\.

   ```
   aws ssm create-association --name "AWS-GatherSoftwareInventory" --targets "Key=tag:Environment,Values=Production" --schedule-expression "cron(0 0/30 * 1/1 * ? *)" --output-location "{ \"S3Location\": { \"OutputS3Region\": \"us-east-1\", \"OutputS3BucketName\": \"Test bucket\", \"OutputS3KeyPrefix\": \"Test\" } }" --parameters "networkConfig=Enabled,windowsUpdates=Enabled,applications=Enabled"
   ```

   You can also inventory files and Windows Registry keys on a Windows instance by using the `files` and `windowsRegistry` inventory types with expressions\. For more information about these inventory types, see [Working with File and Windows Registry Inventory](sysman-inventory-about.md#sysman-inventory-file-and-registry)\.

   ```
   aws ssm create-association --name "AWS-GatherSoftwareInventory" --targets "Key=instanceids,Values=i-0704358e3a3da9eb1" --schedule-expression "cron(0 0/30 * 1/1 * ? *)"  --parameters '{"files":["[{\"Path\": \"C:\\Program Files\", \"Pattern\": [\"*.exe\"], \"Recursive\": true}]"], "windowsRegistry": ["[{\"Path\":\"HKEY_LOCAL_MACHINE\\Software\\Amazon\", \"Recursive\":true}]"]}' --profile dev-pdx
   ```

1. Execute the following command to view the association status\.

   ```
   aws ssm describe-instance-associations-status --instance-id an instance ID
   ```

   The system responds with information like the following\.

   ```
   {
   "InstanceAssociationStatusInfos": [
            {
               "Status": "Pending",
               "DetailedStatus": "Associated",
               "Name": "reInvent2016PolicyDocumentTest",
               "InstanceId": "i-1a2b3c4d5e6f7g",
               "AssociationId": "1a2b3c4d5e6f7g-1a2b3c-1a2b3c-1a2b3c-1a2b3c4d5e6f7g",
               "DocumentVersion": "1"
           }
   ]
   }
   ```

## Using Resource Data Sync to Aggregate Inventory Data<a name="sysman-inventory-resource-data-sync"></a>

The following walkthrough describes how to create a Resource Data Sync configuration by using the AWS CLI\. A Resource Data Sync automatically ports Inventory data from all of your managed instances to a central Amazon S3 bucket\. The sync automatically updates the data in the central Amazon S3 bucket whenever new Inventory data is discovered\. This walkthrough also describes how to use Amazon Athena and Amazon QuickSight to query and analyze the aggregated data\. For information about creating a Resource Data Sync by using the Amazon EC2 console, see [Configuring Resource Data Sync for Inventory](sysman-inventory-datasync.md)\.

**Note**  
This walkthrough includes information about how to encrypt the sync by using AWS Key Management Service \(AWS KMS\)\. Inventory does not collect any user\-specific, proprietary, or sensitive data so encryption is optional\. For more information about AWS KMS, see [AWS Key Management Service Developer Guide](http://docs.aws.amazon.com/kms/latest/developerguide/)\.

**Before You Begin**  
Before you start this walkthrough, you must collect Inventory metadata from your managed instances\. For the purpose of the Amazon Athena and Amazon QuickSight sections in this walkthrough, we recommend that you collect Application metadata\. For more information about how to collect Inventory data, see [Collecting Inventory by Using the AWS CLI](#sysman-inventory-cliwalk)\.

\(Optional\) If you want to encrypt the sync by using AWS KMS, then you must either create a new key that includes the following policy, or you must update an existing key and add this policy to it\.

```
{
   "Version":"2012-10-17",
   "Id":"ssm-access-policy",
   "Statement":[
      {
         "Sid":"ssm-access-policy-statement",
         "Action":[
            "kms:GenerateDataKey"
         ],
         "Effect":"Allow",
         "Principal":{
            "Service":"ssm.amazonaws.com"
         },
         "Resource":"arn:aws:kms:region:AWS-account-ID:key/KMS-key-id"
      }
   ]
}
```

**To create a Resource Data Sync for Inventory**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Create a bucket to store your aggregated Inventory data\. For more information, see [Create a Bucket](http://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html) in the Amazon Simple Storage Service Getting Started Guide\. Make a note of the bucket name and the AWS Region where you created it\.

1. After you create the bucket, choose the **Permissions** tab, and then choose **Bucket Policy**\.

1. Copy and paste the following bucket policy into the policy editor\. Replace *Bucket\-Name* and *Account\-ID* with the name of the Amazon S3 bucket you created and a valid AWS account ID\. Optionally, replace *Bucket\-Prefix* with the name of an Amazon S3 prefix \(subdirectory\)\. If you did not created a prefix, remove *Bucket\-Prefix*/ from the ARN in the policy\. 

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "SSMBucketPermissionsCheck",
               "Effect": "Allow",
               "Principal": {
                   "Service": "ssm.amazonaws.com"
               },
               "Action": "s3:GetBucketAcl",
               "Resource": "arn:aws:s3:::Bucket-Name"
           },
           {
               "Sid": " SSMBucketDelivery",
               "Effect": "Allow",
               "Principal": {
                   "Service": "ssm.amazonaws.com"
               },
               "Action": "s3:PutObject",
               "Resource": ["arn:aws:s3:::Bucket-Name/Bucket-Prefix/*/accountid=Account_ID_number/*"],
               "Condition": {
                   "StringEquals": {
                       "s3:x-amz-acl": "bucket-owner-full-control"
                   }
               }
           }
       ]
   }
   ```

1. \(Optional\) If you want to encrypt the sync, then you must add the following policy to the bucket\. Repeat the previous step to add the following policy to the bucket\.

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
            "Effect":"Allow",
            "Principal":{
               "Service":"ssm.amazonaws.com"
            },
            "Action":"s3:PutObject",
            "Resource":"arn:aws:s3:::bucket-name/prefix/*",
            "Condition":{
               "StringEquals":{
                  "s3:x-amz-server-side-encryption":"aws:kms",
                  "s3:x-amz-server-side-encryption-aws-kms-key-id":"arn:aws:kms:region:AWS-account-ID:key/KMS-key-ID"
               }
            }
         }
      ]
   }
   ```

1. [Download](https://aws.amazon.com/cli/) the latest version of the AWS CLI to your local machine\.

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in Amazon EC2, or you must have been granted the appropriate permission in AWS Identity and Access Management \(IAM\)\.

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

1. \(Optional\) If you want to encrypt the sync, execute the following command to verify that the bucket policy is enforcing the KMS key requirement\.

   ```
   aws s3 cp ./A file in the bucket s3://bucket-name/prefix/ --sse aws:kms --sse-kms-key-id "arn:aws:kms:region:AWS-account-ID:key/KMS-key-id" --region region
   ```

1. Execute the following command to create a Resource Data Sync configuration with the Amazon S3 bucket you created at the start of this procedure\. This command creates a sync from the AWS Region you are currently logged into\.
**Note**  
If the sync and the target Amazon S3 bucket are located in different regions, you may be subject to data transfer pricing\. For more information, see [Amazon S3 Pricing](https://aws.amazon.com//s3/pricing/)\.

   ```
   aws ssm create-resource-data-sync --sync-name a name --s3-destination "BucketName=the name of the S3 bucket,Prefix=the name of the prefix, if specified,SyncFormat=JsonSerDe,Region=the region where the S3 bucket was created" 
   ```

   You can use the `region` parameter to specify where the sync configuration should be created\. In the following example, Inventory data from the us\-west\-1 Region, will be synchronized in the Amazon S3 bucket in the us\-west\-2 Region\.

   ```
   aws ssm create-resource-data-sync --sync-name InventoryDataWest --s3-destination "BucketName=InventoryData,Prefix=HybridEnv,SyncFormat=JsonSerDe,Region=us-west-2" --region us-west-1
   ```

   \(Optional\) If you want to encrypt the sync by using AWS KMS, execute the following command to create the sync\. If you encrypt the sync, then the AWS KMS key and the Amazon S3 bucket must be in the same Region\.

   ```
   aws ssm create-resource-data-sync --sync-name sync-name --s3-destination "BucketName=bucket-name,Prefix=prefix,SyncFormat=JsonSerDe,AWSKMSKeyARN=arn:aws:kms:region:AWS-account-ID:key/KMS-key-id,Region=bucket-region" --region region
   ```

1. Execute the following command to view the status of sync configuration\. 

   ```
   aws ssm list-resource-data-sync 
   ```

   If you created the sync configuration in a different Region, then you must specify the `region` parameter, as shown in the following example\.

   ```
   aws ssm list-resource-data-sync --region us-west-1
   ```

1. After the sync configuration is created successfully, browse the target bucket in Amazon S3\. Inventory data should appear within a few minutes\.

**Working with the Data in Amazon Athena**

The following section describes how to view and query the data in Amazon Athena\. Before you begin, we recommend that you learn about Athena\. For more information, see [What is Amazon Athena?](http://docs.aws.amazon.com/athena/latest/ug/what-is.html) and [Working with Data](http://docs.aws.amazon.com/athena/latest/ug/work-with-data.html) in the *Amazon Athena User Guide*\.

**To view and query the data in Amazon Athena**

1. Open the Athena console at [https://console\.aws\.amazon\.com/athena/](https://console.aws.amazon.com/athena/home)\.

1. Copy and paste the following statement into the query editor and then choose **Run Query**\.

   ```
   CREATE DATABASE ssminventory
   ```

   The system creates a database called ssminventory\.

1. Copy and paste the following statement into the query editor and then choose **Run Query**\. Replace *Bucket\-Name* and *Bucket\-Prefix* with the name and prefix of the Amazon S3 target\.

   ```
   CREATE EXTERNAL TABLE IF NOT EXISTS ssminventory.AWS_Application (
   Name string,
   ApplicationType string,
   Publisher string,
   Version string,
   InstalledTime string,
   Architecture string,
   URL string,
   Summary string,
   PackageId string
   ) 
   PARTITIONED BY (AccountId string, Region string, ResourceType string)
   ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
   WITH SERDEPROPERTIES (
     'serialization.format' = '1'
   ) LOCATION 's3://Bucket-Name/Bucket-Prefix/AWS:Application/'
   ```

1. Copy and paste the following statement into the query editor and then choose **Run Query**\.

   ```
   MSCK REPAIR TABLE ssminventory.AWS_Application
   ```

   The system partitions the table\.
**Note**  
If you create Resource Data Syncs from additional AWS Regions or accounts, then you must run this command again to update the partitions\. You may also need to update your Amazon S3 bucket policy\.

1. To preview your data, choose the view icon next to the AWS\_Application table\.  
![\[alt-text\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/sysman-inventory-resource-data-sync-walk.png)

1. Copy and paste the following statement into the query editor and then choose **Run Query**\.

   ```
   SELECT a.name, a.version, count( a.version) frequency 
   from aws_application a where
   a.name = 'aws-cfn-bootstrap'
   group by a.name, a.version
   order  by frequency desc
   ```

   The query returns a count of different versions of aws\-cfn\-bootstrap, which is an AWS application present on Amazon EC2 Linux and Windows instances\.

1. Individually copy and paste the following statements into the query editor, replace *Bucket\-Name* and *Bucket\-Prefix* with information for Amazon S3, and then choose **Run Query**\. These statements set up additional Inventory tables in Athena\.

   ```
   CREATE EXTERNAL TABLE IF NOT EXISTS ssminventory.AWS_AWSComponent (
    `ResourceId` string,
     `Name` string,
     `ApplicationType` string,
     `Publisher` string,
     `Version` string,
     `InstalledTime` string,
     `Architecture` string,
     `URL` string
   )
   PARTITIONED BY (AccountId string, Region string, ResourceType string)
   ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
   WITH SERDEPROPERTIES (
     'serialization.format' = '1'
   ) LOCATION 's3://Bucket-Name/Bucket-Prefix/AWS:AWSComponent/'
   
   MSCK REPAIR TABLE ssminventory.AWS_AWSComponent
   ```

   ```
   CREATE EXTERNAL TABLE IF NOT EXISTS ssminventory.AWS_WindowsUpdate (
     `ResourceId` string,
     `HotFixId` string,
     `Description` string,
     `InstalledTime` string,
     `InstalledBy` string
   )
   PARTITIONED BY (AccountId string, Region string, ResourceType string)
   ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
   WITH SERDEPROPERTIES (
     'serialization.format' = '1'
   ) LOCATION 's3://Bucket-Name/Bucket-Prefix/AWS:WindowsUpdate/'
   
   MSCK REPAIR TABLE ssminventory.AWS_WindowsUpdate
   ```

   ```
   CREATE EXTERNAL TABLE IF NOT EXISTS ssminventory.AWS_InstanceInformation (
     `AgentType` string,
     `AgentVersion` string,
     `ComputerName` string,
     `IamRole` string,
     `InstanceId` string,
     `IpAddress` string,
     `PlatformName` string,
     `PlatformType` string,
     `PlatformVersion` string
   )
   PARTITIONED BY (AccountId string, Region string, ResourceType string)
   ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
   WITH SERDEPROPERTIES (
     'serialization.format' = '1'
   ) LOCATION 's3://Bucket-Name/Bucket-Prefix/AWS:InstanceInformation/'
   
   MSCK REPAIR TABLE ssminventory.AWS_InstanceInformation
   ```

   ```
   CREATE EXTERNAL TABLE IF NOT EXISTS ssminventory.AWS_Network (
     `ResourceId` string,
     `Name` string,
     `SubnetMask` string,
     `Gateway` string,
     `DHCPServer` string,
     `DNSServer` string,
     `MacAddress` string,
     `IPV4` string,
     `IPV6` string
   )
   PARTITIONED BY (AccountId string, Region string, ResourceType string)
   ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
   WITH SERDEPROPERTIES (
     'serialization.format' = '1'
   ) LOCATION 's3://Bucket-Name/Bucket-Prefix/AWS:Network/'
   
   
   MSCK REPAIR TABLE ssminventory.AWS_Network
   ```

   ```
   CREATE EXTERNAL TABLE IF NOT EXISTS ssminventory.AWS_PatchCompliance (
     `ResourceId` string,
     `Title` string,
     `KBId` string,
     `Classification` string,
     `Severity` string,
     `State` string,
     `InstalledTime` string
   )
   PARTITIONED BY (AccountId string, Region string, ResourceType string)
   ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
   WITH SERDEPROPERTIES (
     'serialization.format' = '1'
   ) LOCATION 's3://Bucket-Name/Bucket-Prefix/AWS:PatchCompliance/'
   
   MSCK REPAIR TABLE ssminventory.AWS_PatchCompliance
   ```

   ```
   CREATE EXTERNAL TABLE IF NOT EXISTS ssminventory.AWS_PatchSummary (
     `ResourceId` string,
     `PatchGroup` string,
     `BaselineId` string,
     `SnapshotId` string,
     `OwnerInformation` string,
     `InstalledCount` int,
     `InstalledOtherCount` int,
     `NotApplicableCount` int,
     `MissingCount` int,
     `FailedCount` int,
     `OperationType` string,
     `OperationStartTime` string,
     `OperationEndTime` string
   )
   PARTITIONED BY (AccountId string, Region string, ResourceType string)
   ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
   WITH SERDEPROPERTIES (
     'serialization.format' = '1'
   ) LOCATION 's3://Bucket-Name/Bucket-Prefix/AWS:PatchSummary/'
   
   MSCK REPAIR TABLE ssminventory.AWS_PatchSummary
   ```

**Working with the Data in Amazon QuickSight**

The following section provides an overview with links for building a visualization in Amazon QuickSight\.

**To build a visualization in Amazon QuickSight**

1. Sign up for [Amazon QuickSight](https://quicksight.aws/) and then log in to the QuickSight console\.

1. Create a data set from the AWS\_Application table and any other tables you created\. For more information, see [Creating a Data Set Using Amazon Athena Data](http://docs.aws.amazon.com/quicksight/latest/user/create-a-data-set-athena.html)\.

1. Join tables\. For example, you could join the `instanceid` column from `AWS_InstanceInformation` because it matches the `resourceid` column in other inventory tables\. For more information about joining tables, see [Joining Tables](http://docs.aws.amazon.com/quicksight/latest/user/joining-tables.html)\.

1. Build a visualization\. For more information, see [Working with Amazon QuickSight Visuals](http://docs.aws.amazon.com/quicksight/latest/user/working-with-visuals.html)\.