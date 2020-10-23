# Assign targets to a maintenance window \(console\)<a name="sysman-maintenance-assign-targets"></a>

In this procedure, you register a target with a maintenance window\. In other words, you specify which resources the maintenance window performs actions on\.

**To assign targets to a maintenance window \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Maintenance Windows**\. 

1. In the list of maintenance windows, choose the maintenance window to add targets to\.

1. Choose **Actions**, and then choose **Register targets**\.

1. \(Optional\) For **Target name**, enter a name for the targets\.

1. \(Optional\) For **Description**, enter a description\.

1. \(Optional\) For **Owner information**, specify information to include in any EventBridge event raised while running tasks for these targets in this maintenance window\.

   For information about using EventBridge to monitor Systems Manager events, see [Monitoring Systems Manager events with Amazon EventBridge](monitoring-eventbridge-events.md)\.

1. In the **Targets** area, choose one of the options described in the following table\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-maintenance-assign-targets.html)

1. Choose **Register targets**\.

If you want to assign more targets to this maintenance window, choose the **Targets** tab, and then choose **Register target**\. With this option, you can choose a different means of targeting\. For example, if you previously targeted instances by instance ID, you can register new targets and target instances by specifying tags applied to managed instances or choosing resource types from a resource group\.