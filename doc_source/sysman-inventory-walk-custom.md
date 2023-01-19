# Walkthrough: Assign custom inventory metadata to a managed node<a name="sysman-inventory-walk-custom"></a>

The following procedure walks you through the process of using the AWS Systems Manager [PutInventory](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutInventory.html) API operation to assign custom inventory metadata to a managed node\. This example assigns rack location information to a node\. For more information about custom inventory, see [Working with custom inventory](sysman-inventory-custom.md)\.

**To assign custom inventory metadata to a node**

1. Install and configure the AWS Command Line Interface \(AWS CLI\), if you haven't already\.

   For information, see [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)\.

1. Run the following command to assign rack location information to a node\.

   **Linux**

   ```
   aws ssm put-inventory --instance-id "ID" --items '[{"CaptureTime": "2016-08-22T10:01:01Z", "TypeName": "Custom:RackInfo", "Content":[{"RackLocation": "Bay B/Row C/Rack D/Shelf E"}], "SchemaVersion": "1.0"}]'
   ```

   **Windows**

   ```
   aws ssm put-inventory --instance-id "ID" --items "TypeName=Custom:RackInfo,SchemaVersion=1.0,CaptureTime=2021-05-22T10:01:01Z,Content=[{RackLocation='Bay B/Row C/Rack D/Shelf F'}]"
   ```

1. Run the following command to view custom inventory entries for this node\.

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

1. Run the following command to view the custom inventory schema\.

   ```
   aws ssm get-inventory-schema --type-name Custom:RackInfo
   ```

   The system responds with information like the following\.

   ```
   {
       "Schemas": [
           {
               "TypeName": "Custom:RackInfo",
               "Version": "1.0",
               "Attributes": [
                   {
                       "DataType": "STRING",
                       "Name": "RackLocation"
                   }
               ]
           }
       ]
   }
   ```