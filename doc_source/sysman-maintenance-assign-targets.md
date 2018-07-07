# Assign Targets to a Maintenance Window \(Console\)<a name="sysman-maintenance-assign-targets"></a>

After you create a Maintenance Window, you assign targets where the tasks will run\.

**To assign targets to a Maintenance Window**

1. In the Maintenance Window list, choose the Maintenance Window you just created\.

1. Choose **Actions**, and then choose **Register targets**\.

1. In the **Target Name** field, type a name for the targets\.

1. In the **Description** field, type a description\.

1. In the **Owner information** field, specify your name or work alias\. Owner information is included in any CloudWatch Events raised while running tasks for these targets in this Maintenance Window\. 

1. In the **Select targets by** section, choose **Specifying Tags** to target instances by using Amazon EC2 tags that you previously assigned to the instances\. Choose **Manually Selecting Instances** to choose individual instances according to their instance ID\.
**Note**  
If you don't see the instances that you'd like to target, verify that those instances are configured for Systems Manager\. For more information, see [Setting Up AWS Systems Manager](systems-manager-setting-up.md)\.

1. Choose **Register targets**\.

If you want to assign more targets to this window, choose the **Targets** tab, and then choose **Register new targets**\. With this option, you can choose a different means of targeting\. For example, if you previously targeted instances by instance ID, you can register new targets and target instances by specifying Amazon EC2 tags\.