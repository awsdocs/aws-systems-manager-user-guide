# Walkthrough: Use the AWS CLI to Collect Inventory<a name="sysman-inventory-cliwalk"></a>

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
   aws ssm create-association --name "AWS-GatherSoftwareInventory" --targets "Key=instanceids,Values=an instance ID" --schedule-expression "cron(0 0/30 * 1/1 * ? *)" --output-location "{ \"S3Location\": { \"OutputS3Region\": \"region-id\", \"OutputS3BucketName\": \"Test bucket\", \"OutputS3KeyPrefix\": \"Test\" } }" --parameters "networkConfig=Enabled,windowsUpdates=Enabled,applications=Enabled"
   ```

   *region\-id* represents the AWS Region where the instance is located, such as us\-east\-2 for the US East \(Ohio\) Region\.

   The system responds with information like the following\.

   ```
   {
       "AssociationDescription": {
           "ScheduleExpression": "cron(0 0/30 * 1/1 * ? *)",
           "OutputLocation": {
               "S3Location": {
                   "OutputS3KeyPrefix": "Test",
                   "OutputS3BucketName": "Test bucket",
                   "OutputS3Region": "us-east-2"
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

   You can target large groups of instances by using the `Targets` parameter with EC2 tags\. For example:

   ```
   aws ssm create-association --name "AWS-GatherSoftwareInventory" --targets "Key=tag:Environment,Values=Production" --schedule-expression "cron(0 0/30 * 1/1 * ? *)" --output-location "{ \"S3Location\": { \"OutputS3Region\": \"us-east-2\", \"OutputS3BucketName\": \"Test bucket\", \"OutputS3KeyPrefix\": \"Test\" } }" --parameters "networkConfig=Enabled,windowsUpdates=Enabled,applications=Enabled"
   ```

   You can also inventory files and Windows Registry keys on a Windows instance by using the `files` and `windowsRegistry` inventory types with expressions\. For more information about these inventory types, see [Working with File and Windows Registry Inventory](sysman-inventory-file-and-registry.md)\.

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