# Inventory All Managed Instances in Your AWS Account<a name="inventory-management-inventory-all"></a>

You can easily inventory all managed instances in your AWS account by creating a global inventory association\. A global inventory association performs the following actions:
+ Automatically applies the global inventory configuration \(association\) to all existing managed instances in your AWS account\. Instances that already have an inventory association are skipped when the global inventory association is applied and runs\. When an instance is skipped, the detailed status message states `Overridden By Explicit Inventory Association`\. Those instances are skipped by the global association, but they will still report inventory when they run their assigned inventory association\.
+ Automatically adds new instances created in your AWS account to the global inventory association\.

**Note**  
If an instance is configured for the global inventory association, and you assign a specific association to that instance, then Systems Manager Inventory deprioritizes the global association and applies the specific association\.

You can configure a global inventory association in the AWS Systems Manager console by choosing the **Selecting all managed instances in this account** target option when you configure inventory collection\. For more information, see [Configuring Inventory Collection](sysman-inventory-configuring.md)\. To create a global inventory association by using the AWS CLI, use the wildcard option for the `instanceIds` value, as shown in the following example:

```
aws ssm create-association --name AWS-GatherSoftwareInventory --targets Key=InstanceIds,Values=* --schedule-expression "rate(1 day)" --parameters applications=Enabled,awsComponents=Enabled,customInventory=Enabled,instanceDetailedInformation=Enabled,networkConfig=Enabled,services=Enabled,windowsRoles=Enabled,windowsUpdates=Enabled
```

**Note**  
Global inventory associations are available in SSM Agent version 2\.0\.790\.0 or later\. For information about how to update SSM Agent on your instances, see [Example: Update the SSM Agent](rc-console.md#rc-console-agentexample)\.