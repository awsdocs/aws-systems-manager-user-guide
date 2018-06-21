# Working with Custom Inventory<a name="sysman-inventory-custom"></a>

You can assign any metadata you want to your instances by creating *custom inventory*\. For example, let's say you manage a large number of servers in racks in your data center, and these servers have been configured as Systems Manager managed instances\. Currently, you store information about server rack location in a spreadsheet\. With custom inventory, you can specify the rack location of each instance as metadata on the instance\. When you collect Inventory by using Systems Manager, the metadata is collected with other Inventory metadata\. You can then port all Inventory metadata to a central Amazon S3 bucket by using [Resource Data Sync](sysman-inventory-resource-data-sync.html) and query the data\.

To assign custom inventory to an instance, you can either use the Systems Manager [PutInventory](http://docs.aws.amazon.com/ssm/latest/APIReference/API_PutInventory.html) API action, as described in [Walkthrough: Assign Custom Inventory Metadata to an Instance](sysman-inventory-walk-custom.md)\. Or, you can create a custom inventory JSON file and upload it to the instance\. This section describes how to create the JSON file\.

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