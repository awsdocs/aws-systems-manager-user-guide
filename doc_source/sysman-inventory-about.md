# About Systems Manager Inventory<a name="sysman-inventory-about"></a>

When you configure Systems Manager Inventory, you specify the type of metadata to collect, the instances from where the metadata should be collected, and a schedule for metadata collection\. These configurations are saved with your AWS account as a State Manager association\.

**Note**  
Inventory only collects metadata\. It does not collect any personal or proprietary data\.

The following table describes the different aspects of Inventory collection that you configure\.


****  

| Configuration | Details | 
| --- | --- | 
|  Type of information to collect  | You can configure Inventory to collect the following types of metadata: [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-inventory-about.html)  To view a list of all metadata collected by Inventory, see [Metadata Collected by Inventory](#sysman-inventory-schema)\.   | 
|  Instances to collect information from  |  You can individually select instances or target groups of instances by using Amazon EC2 tags\.   | 
|  When to collect information  |  You can specify a collection interval in terms of minutes, hours, days, and weeks\. The shortest collection interval is every 30 minutes\.   | 

Depending on the amount of data collected, the system can take several minutes to report the data to the output you specified\. After the information is collected, the metadata is sent over a secure HTTPS channel to a plain\-text AWS store that is accessible only from your AWS account\. You can view the data in the Amazon S3 bucket you specified, or in the Amazon EC2 console on the **Inventory** tab for your managed instance\. The **Inventory** tab includes several predefined filters to help you query the data\.

To start collecting inventory on your managed instance, see [Configuring Inventory Collection](sysman-inventory-configuring.md) and [Collecting Inventory by Using the AWS CLI](sysman-inventory-walk.md#sysman-inventory-cliwalk)\.

## Metadata Collected by Inventory<a name="sysman-inventory-schema"></a>

The following sample shows the complete list of metadata collected by each Inventory plugin\. 

```
[
  {
    "typeName": "AWS:InstanceInformation",
    "version": "1.0",
    "attributes":[
      { "name": "AgentType",                              "dataType" : "STRING"},
      { "name": "AgentVersion",                           "dataType" : "STRING"},
      { "name": "ComputerName",                           "dataType" : "STRING"},
      { "name": "IamRole",                                "dataType" : "STRING"},
      { "name": "InstanceId",                             "dataType" : "STRING"},
      { "name": "IpAddress",                              "dataType" : "STRING"},
      { "name": "PlatformName",                           "dataType" : "STRING"},
      { "name": "PlatformType",                           "dataType" : "STRING"},
      { "name": "PlatformVersion",                        "dataType" : "STRING"},
      { "name": "ResourceType",                           "dataType" : "STRING"}
    ]
  },
  {
    "typeName" : "AWS:Application",
    "version": "1.1",
    "attributes":[
      { "name": "Name",               "dataType": "STRING"},
      { "name": "ApplicationType",    "dataType": "STRING"},
      { "name": "Publisher",          "dataType": "STRING"},
      { "name": "Version",            "dataType": "STRING"},
      { "name": "InstalledTime",      "dataType": "STRING"},
      { "name": "Architecture",       "dataType": "STRING"},
      { "name": "URL",                "dataType": "STRING"},
      { "name": "Summary",            "dataType": "STRING"},
      { "name": "PackageId",          "dataType": "STRING"}
    ]
  },
  {
    "typeName" : "AWS:File",
    "version": "1.0",
    "attributes":[
      { "name": "Name",               "dataType": "STRING"},
      { "name": "Size",    	      	  "dataType": "STRING"},
      { "name": "Description",        "dataType": "STRING"},
      { "name": "FileVersion",        "dataType": "STRING"},
      { "name": "InstalledDate",      "dataType": "STRING"},
      { "name": "ModificationTime",   "dataType": "STRING"},
      { "name": "LastAccessTime",     "dataType": "STRING"},
      { "name": "ProductName",        "dataType": "STRING"},
      { "name": "InstalledDir",       "dataType": "STRING"},
      { "name": "ProductLanguage",    "dataType": "STRING"},
      { "name": "CompanyName",        "dataType": "STRING"},
      { "name": "ProductVersion",       "dataType": "STRING"}
    ]
  },
  {
    "typeName": "AWS:AWSComponent",
    "version": "1.0",
    "attributes":[
      { "name": "Name",               "dataType": "STRING"},
      { "name": "ApplicationType",    "dataType": "STRING"},
      { "name": "Publisher",          "dataType": "STRING"},
      { "name": "Version",            "dataType": "STRING"},
      { "name": "InstalledTime",      "dataType": "STRING"},
      { "name": "Architecture",       "dataType": "STRING"},
      { "name": "URL",                "dataType": "STRING"}
    ]
  },
  {
    "typeName": "AWS:WindowsUpdate",
    "version":"1.0",
    "attributes":[
      { "name": "HotFixId",           "dataType": "STRING"},
      { "name": "Description",        "dataType": "STRING"},
      { "name": "InstalledTime",      "dataType": "STRING"},
      { "name": "InstalledBy",        "dataType": "STRING"}
    ]
  },
  {
    "typeName": "AWS:Network",
    "version":"1.0",
    "attributes":[
      { "name": "Name",               "dataType": "STRING"},
      { "name": "SubnetMask",         "dataType": "STRING"},
      { "name": "Gateway",            "dataType": "STRING"},
      { "name": "DHCPServer",         "dataType": "STRING"},
      { "name": "DNSServer",          "dataType": "STRING"},
      { "name": "MacAddress",         "dataType": "STRING"},
      { "name": "IPV4",               "dataType": "STRING"},
      { "name": "IPV6",               "dataType": "STRING"}
    ]
  },
  {
    "typeName": "AWS:PatchSummary",
    "version":"1.0",
    "attributes":[
      { "name": "PatchGroup",                   "dataType": "STRING"},
      { "name": "BaselineId",                   "dataType": "STRING"},
      { "name": "SnapshotId",                   "dataType": "STRING"},
      { "name": "OwnerInformation",             "dataType": "STRING"},
      { "name": "InstalledCount",               "dataType": "NUMBER"},
      { "name": "InstalledOtherCount",          "dataType": "NUMBER"},
      { "name": "NotApplicableCount",           "dataType": "NUMBER"},
      { "name": "MissingCount",                 "dataType": "NUMBER"},
      { "name": "FailedCount",                  "dataType": "NUMBER"},
      { "name": "OperationType",                "dataType": "STRING"},
      { "name": "OperationStartTime",           "dataType": "STRING"},
      { "name": "OperationEndTime",             "dataType": "STRING"}
    ]
  },
  {
    "typeName": "AWS:PatchCompliance",
    "version":"1.0",
    "attributes":[
      { "name": "Title",                        "dataType": "STRING"},
      { "name": "KBId",                         "dataType": "STRING"},
      { "name": "Classification",               "dataType": "STRING"},
      { "name": "Severity",                     "dataType": "STRING"},
      { "name": "State",                        "dataType": "STRING"},
      { "name": "InstalledTime",                "dataType": "STRING"}
    ]
  },
  {
    "typeName": "AWS:ComplianceItem",
    "version":"1.0",
    "attributes":[
      { "name": "ComplianceType",               "dataType": "STRING",                 
      { "name": "ExecutionId",                  "dataType": "STRING",                 
      { "name": "ExecutionType",                "dataType": "STRING",                 
      { "name": "ExecutionTime",                "dataType": "STRING",                 
      { "name": "Id",                           "dataType": "STRING"},
      { "name": "Title",                        "dataType": "STRING"},
      { "name": "Status",                       "dataType": "STRING"},
      { "name": "Severity",                     "dataType": "STRING"},
      { "name": "DocumentName",                 "dataType": "STRING"},
      { "name": "DocumentVersion",              "dataType": "STRING"},
      { "name": "Classification",               "dataType": "STRING"},
      { "name": "PatchBaselineId",              "dataType": "STRING"},
      { "name": "PatchSeverity",                "dataType": "STRING"},
      { "name": "PatchState",                   "dataType": "STRING"},
      { "name": "PatchGroup",                   "dataType": "STRING"},
      { "name": "InstalledTime",                "dataType": "STRING"}
    ]
  },
  {
    "typeName": "AWS:ComplianceSummary",
    "version":"1.0",
    "attributes":[
      { "name": "ComplianceType",                 "dataType": "STRING"},
      { "name": "PatchGroup",                     "dataType": "STRING"},
      { "name": "PatchBaselineId",                "dataType": "STRING"},
      { "name": "Status",                         "dataType": "STRING"},
      { "name": "OverallSeverity",                "dataType": "STRING"},
      { "name": "ExecutionId",                    "dataType": "STRING"},
      { "name": "ExecutionType",                  "dataType": "STRING"},
      { "name": "ExecutionTime",                  "dataType": "STRING"},
      { "name": "CompliantCriticalCount",         "dataType": "NUMBER"},
      { "name": "CompliantHighCount",             "dataType": "NUMBER"},
      { "name": "CompliantMediumCount",           "dataType": "NUMBER"},
      { "name": "CompliantLowCount",              "dataType": "NUMBER"},
      { "name": "CompliantInformationalCount",    "dataType": "NUMBER"},
      { "name": "CompliantUnspecifiedCount",      "dataType": "NUMBER"},
      { "name": "NonCompliantCriticalCount",      "dataType": "NUMBER"},
      { "name": "NonCompliantHighCount",          "dataType": "NUMBER"},
      { "name": "NonCompliantMediumCount",        "dataType": "NUMBER"},
      { "name": "NonCompliantLowCount",           "dataType": "NUMBER"},
      { "name": "NonCompliantInformationalCount", "dataType": "NUMBER"},
      { "name": "NonCompliantUnspecifiedCount",   "dataType": "NUMBER"}
    ]
  },
  {
    "typeName": "AWS:InstanceDetailedInformation",
    "version":"1.0",
    "attributes":[
      { "name": "CPUModel",                     "dataType": "STRING"},
      { "name": "CPUCores",                     "dataType": "NUMBER"},
      { "name": "CPUs",                         "dataType": "NUMBER"},
      { "name": "CPUSpeedMHz",                  "dataType": "NUMBER"},
      { "name": "CPUSockets",                   "dataType": "NUMBER"},
      { "name": "CPUHyperThreadEnabled",        "dataType": "STRING"},
      { "name": "OSServicePack",                "dataType": "STRING"}
    ]
   },
   {
     "typeName": "AWS:Service",
     "version":"1.0",
     "attributes":[
       { "name": "Name",                         "dataType": "STRING"},
       { "name": "DisplayName",                  "dataType": "STRING"},
       { "name": "ServiceType",                  "dataType": "STRING"},
       { "name": "Status",                       "dataType": "STRING"},
       { "name": "DependentServices",            "dataType": "STRING"},
       { "name": "ServicesDependedOn",           "dataType": "STRING"},
       { "name": "StartType",                    "dataType": "STRING"}
     ]
    },
    {
      "typeName": "AWS:WindowsRegistry",
      "version":"1.0",
      "attributes":[
        { "name": "KeyPath",                         "dataType": "STRING"},
        { "name": "ValueName",                       "dataType": "STRING"},
        { "name": "ValueType",                       "dataType": "STRING"},
        { "name": "Value",                           "dataType": "STRING"}
      ]
    },
    {
      "typeName": "AWS:WindowsRole",
      "version":"1.0",
      "attributes":[
        { "name": "Name",                         "dataType": "STRING"},
        { "name": "DisplayName",                  "dataType": "STRING"},
        { "name": "Path",                         "dataType": "STRING"},
        { "name": "FeatureType",                  "dataType": "STRING"},
        { "name": "DependsOn",                    "dataType": "STRING"},
        { "name": "Description",                  "dataType": "STRING"},
        { "name": "Installed",                    "dataType": "STRING"},
        { "name": "InstalledState",               "dataType": "STRING"},
        { "name": "SubFeatures",                  "dataType": "STRING"},
        { "name": "ServerComponentDescriptor",    "dataType": "STRING"},
        { "name": "Parent",                       "dataType": "STRING"}
      ]
    }
]
```

## Working with File and Windows Registry Inventory<a name="sysman-inventory-file-and-registry"></a>

Systems Manager Inventory enables you to search and inventory files on Windows and Linux operating systems\. You can also search and inventory the Windows Registry\.

**Files**: You can collect metadata information about files, including file names, the time files were created, the time files were last modified and accessed, and file sizes, to name a few\. To start collecting file inventory, you specify a file path where you want to perform the inventory, one or more patterns that define the types of files you want to inventory, and if the path should be traversed recursively\. Systems Manager inventories all file metadata for files in the specified path that match the pattern\. File inventory uses the following parameter input\.

```
{
"Path": string,
"Pattern": array[string],
"Recursive": true,
"DirScanLimit" : number // Optional
}
```

+ **Path**: The directory path where you want to inventory files\. For Windows, you can use environment variables like %PROGRAMFILES% as long as the variable maps to a single directory path\. For example, if you use %PATH% that maps to multiple directory paths, Inventory throws an error\. 

+ **Pattern**: An array of patterns to identify files\.

+ **Recursive**: A Boolean value indicating whether Inventory should recursively traverse the directories\.

+ **DirScanLimit**: An optional value specifying how many directories to scan\. Use this parameter to minimize performance impact on your instances\. By default, Inventory scans a maximum of 5,000 directories\.

**Note**  
Inventory collects metadata for a maximum of 500 files across all specified paths\.

Here are some examples of how to specify the parameters when performing an inventory of files\.

+ On Linux, collect metadata of \.sh files in the `/home/ec2-user` directory, excluding all subdirectories\.

  ```
  [{"Path":"/home/ec2-user","Pattern":["*.sh", "*.sh"],"Recursive":false}]
  ```

+ On Windows, collect metadata of all "\.exe" files in the Program Files folder, including subdirectories recursively\.

  ```
  [{"Path":"C:\Program Files","Pattern":["*.exe"],"Recursive":true}]
  ```

+ On Windows, collect metadata of specific log patterns\.

  ```
  [{"Path":"C:\ProgramData\Amazon","Pattern":["*amazon*.log"],"Recursive":true}]
  ```

+ Limit the directory count when performing recursive collection\.

  ```
  [{"Path":"C:\Users","Pattern":["*.ps1"],"Recursive":true, "DirScanLimit": 1000}]
  ```

**Windows Registry**: You can collect Windows Registry keys and values\. You can choose a key path and collect all keys and values recursively\. You can also collect a specific registry key and its value for a specific path\. Inventory collects the key path, name, type, and the value\.

```
{
"Path": string, 
"Recursive": boolean,
"ValueNames": array[string] // optional
}
```

+ **Path**: The path to the Registry key\.

+ **Recursive**: A Boolean value indicating whether Inventory should recursively traverse Registry paths\.

+ **ValueNames**: An array of value names for performing inventory of Registry keys\. If you use this parameter, Systems Manager will inventory only the specified value names for the specified path\.

**Note**  
Inventory collects a maximum of 250 Registry key values for all specified paths\.

Here are some examples of how to specify the parameters when performing an inventory of the Windows Registry\.

+ Collect all keys and values recursively for a specific path\.

  ```
  [{"Path":"HKEY_LOCAL_MACHINE\SOFTWARE\Amazon","Recursive": true}]
  ```

+ Collect all keys and values for a specific path \(recursive search disabled\)\.

  ```
  [{"Path":"HKEY_LOCAL_MACHINE\SOFTWARE\Intel\PSIS\PSIS_DECODER", "Recursive": false}]
  ```

+ Collect a specific key by using the `ValueNames` option\.

  ```
  {"Path":"HKEY_LOCAL_MACHINE\SOFTWARE\Amazon\MachineImage","ValueNames":["AMIName"]}
  ```

## Working with Custom Inventory<a name="sysman-inventory-custom"></a>

You can assign any metadata you want to your instances by creating *custom inventory*\. For example, let's say you manage a large number of servers in racks in your data center, and these servers have been configured as Systems Manager managed instances\. Currently, you store information about server rack location in a spreadsheet\. With custom inventory, you can specify the rack location of each instance as metadata on the instance\. When you collect Inventory by using Systems Manager, the metadata is collected with other Inventory metadata\. You can then port all Inventory metadata to a central Amazon S3 bucket by using [Resource Data Sync](sysman-inventory-resource-data-sync.html) and query the data\.

To assign custom inventory to an instance, you can either use the Systems Manager [PutInventory](http://docs.aws.amazon.com/ssm/latest/APIReference/API_PutInventory.html) API action, as described in [Assigning Custom Inventory Metadata to an Instance](sysman-inventory-walk.md#sysman-inventory-walk-custom)\. Or, you can create a custom inventory JSON file and upload it to the instance\. This section describes how to create the JSON file\.

```
{
    "SchemaVersion": "1.0",
    "TypeName": "Custom:RackInformation",
    "Content": {
        "Location": "US-EAST-01.DC.RACK1",
        "InstalledTime": "2016-01-01T01:01:01Z",
        "vendor": "DELL",
        "Zone" : "BJS12",
        "TimeZone": "UTC-8"
      }
 }
```

You can also specify multiple items in the file, as shown in the following example\.

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
You must save the file with a \.json extension\.

After you create the file, you must save it on the instance\. The following table shows the location where custom inventory JSON files must be stored on the instance:


****  

| Operating System | Path | 
| --- | --- | 
|  Windows  |  %SystemDrive%\\ProgramData\\Amazon\\SSM\\InstanceData\\<instance\-id>\\inventory\\custom  | 
|  Linux  |  /var/lib/amazon/ssm/<instance\-id>/inventory/custom  | 

For an example of how to use custom inventory, see [Get Disk Utilization of Your Fleet Using EC2 Systems Manager Custom Inventory Types](https://aws.amazon.com/blogs/mt/get-disk-utilization-of-your-fleet-using-ec2-systems-manager-custom-inventory-types/)\.

## Related AWS Services<a name="sysman-inventory-relatedsvc"></a>

Systems Manager Inventory provides a snapshot of your current inventory to help you manage software policy and improve the security posture of your entire fleet\. You can extend your inventory management and migration capabilities using the following AWS services\.

+ AWS Config provides a historical record of changes to your inventory, along with the ability to create rules to generate notifications when a configuration item is changed\. For more information, see, [Recording Amazon EC2 managed instance inventory](http://docs.aws.amazon.com/config/latest/developerguide/resource-config-reference.html#recording-managed-instance-inventory) in the *AWS Config Developer Guide*\.

+ AWS Application Discovery Service is designed to collect inventory on OS type, application inventory, processes, connections, and server performance metrics from your on\-premises VMs to support a successful migration to AWS\. For more information, see the [Application Discovery Service User Guide](http://docs.aws.amazon.com/application-discovery/latest/userguide/)\.