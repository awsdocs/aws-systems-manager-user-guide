# Aggregating inventory data<a name="sysman-inventory-aggregate"></a>

After you configure your managed instances for AWS Systems Manager Inventory, you can view aggregated counts of inventory data\. For example, say you configured dozens or hundreds of managed instances to collect the AWS:Application inventory type\. By using the information in this section, you can see an exact count of how many instances are configured to collect this data\.

You can also see specific inventory details by aggregating on a data type\. For example, the AWS:InstanceInformation inventory type collects operating system platform information with the `Platform` data type\. By aggregating data on the `Platform` data type, you can quickly see how many instances are running Windows and how many are running Linux\. 

The procedures in this section describe how to view aggregated counts of inventory data by using the AWS CLI\. You can also view pre\-configured aggregated counts in the AWS Systems Manager console on the **Inventory** page\. These pre\-configured dashboards are called *Inventory Insights* and they offer one\-click remediation of your inventory configuration issues\.

Note the following important details about aggregation counts of inventory data:
+ Systems Manager Inventory stores inventory data for 30 days\. This means that aggregated counts of inventory include all data collected during the last 30 days\.
+ Inventory shows data that has been sent by an instance over the course of its lifetime\. If an instance was previously configured to report a specific inventory data type, for example AWS:Network, and later you change the configuration to stop collecting that type, aggregation counts still show AWS:Network data until the instance has been terminated\.
+ If an instance was previously configured to collect inventory data, and you terminate that instance, inventory counts still show data for the deleted instance for 30 days\.

For information about how to quickly configure and collect inventory data from all instances in a specific AWS account \(and any future instances that might be created in that account\) see [Configuring collection by using the console](sysman-inventory-configuring.md#sysman-inventory-config-collection)\.

**Topics**
+ [Aggregating inventory data to see counts of instances that collect specific types of data](#sysman-inventory-aggregate-type)
+ [Aggregating inventory data with groups to see which instances are and aren't configured to collect an inventory type](#sysman-inventory-aggregate-groups)

## Aggregating inventory data to see counts of instances that collect specific types of data<a name="sysman-inventory-aggregate-type"></a>

You can use the [GetInventory](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetInventory.html) API action to view aggregated counts of instances that collect one or more inventory types and data types\. For example, the AWS:InstanceInformation inventory type enables you to view an aggregate of operating systems by using the GetInventory API action with the AWS:InstanceInformation\.PlatformType data type\. Here is an example AWS CLI command and output:

```
aws ssm get-inventory --aggregators "Expression=AWS:InstanceInformation.PlatformType"
```

The system returns information like the following\.

```
{
   "Entities":[
      {
         "Data":{
            "AWS:InstanceInformation":{
               "Content":[
                  {
                     "Count":"7",
                     "PlatformType":"windows"
                  },
                  {
                     "Count":"5",
                     "PlatformType":"linux"
                  }
               ]
            }
         }
      }
   ]
}
```

**Getting started**  
Determine the inventory types and data types for which you want to view counts\. You can view a list of inventory types and data types that support aggregation by running the following command in the AWS CLI:

```
aws ssm get-inventory-schema --aggregator
```

The command returns a JSON list of inventory types and data types that support aggregation\. The **TypeName** field shows supported inventory types\. And the **Name** field shows each data type\. For example, in the following list, the AWS:Application inventory type includes data types for Name and Version\.

```
{
    "Schemas": [
        {
            "TypeName": "AWS:Application",
            "Version": "1.1",
            "DisplayName": "Application",
            "Attributes": [
                {
                    "DataType": "STRING",
                    "Name": "Name"
                },
                {
                    "DataType": "STRING",
                    "Name": "Version"
                }
            ]
        },
        {
            "TypeName": "AWS:InstanceInformation",
            "Version": "1.0",
            "DisplayName": "Platform",
            "Attributes": [
                {
                    "DataType": "STRING",
                    "Name": "PlatformName"
                },
                {
                    "DataType": "STRING",
                    "Name": "PlatformType"
                },
                {
                    "DataType": "STRING",
                    "Name": "PlatformVersion"
                }
            ]
        },
        {
            "TypeName": "AWS:ResourceGroup",
            "Version": "1.0",
            "DisplayName": "ResourceGroup",
            "Attributes": [
                {
                    "DataType": "STRING",
                    "Name": "Name"
                }
            ]
        },
        {
            "TypeName": "AWS:Service",
            "Version": "1.0",
            "DisplayName": "Service",
            "Attributes": [
                {
                    "DataType": "STRING",
                    "Name": "Name"
                },
                {
                    "DataType": "STRING",
                    "Name": "DisplayName"
                },
                {
                    "DataType": "STRING",
                    "Name": "ServiceType"
                },
                {
                    "DataType": "STRING",
                    "Name": "Status"
                },
                {
                    "DataType": "STRING",
                    "Name": "StartType"
                }
            ]
        },
        {
            "TypeName": "AWS:WindowsRole",
            "Version": "1.0",
            "DisplayName": "WindowsRole",
            "Attributes": [
                {
                    "DataType": "STRING",
                    "Name": "Name"
                },
                {
                    "DataType": "STRING",
                    "Name": "DisplayName"
                },
                {
                    "DataType": "STRING",
                    "Name": "FeatureType"
                },
                {
                    "DataType": "STRING",
                    "Name": "Installed"
                }
            ]
        }
    ]
}
```

You can aggregate data for any of the listed inventory types by creating a command that uses the following syntax:

```
aws ssm get-inventory --aggregators "Expression=InventoryType.DataType"
```

Here are some examples\.

**Example 1**

This example aggregates a count of the Windows roles used by your instances\.

```
aws ssm get-inventory --aggregators "Expression=AWS:WindowsRole.Name"
```

**Example 2**

This example aggregates a count of the applications installed on your instances\.

```
aws ssm get-inventory --aggregators "Expression=AWS:Application.Name"
```

**Combining Multiple Aggregators**  
You can also combine multiple inventory types and data types in one command to help you better understand the data\. Here are some examples\.

**Example 1**

This example aggregates a count of the operating system types used by your instances\. It also returns the specific name of the operating systems\.

```
aws ssm get-inventory --aggregators '[{"Expression": "AWS:InstanceInformation.PlatformType", "Aggregators":[{"Expression": "AWS:InstanceInformation.PlatformName"}]}]'
```

**Example 2**

This example aggregates a count of the applications running on your instances and the specific version of each application\.

```
aws ssm get-inventory --aggregators '[{"Expression": "AWS:Application.Name", "Aggregators":[{"Expression": "AWS:Application.Version"}]}]'
```

If you prefer, you can create an aggregation expression with one or more inventory types and data types in a JSON file and call the file from the AWS CLI\. The JSON in the file must use the following syntax:

```
[
       {
           "Expression": "string",
           "Aggregators": [
               {
                  "Expression": "string"
               }
           ]
       }
]
```

You must save the file with the \.json file extension\. 

Here is an example that uses multiple inventory types and data types\.

```
[
       {
           "Expression": "AWS:Application.Name",
           "Aggregators": [
               {
                   "Expression": "AWS:Application.Version",
                   "Aggregators": [
                     {
                     "Expression": "AWS:InstanceInformation.PlatformType"
                     }
                   ]
               }
           ]
       }
]
```

Use the following command to call the file from the AWS CLI\. 

```
aws ssm get-inventory --aggregators file://file_name.json
```

The command returns information like the following:

```
{"Entities": 
 [
   {"Data": 
     {"AWS:Application": 
       {"Content": 
         [
           {"Count": "3", 
            "PlatformType": "linux", 
            "Version": "2.6.5", 
            "Name": "audit-libs"}, 
           {"Count": "2", 
            "PlatformType": "windows", 
            "Version": "2.6.5", 
            "Name": "audit-libs"}, 
           {"Count": "4", 
            "PlatformType": "windows", 
            "Version": "6.2.8", 
            "Name": "microsoft office"}, 
           {"Count": "2", 
            "PlatformType": "windows", 
            "Version": "2.6.5", 
            "Name": "chrome"}, 
           {"Count": "1", 
            "PlatformType": "linux", 
            "Version": "2.6.5", 
            "Name": "chrome"}, 
           {"Count": "2", 
            "PlatformType": "linux", 
            "Version": "6.3", 
            "Name": "authconfig"}
         ]
       }
     }, 
    "ResourceType": "ManagedInstance"}
 ]
}
```

## Aggregating inventory data with groups to see which instances are and aren't configured to collect an inventory type<a name="sysman-inventory-aggregate-groups"></a>

Groups enable you to quickly see a count of which managed instances are and aren’t configured to collect one or more inventory types\. With groups, you specify one or more inventory types and a filter that uses the `exists` operator\.

For example, say that you have four managed instances configured to collect the following inventory types:
+ Instance 1: AWS:Application
+ Instance 2: AWS:File
+ Instance 3: AWS:Application, AWS:File
+ Instance 4: AWS:Network

You can run the following command from the AWS CLI to see how many instances are configured to collect both the AWS:Application and AWS:File inventory types\. The response also returns a count of how many instance aren't configured to collect both of these inventory types\.

```
aws ssm get-inventory --aggregators 'Groups=[{Name=ApplicationAndFile,Filters=[{Key=TypeName,Values=[AWS:Application],Type=Exists},{Key=TypeName,Values=[AWS:File],Type=Exists}]}]'
```

The command response shows that only one managed instance is configured to collect both the AWS:Application and AWS:File inventory types\.

```
{
   "Entities":[
      {
         "Data":{
            "ApplicationAndFile":{
               "Content":[
                  {
                     "notMatchingCount":"3"
                  },
                  {
                     "matchingCount":"1"
                  }
               ]
            }
         }
      }
   ]
}
```

**Note**  
Groups don't return data type counts\. Also, you can't drill\-down into the results to see the IDs of instances that are or aren't configured to collect the inventory type\.

If you prefer, you can create an aggregation expression with one or more inventory types in a JSON file and call the file from the AWS CLI\. The JSON in the file must use the following syntax:

```
{
   "Aggregators":[
      {
         "Groups":[
            {
               "Name":"Name",
               "Filters":[
                  {
                     "Key":"TypeName",
                     "Values":[
                        "Inventory_type"
                     ],
                     "Type":"Exists"
                  },
                  {
                     "Key":"TypeName",
                     "Values":[
                        "Inventory_type"
                     ],
                     "Type":"Exists"
                  }
               ]
            }
         ]
      }
   ]
}
```

You must save the file with the \.json file extension\. 

Use the following command to call the file from the AWS CLI\. 

```
aws ssm get-inventory --cli-input-json file://file_name.json
```

**Additional examples**  
The following examples show you how to aggregate inventory data to see which managed instances are and aren't configured to collect the specified inventory types\. These examples use the AWS CLI\. Each example includes a full command with filters that you can run from the command line and a sample input\.json file if you prefer to enter the information in a file\.

**Example 1**

This example aggregates a count of instances that are and aren't configured to collect either the AWS:Application or the AWS:File inventory types\.

Run the following command from the AWS CLI\.

```
aws ssm get-inventory --aggregators 'Groups=[{Name=ApplicationORFile,Filters=[{Key=TypeName,Values=[AWS:Application, AWS:File],Type=Exists}]}]'
```

If you prefer to use a file, copy and paste the following sample into a file and save it as input\.json\.

```
{
   "Aggregators":[
      {
         "Groups":[
            {
               "Name":"ApplicationORFile",
               "Filters":[
                  {
                     "Key":"TypeName",
                     "Values":[
                        "AWS:Application",
                        "AWS:File"
                     ],
                     "Type":"Exists"
                  }
               ]
            }
         ]
      }
   ]
}
```

Run the following command from the AWS CLI\.

```
aws ssm get-inventory --cli-input-json file://input.json
```

The command returns information like the following:

```
{
   "Entities":[
      {
         "Data":{
            "ApplicationORFile":{
               "Content":[
                  {
                     "notMatchingCount":"1"
                  },
                  {
                     "matchingCount":"3"
                  }
               ]
            }
         }
      }
   ]
}
```

**Example 2**

This example aggregates a count of instances that are and aren't configured to collect the AWS:Application, AWS:File, and AWS:Network inventory types\.

Run the following command from the AWS CLI\.

```
aws ssm get-inventory --aggregators 'Groups=[{Name=Application,Filters=[{Key=TypeName,Values=[AWS:Application],Type=Exists}]}, {Name=File,Filters=[{Key=TypeName,Values=[AWS:File],Type=Exists}]}, {Name=Network,Filters=[{Key=TypeName,Values=[AWS:Network],Type=Exists}]}]'
```

If you prefer to use a file, copy and paste the following sample into a file and save it as input\.json\.

```
{
   "Aggregators":[
      {
         "Groups":[
            {
               "Name":"Application",
               "Filters":[
                  {
                     "Key":"TypeName",
                     "Values":[
                        "AWS:Application"
                     ],
                     "Type":"Exists"
                  }
               ]
            },
            {
               "Name":"File",
               "Filters":[
                  {
                     "Key":"TypeName",
                     "Values":[
                        "AWS:File"
                     ],
                     "Type":"Exists"
                  }
               ]
            },
            {
               "Name":"Network",
               "Filters":[
                  {
                     "Key":"TypeName",
                     "Values":[
                        "AWS:Network"
                     ],
                     "Type":"Exists"
                  }
               ]
            }
         ]
      }
   ]
}
```

Run the following command from the AWS CLI\.

```
aws ssm get-inventory --cli-input-json file://input.json
```

The command returns information like the following:

```
{
   "Entities":[
      {
         "Data":{
            "Application":{
               "Content":[
                  {
                     "notMatchingCount":"2"
                  },
                  {
                     "matchingCount":"2"
                  }
               ]
            },
            "File":{
               "Content":[
                  {
                     "notMatchingCount":"2"
                  },
                  {
                     "matchingCount":"2"
                  }
               ]
            },
            "Network":{
               "Content":[
                  {
                     "notMatchingCount":"3"
                  },
                  {
                     "matchingCount":"1"
                  }
               ]
            }
         }
      }
   ]
}
```