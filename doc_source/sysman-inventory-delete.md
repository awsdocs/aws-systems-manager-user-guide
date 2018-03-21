# Deleting Custom Inventory<a name="sysman-inventory-delete"></a>

You can use the [DeleteInventory](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeleteInventory.html) API action to delete custom inventory data\. When you call this API action by using the AWS CLI, you specify a custom inventory type such as Custom:RackSpace\. The system deletes all data for the inventory type from Amazon S3\.

You can use the `SchemaDeleteOption` to manage custom inventory by disabling or deleting a custom inventory type\.

**Note**  
An inventory type is also called an inventory schema\.

`SchemaDeleteOption` includes the following options:

+ **DisableSchema**: If you choose this option, the system ignores all data for the current version and any earlier versions of this inventory type\. You can enable this inventory type again by calling the [PutInventory](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutInventory.html) action for a version greater than the disabled version\.

+ **DeleteSchema**: This option deletes the specified custom type from the Inventory service\. You can recreate the schema later, if you want\.

**To delete custom inventory by using the AWS CLI**

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

1. Execute the following command to use the `dry-run` option to see which data will be deleted from the system\. This command does not delete any data\.

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

   For information about how to understand the delete inventory summary, see [Understanding the Delete Inventory Summary](#sysman-inventory-delete-summary)\.

1. Execute the following command to delete all data for a custom inventory type\.

   ```
   aws ssm delete-inventory --type-name "Custom:custom_type_name"
   ```

   The system returns information like the following\.

   ```
   {
      "DeletionId":"9a238622-5949-41b5-ad5f-a2ee78d262af",
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

   All data for the specified custom inventory type is deleted from Amazon S3\. 

1. Execute the following command to disable and ignore all data for the inventory type\. 

   ```
   aws ssm delete-inventory --type-name "Custom:custom_type_name" --schema-delete-option "DisableSchema"
   ```

   The system returns information like the following\.

   ```
   {
      "DeletionId":"9a238622-5949-41b5-ad5f-a2ee78d262af",
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

1. Execute the following command to delete an inventory type\.

   ```
   aws ssm delete-inventory --type-name "Custom:custom_type_name" --schema-delete-option "DeleteSchema"
   ```

   The system deletes the schema and all inventory for the specified custom type\.

   The system returns information like the following\.

   ```
   {
      "DeletionId":"9a238622-5949-41b5-ad5f-a2ee78d262af",
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

## Understanding the Delete Inventory Summary<a name="sysman-inventory-delete-summary"></a>

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

The user executes the following command to preview which data will be deleted\.

```
aws ssm delete-inventory --type-name "Custom:RackSpace" --dry-run
```

The system returns information like the following\.

```
{
   "DeletionId":"1111-2222-333-444-66666",
   "DeletionSummary":{
      "RemainingCount":3,           
      "TotalCount":3,             RemainingCount is the number of items that would be deleted if this was not a dry run. TotalCount is the number of items that were                                   deleted if this was not a dry run. These numbers are the same because the system didn't delete anything.
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

The user executes the following command to delete the Custom:RackSpace inventory\.

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
            "RemainingCount":0,     Both items were deleted. There are no items left of this version.
            "Version":"1.0"
         },
         {
            "Count":1,              The system found one item that uses SchemaVersion 2.0.
            "RemainingCount":0,     The item was deleted. There are no items left of this version.
            "Version":"2.0"
         }
      ],
      "TotalCount":3                Three items were deleted.
   },
   "TypeName":"RackSpace"
}
```