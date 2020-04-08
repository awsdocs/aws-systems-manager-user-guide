# Walkthrough: Assign custom inventory metadata to an instance<a name="sysman-inventory-walk-custom"></a>

The following procedure walks you through the process of using the [PutInventory](https://docs.aws.amazon.com/ssm/latest/APIReference/API_PutInventory.html) API action to assign custom inventory metadata to a managed instance\. This example assigns rack location information to an instance\. For more information about custom inventory, see [Working with custom inventory](sysman-inventory-custom.md)\.

**To assign custom inventory metadata to an instance**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to assign rack location information to an instance\.

   ```
   aws ssm put-inventory --instance-id "ID" --items '[{"CaptureTime": "2016-08-22T10:01:01Z", "TypeName": "Custom:RackInfo", "Content":[{"RackLocation": "Bay B/Row C/Rack D/Shelf E"}], "SchemaVersion": "1.0"}]'
   ```

1. Run the following command to view custom inventory entries for this instance\.

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