# Metadata collected by inventory<a name="sysman-inventory-schema"></a>

The following sample shows the complete list of metadata collected by each AWS Systems Manager Inventory plugin\.

```
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
      { "name": "ResourceType",                           "dataType" : "STRING"},
      { "name": "AgentStatus",                            "dataType" : "STRING"},
      { "name": "InstanceStatus",                         "dataType" : "STRING"}
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
      { "name": "Release",            "dataType": "STRING"},
      { "name": "Epoch",              "dataType": "STRING"},
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
      { "name": "Size",    	      "dataType": "STRING"},
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
    "typeName" : "AWS:Certificate",
    "version": "1.0",
    "attributes":[
      { "name": "Name",               "dataType": "STRING"},
      { "name": "SignatureAlgorithm", "dataType": "STRING"},
      { "name": "KeyAlgorithm",       "dataType": "STRING"},
      { "name": "SerialNumber",       "dataType": "STRING"},
      { "name": "Issuer",             "dataType": "STRING"},
      { "name": "ValidityFrom",       "dataType": "STRING"},
      { "name": "ValidityTo",         "dataType": "STRING"},
      { "name": "Subject",            "dataType": "STRING"}
    ]
  },
  {
    "typeName" : "AWS:Process",
    "version": "1.0",
    "attributes":[
      { "name": "StartTime",          "dataType": "STRING"},
      { "name": "CommandLine",        "dataType": "STRING"},
      { "name": "User",               "dataType": "STRING"},
      { "name": "FileName",           "dataType": "STRING"},
      { "name": "FileVersion",        "dataType": "STRING"},
      { "name": "FileDescription",    "dataType": "STRING"},
      { "name": "FileSize",           "dataType": "STRING"},
      { "name": "CompanyName",        "dataType": "STRING"},
      { "name": "ProductName",        "dataType": "STRING"},
      { "name": "ProductVersion",     "dataType": "STRING"},
      { "name": "InstalledDate",      "dataType": "STRING"},
      { "name": "InstalledDir",       "dataType": "STRING"},
      { "name": "UsageId",            "dataType": "STRING"}
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
      { "name": "PatchGroup",                       "dataType": "STRING"},
      { "name": "BaselineId",                       "dataType": "STRING"},
      { "name": "SnapshotId",                       "dataType": "STRING"},
      { "name": "OwnerInformation",                 "dataType": "STRING"},
      { "name": "InstalledCount",                   "dataType": "NUMBER"},
      { "name": "InstalledPendingRebootCount",      "dataType": "NUMBER"},
      { "name": "InstalledOtherCount",              "dataType": "NUMBER"},
      { "name": "InstalledRejectedCount",           "dataType": "NUMBER"},
      { "name": "NotApplicableCount",               "dataType": "NUMBER"},
      { "name": "UnreportedNotApplicableCount",     "dataType": "NUMBER"},
      { "name": "MissingCount",                     "dataType": "NUMBER"},
      { "name": "FailedCount",                      "dataType": "NUMBER"},
      { "name": "OperationType",                    "dataType": "STRING"},
      { "name": "OperationStartTime",               "dataType": "STRING"},
      { "name": "OperationEndTime",                 "dataType": "STRING"},
      { "name": "InstallOverrideList",              "dataType": "STRING"},
      { "name": "RebootOption",                     "dataType": "STRING"},
      { "name": "LastNoRebootInstallOperationTime", "dataType": "STRING"},
      { "name": "ExecutionId",                      "dataType": "STRING",                 "isOptional": "true"},
      { "name": "NonCompliantSeverity",             "dataType": "STRING",                 "isOptional": "true"},
      { "name": "SecurityNonCompliantCount",        "dataType": "NUMBER",                 "isOptional": "true"},
      { "name": "CriticalNonCompliantCount",        "dataType": "NUMBER",                 "isOptional": "true"},
      { "name": "OtherNonCompliantCount",           "dataType": "NUMBER",                 "isOptional": "true"}
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
      { "name": "ComplianceType",               "dataType": "STRING",                 "isContext": "true"},
      { "name": "ExecutionId",                  "dataType": "STRING",                 "isContext": "true"},
      { "name": "ExecutionType",                "dataType": "STRING",                 "isContext": "true"},
      { "name": "ExecutionTime",                "dataType": "STRING",                 "isContext": "true"},
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
      { "name": "InstalledTime",                "dataType": "STRING"},
      { "name": "InstallOverrideList",          "dataType": "STRING",                 "isOptional": "true"},
      { "name": "DetailedText",                 "dataType": "STRING",                 "isOptional": "true"},
      { "name": "DetailedLink",                 "dataType": "STRING",                 "isOptional": "true"},
      { "name": "CVEIds",                       "dataType": "STRING",                 "isOptional": "true"}
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
    },
    {
      "typeName": "AWS:Tag",
      "version":"1.0",
      "attributes":[
        { "name": "Key",                     "dataType": "STRING"},
        { "name": "Value",                   "dataType": "STRING"}
      ]
    },
    {
      "typeName": "AWS:ResourceGroup",
      "version":"1.0",
      "attributes":[
        { "name": "Name",                   "dataType": "STRING"},
        { "name": "Arn",                    "dataType": "STRING"}
      ]
    },
    {
      "typeName": "AWS:BillingInfo",
      "version": "1.0",
      "attributes": [
        { "name": "BillingProductId",       "dataType": "STRING"}
      ]
    }
```

**Note**  
With the release of version 2\.5, RPM Package Manager replaced the Serial attribute with Epoch\. The Epoch attribute is a monotonically increasing integer like Serial\. When you inventory by using the `AWS:Application` type, a larger value for Epoch means a newer version\. If Epoch values are the same or empty, then use the value of the Version or Release attribute to determine the newer version\. 