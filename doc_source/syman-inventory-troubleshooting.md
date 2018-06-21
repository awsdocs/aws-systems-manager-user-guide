# Troubleshooting Problems with Systems Manager Inventory<a name="syman-inventory-troubleshooting"></a>

This topic includes information about how to troubleshoot common errors or problems with Systems Manager Inventory\.

**UnsupportedAgent**

If the detailed status of an inventory association shows **UnsupportedAgent**, and the **Association status** shows **Failed**, then the version of SSM Agent on the instance is not correct\. To create a global inventory association \(to inventory all instances in your AWS account\) for example, you must use SSM Agent version 2\.0\.790\.0 or later\. You can view the agent version running on each of your instances on the **Managed Instances** page in the **Agent version** column\. For information about how to update SSM Agent on your instances, see [Example: Update the SSM Agent](rc-console.md#rc-console-agentexample)\.

**Skipped**

If the status of the inventory association for an instance shows **Skipped**, this means that you created a global inventory association, but the skipped instance already had an inventory association assigned to it\. The global inventory association was not assigned to this instance, and no inventory was collected by the global inventory association\. However, the instance will still report inventory data when the specific inventory association runs\.

**Failed**

If the status of the inventory association for an instance shows **Failed**, this could mean that the instance has multiple inventory associations assigned to it\. An instance can only have one inventory association assigned at a time\. An inventory association uses the AWS\-GatherSoftwareInventory SSM document\. You can execute the following command by using the AWS CLI to view a list of associations for an instance\.

```
aws ssm describe-instance-associations-status --instance-id instance ID
```