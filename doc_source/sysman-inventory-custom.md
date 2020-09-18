# Working with custom inventory<a name="sysman-inventory-custom"></a>

You can assign any metadata you want to your instances by creating *custom inventory*\. For example, let's say you manage a large number of servers in racks in your data center, and these servers have been configured as Systems Manager managed instances\. Currently, you store information about server rack location in a spreadsheet\. With custom inventory, you can specify the rack location of each instance as metadata on the instance\. When you collect inventory by using Systems Manager, the metadata is collected with other inventory metadata\. You can then port all inventory metadata to a central Amazon S3 bucket by using [Resource Data Sync](sysman-inventory-resource-data-sync.html) and query the data\.

**Note**  
Systems Manager supports a maximum of 20 custom inventory types per AWS account\.

To assign custom inventory to an instance, you can either use the Systems Manager [PutInventory](https://docs.aws.amazon.com/ssm/latest/APIReference/API_PutInventory.html) API action, as described in [Walkthrough: Assign custom inventory metadata to an instance](sysman-inventory-walk-custom.md)\. Or, you can create a custom inventory JSON file and upload it to the instance\. This section describes how to create the JSON file\.

The following example JSON file with custom inventory specifies rack information about an on\-premises server\. This examples specifies one type of custom inventory data \(`"TypeName": "Custom:RackInformation"`\), with multiple entries under `Content` that describe the data\.

```
{
    "SchemaVersion": "1.0",
    "TypeName": "Custom:RackInformation",
    "Content": {
        "Location": "US-EAST-02.CMH.RACK1",
        "InstalledTime": "2016-01-01T01:01:01Z",
        "vendor": "DELL",
        "Zone" : "BJS12",
        "TimeZone": "UTC-8"
      }
 }
```

You can also specify distinct entries in the `Content` section, as shown in the following example\.

```
{
    "SchemaVersion": "1.0",
"TypeName": "Custom:PuppetModuleInfo",
    "Content": [{
        "Name": "puppetlabs/aws",
        "Version": "1.0"
      },
      {
        "Name": "puppetlabs/dsc",
        "Version": "2.0"
      }
    ]
}
```

The JSON schema for custom inventory requires SchemaVersion, TypeName, and Content sections, but you can define the information in those sections\.

```
{
    "SchemaVersion": "user_defined",
    "TypeName": "Custom:user_defined",
    "Content": {
        "user_defined_attribute1": "user_defined_value1",
        "user_defined_attribute2": "user_defined_value2",
        "user_defined_attribute3": "user_defined_value3",
        "user_defined_attribute4": "user_defined_value4"
      }
 }
```

TypeName is limited to 100 characters\. Also, the TypeName section must start with Custom\. For example, Custom:PuppetModuleInfo\. Both Custom and the *Data* you specify must begin with a capital letter\. The following examples would cause an exception: "CUSTOM:RackInformation", "custom:rackinformation"\.

The Content section includes attributes and *data*\. These items are not case\-sensitive\. However, if you define an attribute \(for example: "Vendor": "DELL"\), then you must consistently reference this attribute in your custom inventory files\. If you specify "Vendor": "DELL" \(using a capital “V” in vendor\) in one file, and then you specify "vendor": "DELL" \(using a lowercase “v” in vendor\) in another file, the system returns an error\.

**Note**  
You must save the file with a \.json extension and the inventory you define must consist only of string values\.

After you create the file, you must save it on the instance\. The following table shows the location where custom inventory JSON files must be stored on the instance:


****  

| Operating system | Path | 
| --- | --- | 
|  Windows  |  %SystemDrive%\\ProgramData\\Amazon\\SSM\\InstanceData\\<instance\-id>\\inventory\\custom  | 
|  Linux  |  /var/lib/amazon/ssm/<instance\-id>/inventory/custom  | 

For an example of how to use custom inventory, see [Get Disk Utilization of Your Fleet Using EC2 Systems Manager Custom Inventory Types](http://aws.amazon.com/blogs/mt/get-disk-utilization-of-your-fleet-using-ec2-systems-manager-custom-inventory-types/)\.

## Deleting custom inventory<a name="sysman-inventory-delete"></a>

You can use the [DeleteInventory](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeleteInventory.html) API action to delete a custom inventory type and the data associated with that type\. You call the delete\-inventory command by using the AWS CLI to delete all data for an inventory type\. You call the delete\-inventory command with the `SchemaDeleteOption` to delete a custom inventory type\.

**Note**  
An inventory type is also called an inventory schema\.

The `SchemaDeleteOption` parameter includes the following options:
+ **DeleteSchema**: This option deletes the specified custom type and all data associated with it\. You can recreate the schema later, if you want\.
+ **DisableSchema**: If you choose this option, the system disables the current version, deletes all data for it, and ignores all new data if the version is less than or equal to the disabled version\. You can enable this inventory type again by calling the [PutInventory](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutInventory.html) action for a version greater than the disabled version\.

**To delete or disable custom inventory by using the AWS CLI**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to use the `dry-run` option to see which data will be deleted from the system\. This command doesn't delete any data\.

   ```
   aws ssm delete-inventory --type-name "Custom:custom_type_name" --dry-run
   ```

   The system returns information like the following\.

   ```
   {
      "DeletionSummary":{
         "RemainingCount":3,
         "SummaryItems":[
            {
               "Count":2,
               "RemainingCount":2,
               "Version":"1.0"
            },
            {
               "Count":1,
               "RemainingCount":1,
               "Version":"2.0"
            }
         ],
         "TotalCount":3
      },
      "TypeName":"Custom:custom_type_name"
   }
   ```

   For information about how to understand the delete inventory summary, see [Understanding the delete inventory summary](#sysman-inventory-delete-summary)\.

1. Run the following command to delete all data for a custom inventory type\.

   ```
   aws ssm delete-inventory --type-name "Custom:custom_type_name"
   ```
**Note**  
The output of this command doesn't show the deletion progress\. For this reason, TotalCount and Remaining Count are always the same because the system has not deleted anything yet\. You can use the describe\-inventory\-deletions command to show the deletion progress, as described later in this topic\.

   The system returns information like the following\.

   ```
   {
      "DeletionId":"system_generated_deletion_ID",
      "DeletionSummary":{
         "RemainingCount":3,
         "SummaryItems":[
            {
               "Count":2,
               "RemainingCount":2,
               "Version":"1.0"
            },
            {
               "Count":1,
               "RemainingCount":1,
               "Version":"2.0"
            }
         ],
         "TotalCount":3
      },
      "TypeName":"custom_type_name"
   }
   ```

   The system deletes all data for the specified custom inventory type from the Systems Manager Inventory service\. 

1. Run the following command\. The command performs the following actions for the current version of the inventory type: disables the current version, deletes all data for it, and ignores all new data if the version is less than or equal to the disabled version\. 

   ```
   aws ssm delete-inventory --type-name "Custom:custom_type_name" --schema-delete-option "DisableSchema"
   ```

   The system returns information like the following\.

   ```
   {
      "DeletionId":"system_generated_deletion_ID",
      "DeletionSummary":{
         "RemainingCount":3,
         "SummaryItems":[
            {
               "Count":2,
               "RemainingCount":2,
               "Version":"1.0"
            },
            {
               "Count":1,
               "RemainingCount":1,
               "Version":"2.0"
            }
         ],
         "TotalCount":3
      },
      "TypeName":"Custom:custom_type_name"
   }
   ```

   You can view a disabled inventory type by using the following command\.

   ```
   aws ssm get-inventory-schema --type-name Custom:custom_type_name
   ```

1. Run the following command to delete an inventory type\.

   ```
   aws ssm delete-inventory --type-name "Custom:custom_type_name" --schema-delete-option "DeleteSchema"
   ```

   The system deletes the schema and all inventory data for the specified custom type\.

   The system returns information like the following\.

   ```
   {
      "DeletionId":"system_generated_deletion_ID",
      "DeletionSummary":{
         "RemainingCount":3,
         "SummaryItems":[
            {
               "Count":2,
               "RemainingCount":2,
               "Version":"1.0"
            },
            {
               "Count":1,
               "RemainingCount":1,
               "Version":"2.0"
            }
         ],
         "TotalCount":3
      },
      "TypeName":"Custom:custom_type_name"
   }
   ```

### Viewing the deletion status<a name="sysman-inventory-delete-status"></a>

You can check the status of a delete operation by using the describe\-inventory\-deletions AWS CLI command\. You can specify a deletion ID to view the status of a specific delete operation\. Or, you can omit the deletion ID to view a list of all deletions run in the last 30 days\.

****

1. Run the following command to view the status of a deletion operation\. The system returned the deletion ID in the delete\-inventory summary\.

   ```
   aws ssm describe-inventory-deletions --deletion-id system_generated_deletion_ID
   ```

   The system returns the latest status\. The delete operation might not be finished yet\. The system returns information like the following\.

   ```
   {"InventoryDeletions": 
     [
       {"DeletionId": "system_generated_deletion_ID", 
        "DeletionStartTime": 1521744844, 
        "DeletionSummary": 
         {"RemainingCount": 1, 
          "SummaryItems": 
           [
             {"Count": 1, 
              "RemainingCount": 1, 
              "Version": "1.0"}
           ], 
          "TotalCount": 1}, 
        "LastStatus": "InProgress", 
        "LastStatusMessage": "The Delete is in progress", 
        "LastStatusUpdateTime": 1521744844, 
        "TypeName": "Custom:custom_type_name"}
     ]
   }
   ```

   If the delete operation is successful, the `LastStatusMessage` states: Deletion is successful\.

   ```
   {"InventoryDeletions": 
     [
       {"DeletionId": "system_generated_deletion_ID", 
        "DeletionStartTime": 1521744844, 
        "DeletionSummary": 
         {"RemainingCount": 0, 
          "SummaryItems": 
           [
             {"Count": 1, 
              "RemainingCount": 0, 
              "Version": "1.0"}
           ], 
          "TotalCount": 1}, 
        "LastStatus": "Complete", 
        "LastStatusMessage": "Deletion is successful", 
        "LastStatusUpdateTime": 1521745253, 
        "TypeName": "Custom:custom_type_name"}
     ]
   }
   ```

1. Run the following command to view a list of all deletions run in the last 30 days\.

   ```
   aws ssm describe-inventory-deletions --max-results a number
   ```

   ```
   {"InventoryDeletions": 
     [
       {"DeletionId": "system_generated_deletion_ID", 
        "DeletionStartTime": 1521682552, 
        "DeletionSummary": 
         {"RemainingCount": 0, 
          "SummaryItems": 
           [
             {"Count": 1, 
              "RemainingCount": 0, 
              "Version": "1.0"}
           ], 
          "TotalCount": 1}, 
        "LastStatus": "Complete", 
        "LastStatusMessage": "Deletion is successful", 
        "LastStatusUpdateTime": 1521682852, 
        "TypeName": "Custom:custom_type_name"}, 
       {"DeletionId": "system_generated_deletion_ID", 
        "DeletionStartTime": 1521744844, 
        "DeletionSummary": 
         {"RemainingCount": 0, 
          "SummaryItems": 
           [
             {"Count": 1, 
              "RemainingCount": 0, 
              "Version": "1.0"}
           ], 
          "TotalCount": 1}, 
        "LastStatus": "Complete", 
        "LastStatusMessage": "Deletion is successful", 
        "LastStatusUpdateTime": 1521745253, 
        "TypeName": "Custom:custom_type_name"}, 
       {"DeletionId": "system_generated_deletion_ID", 
        "DeletionStartTime": 1521680145, 
        "DeletionSummary": 
         {"RemainingCount": 0, 
          "SummaryItems": 
           [
             {"Count": 1, 
              "RemainingCount": 0, 
              "Version": "1.0"}
           ], 
          "TotalCount": 1}, 
        "LastStatus": "Complete", 
        "LastStatusMessage": "Deletion is successful", 
        "LastStatusUpdateTime": 1521680471, 
        "TypeName": "Custom:custom_type_name"}
     ], 
    "NextToken": "next-token"
   ```

### Understanding the delete inventory summary<a name="sysman-inventory-delete-summary"></a>

To help you understand the contents of the delete inventory summary, consider the following example\. A user assigned Custom:RackSpace inventory to three instances\. Inventory items 1 and 2 use custom type version 1\.0 \("SchemaVersion":"1\.0"\)\. Inventory item 3 uses custom type version 2\.0 \("SchemaVersion":"2\.0"\)\.

RackSpace custom inventory 1

```
{
   "CaptureTime":"2018-02-19T10:48:55Z",
   "TypeName":"CustomType:RackSpace",
   "InstanceId":"i-1234567890",
   "SchemaVersion":"1.0"   "Content":[
      {
         content of custom type omitted
      }
   ]
}
```

RackSpace custom inventory 2

```
{
   "CaptureTime":"2018-02-19T10:48:55Z",
   "TypeName":"CustomType:RackSpace",
   "InstanceId":"i-1234567891",
   "SchemaVersion":"1.0"   "Content":[
      {
         content of custom type omitted
      }
   ]
}
```

RackSpace custom inventory 3

```
{
   "CaptureTime":"2018-02-19T10:48:55Z",
   "TypeName":"CustomType:RackSpace",
   "InstanceId":"i-1234567892",
   "SchemaVersion":"2.0"   "Content":[
      {
         content of custom type omitted
      }
   ]
}
```

The user runs the following command to preview which data will be deleted\.

```
aws ssm delete-inventory --type-name "Custom:RackSpace" --dry-run
```

The system returns information like the following\.

```
{
   "DeletionId":"1111-2222-333-444-66666",
   "DeletionSummary":{
      "RemainingCount":3,           
      "TotalCount":3,             
                TotalCount and RemainingCount are the number of items that would be deleted if this was not a dry run. These numbers are the same because the system didn't delete anything.
      "SummaryItems":[
         {
            "Count":2,             The system found two items that use SchemaVersion 1.0. Neither item was deleted.           
            "RemainingCount":2,
            "Version":"1.0"
         },
         {
            "Count":1,             The system found one item that uses SchemaVersion 1.0. This item was not deleted.
            "RemainingCount":1,
            "Version":"2.0"
         }
      ],

   },
   "TypeName":"Custom:RackSpace"
}
```

The user runs the following command to delete the Custom:RackSpace inventory\. 

**Note**  
The output of this command doesn't show the deletion progress\. For this reason, TotalCount and Remaining Count are always the same because the system has not deleted anything yet\. You can use the describe\-inventory\-deletions command to show the deletion progress\.

```
aws ssm delete-inventory --type-name "Custom:RackSpace"
```

The system returns information like the following\.

```
{
   "DeletionId":"1111-2222-333-444-7777777",
   "DeletionSummary":{
      "RemainingCount":3,           There are three items to delete
      "SummaryItems":[
         {
            "Count":2,              The system found two items that use SchemaVersion 1.0.
            "RemainingCount":2,     
            "Version":"1.0"
         },
         {
            "Count":1,              The system found one item that uses SchemaVersion 2.0.
            "RemainingCount":1,     
            "Version":"2.0"
         }
      ],
      "TotalCount":3                
   },
   "TypeName":"RackSpace"
}
```

### Viewing inventory delete actions in EventBridge<a name="sysman-inventory-delete-cwe"></a>

You can configure Amazon EventBridge to create an event anytime a user deletes custom inventory\. EventBridge offers three types of events for custom inventory delete operations:
+ **Delete action for an instance**: If the custom inventory for a specific managed instance was successfully deleted or not\. 
+ **Delete action summary**: A summary of the delete action\.
+ **Warning for disabled custom inventory type**: A warning event if a user called the [PutInventory](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutInventory.html) API action for a custom inventory type version that was previously\-disabled\.

Here are examples of each event:

**Delete action for an instance**

```
{
   "version":"0",
   "id":"998c9cde-56c0-b38b-707f-0411b3ff9d11",
   "detail-type":"Inventory Resource State Change",
   "source":"aws.ssm",
   "account":"478678815555",
   "time":"2018-05-24T22:24:34Z",
   "region":"us-east-1",
   "resources":[
      "arn:aws:ssm:us-east-1:478678815555:managed-instance/i-0a5feb270fc3f0b97"
   ],
   "detail":{
      "action-status":"succeeded",
      "action":"delete",
      "resource-type":"managed-instance",
      "resource-id":"i-0a5feb270fc3f0b97",
      "action-reason":"",
      "type-name":"Custom:MyInfo"
   }
}
```

**Delete action summary**

```
{
   "version":"0",
   "id":"83898300-f576-5181-7a67-fb3e45e4fad4",
   "detail-type":"Inventory Resource State Change",
   "source":"aws.ssm",
   "account":"478678815555",
   "time":"2018-05-24T22:28:25Z",
   "region":"us-east-1",
   "resources":[

   ],
   "detail":{
      "action-status":"succeeded",
      "action":"delete-summary",
      "resource-type":"managed-instance",
      "resource-id":"",
      "action-reason":"The delete for type name Custom:MyInfo was completed. The deletion summary is: {\"totalCount\":2,\"remainingCount\":0,\"summaryItems\":[{\"version\":\"1.0\",\"count\":2,\"remainingCount\":0}]}",
      "type-name":"Custom:MyInfo"
   }
}
```

**Warning for disabled custom inventory type**

```
{
   "version":"0",
   "id":"49c1855c-9c57-b5d7-8518-b64aeeef5e4a",
   "detail-type":"Inventory Resource State Change",
   "source":"aws.ssm",
   "account":"478678815555",
   "time":"2018-05-24T22:46:58Z",
   "region":"us-east-1",
   "resources":[
      "arn:aws:ssm:us-east-1:478678815555:managed-instance/i-0ee2d86a2cfc371f6"
   ],
   "detail":{
      "action-status":"failed",
      "action":"put",
      "resource-type":"managed-instance",
      "resource-id":"i-0ee2d86a2cfc371f6",
      "action-reason":"The inventory item with type name Custom:MyInfo was sent with a disabled schema version 1.0. You must send a version greater than 1.0",
      "type-name":"Custom:MyInfo"
   }
}
```

Use the following procedure to create an EventBridge rule for custom inventory delete operations\. This procedure shows you how to create a rule that sends notifications for custom inventory delete operations to an Amazon SNS topic\. Before you begin, verify that you have an Amazon SNS topic, or create a new one\. For more information, see [Getting Started](https://docs.aws.amazon.com/sns/latest/dg/GettingStarted.html) in the *Amazon Simple Notification Service Developer Guide*\.

**To configure EventBridge for delete inventory operations**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**, and then choose **Create rule**\.

   \-or\-

   If the Amazon EventBridge home page opens first, choose **Create rule**\.

1. Enter a name and description for the rule\.

   A rule can't have the same name as another rule in the same Region and on the same event bus\.

1. For **Define pattern**, choose **Event pattern**\.

1. Choose **Pre\-defined pattern by service**\.

1. For **Service provider**, choose **AWS**\.

1. For **Service name**, choose **EC2 Simple Systems Manager \(SSM\)**

1. For **Event type**, choose **Inventory**\.

1. Verify that **Any detail type** is selected\.

1. For **Select event bus**, choose the event bus that you want to associate with this rule\. If you want this rule to trigger on matching events that come from your own AWS account, select ** AWS default event bus**\. When an AWS service in your account emits an event, it always goes to your account’s default event bus\. 

1. For **Target**, choose **SNS topic**, and then choose your topic from the **Topic** list\.

1. Expand **Configure input** and verify that **Matched event** is selected\.

1. \(Optional\) Enter one or more tags for the rule\. For more information, see [Tagging Your Amazon EventBridge Resources](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-tagging.html) in the *Amazon EventBridge User Guide*\.

1. Choose **Create**\.