# Walkthrough: Configure Your Managed Instances for Inventory by Using the CLI<a name="sysman-inventory-cliwalk"></a>

The following procedures walk you through the process of configuring Systems Manager Inventory to collect metadata from your managed instances\. When you configure inventory collection, you start by creating a Systems Manager State Manager association\. Systems Manager collects the inventory data when the association is run\. If you don't create the association first, and attempt to invoke the aws:softwareInventory plugin by using, for example, Run Command, the system returns the following error:

`The aws:softwareInventory plugin can only be invoked via ssm-associate`\.

**Note**  
An instance can have only one inventory association configured at a time\. If you configure an instance with two or more inventory associations, the association doesn't run and no inventory data is collected\.

## Quickly Configure All of Your Managed Instances for Inventory \(CLI\)<a name="sysman-inventory-cliwalk-all"></a>

You can quickly configure all managed instances in your AWS account and in the current Region to collect inventory data\. This is called creating a global inventory association\. To create a global inventory association by using the AWS CLI, use the wildcard option for the `instanceIds` value, as shown in the following procedure\.

**To configure inventory for all managed instances in your AWS account and in the current Region \(CLI\)**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or Upgrade AWS Command Line Tools](getting-started-cli.md)\.

1. Run the following command\.

   ```
   aws ssm create-association --name AWS-GatherSoftwareInventory --targets Key=InstanceIds,Values=* --schedule-expression "rate(1 day)" --parameters applications=Enabled,awsComponents=Enabled,customInventory=Enabled,instanceDetailedInformation=Enabled,networkConfig=Enabled,services=Enabled,windowsRoles=Enabled,windowsUpdates=Enabled
   ```

**Note**  
This command does not enable inventory of file or Windows Registry metadata\. To inventory these datatypes, use the next procedure\.

## Manually Configuring Inventory on Your Managed Instances \(CLI\)<a name="sysman-inventory-cliwalk-manual"></a>

Use the following procedure to manually configure Systems Manager Inventory on your managed instances by using instance IDs or tags\.

**To manually configure your managed instances for inventory \(CLI\)**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or Upgrade AWS Command Line Tools](getting-started-cli.md)\.

1. Run the following command to create a State Manager association that runs Systems Manager Inventory on the instance\. This command configures the service to run every six hours and to collect network configuration, Windows Update, and application metadata from an instance\.

   ```
   aws ssm create-association --name "AWS-GatherSoftwareInventory" --targets "Key=instanceids,Values=an instance ID" --schedule-expression "rate(240 minutes)" --output-location "{ \"S3Location\": { \"OutputS3Region\": \"region-id\", \"OutputS3BucketName\": \"Test bucket\", \"OutputS3KeyPrefix\": \"Test\" } }" --parameters "networkConfig=Enabled,windowsUpdates=Enabled,applications=Enabled"
   ```

   *region\-id* represents the AWS Region where the instance is located, such as us\-east\-2 for the US East \(Ohio\) Region\.

   The system responds with information like the following\.

   ```
   {
       "AssociationDescription": {
           "ScheduleExpression": "rate(240 minutes)",
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
   aws ssm create-association --name "AWS-GatherSoftwareInventory" --targets "Key=tag:Environment,Values=Production" --schedule-expression "rate(240 minutes)" --output-location "{ \"S3Location\": { \"OutputS3Region\": \"us-east-2\", \"OutputS3BucketName\": \"Test bucket\", \"OutputS3KeyPrefix\": \"Test\" } }" --parameters "networkConfig=Enabled,windowsUpdates=Enabled,applications=Enabled"
   ```

   You can also inventory files and Windows Registry keys on a Windows instance by using the `files` and `windowsRegistry` inventory types with expressions\. For more information about these inventory types, see [Working with File and Windows Registry Inventory](sysman-inventory-file-and-registry.md)\.

   ```
   aws ssm create-association --name "AWS-GatherSoftwareInventory" --targets "Key=instanceids,Values=i-0704358e3a3da9eb1" --schedule-expression "rate(240 minutes)"  --parameters '{"files":["[{\"Path\": \"C:\\Program Files\", \"Pattern\": [\"*.exe\"], \"Recursive\": true}]"], "windowsRegistry": ["[{\"Path\":\"HKEY_LOCAL_MACHINE\\Software\\Amazon\", \"Recursive\":true}]"]}' --profile dev-pdx
   ```

1. Run the following command to view the association status\.

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