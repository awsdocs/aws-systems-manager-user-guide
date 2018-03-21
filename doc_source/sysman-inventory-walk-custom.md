# Walkthrough: Assign Custom Inventory Metadata to an Instance<a name="sysman-inventory-walk-custom"></a>

The following procedure walks you through the process of using the [PutInventory](http://docs.aws.amazon.com/ssm/latest/APIReference/API_PutInventory.html) API action to assign custom Inventory metadata to a managed instance\. This example assigns rack location information to an instance\. For more information about custom Inventory, see [Working with Custom Inventory](sysman-inventory-custom.md)\.

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