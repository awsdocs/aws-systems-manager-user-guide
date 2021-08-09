# Troubleshooting problems with Systems Manager Inventory<a name="syman-inventory-troubleshooting"></a>

This topic includes information about how to troubleshoot common errors or problems with AWS Systems Manager Inventory\. If you're having trouble viewing your instances in Systems Manager, see [Troubleshooting Amazon EC2 managed instance availability](troubleshooting-managed-instances.md)\.

**Topics**
+ [Multiple apply all associations with document '`AWS-GatherSoftwareInventory`' are not supported](#systems-manager-inventory-troubleshooting-multiple)
+ [Inventory execution status never exits pending](#sysman-inventory-troubleshooting-pending)
+ [The `AWS-ListWindowsInventory` document fails to run](#sysman-inventory-troubleshooting-ListWindowsInventory)
+ [Console doesn't display Inventory Dashboard \| Detailed View \| Settings tabs](#sysman-inventory-troubleshooting-tabs)
+ [UnsupportedAgent](#sysman-inventory-troubleshooting-unsupported-agent)
+ [Skipped](#sysman-inventory-troubleshooting-skipped)
+ [Failed](#sysman-inventory-troubleshooting-failed)

## Multiple apply all associations with document '`AWS-GatherSoftwareInventory`' are not supported<a name="systems-manager-inventory-troubleshooting-multiple"></a>

An error that `Multiple apply all associations with document 'AWS-GatherSoftwareInventory' are not supported` means that one or more AWS Regions where you're trying to configure an Inventory association *for all instances* are already configured with an inventory association for all instances\. If necessary, you can delete the existing inventory association for all instance and then create a new one\. To view existing inventory associations, choose **State Manager** in the Systems Manager console and then locate associations that use the `AWS-GatherSoftwareInventory` SSM document\. If the existing inventory association for all instances was created across multiple Regions, and you want to create a new one, you must delete the existing association from each Region where it exists\.

## Inventory execution status never exits pending<a name="sysman-inventory-troubleshooting-pending"></a>

There are two reasons why inventory collection never exits the `Pending` status\.

1. No instances in the selected AWS Region:

   If you create a global inventory association by using Systems Manager Quick Setup, the status of the inventory association \(`AWS-GatherSoftwareInventory` document\) shows `Pending` if there are no instances available in the selected Region\.****

1. Insufficient permissions:

   An inventory association shows `Pending` if one or more instances don't have permission to run Systems Manager Inventory\. Verify that the AWS Identity and Access Management \(IAM\) instance profile includes the **AmazonSSMManagedInstanceCore** managed policy\. For information about how to add this policy to an instance profile, see [Task 2: Add permissions to a Systems Manager instance profile \(console\)](setup-instance-profile.md#instance-profile-add-permissions)\.

   At a minimum, the instance profile must have the following IAM permissions\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": [
                   "ssm:DescribeAssociation",
                   "ssm:ListAssociations",
                   "ssm:ListInstanceAssociations",
                   "ssm:PutInventory",
                   "ssm:PutComplianceItems",
                   "ssm:UpdateAssociationStatus",
                   "ssm:UpdateInstanceAssociationStatus",
                   "ssm:UpdateInstanceInformation",
                   "ssm:GetDocument",
                   "ssm:DescribeDocument"
               ],
               "Resource": "*"
           }
       ]
   }
   ```

## The `AWS-ListWindowsInventory` document fails to run<a name="sysman-inventory-troubleshooting-ListWindowsInventory"></a>

The `AWS-ListWindowsInventory` document is deprecated\. Don't use this document to collect inventory\. Instead, use one of the processes described in [Configuring inventory collection](sysman-inventory-configuring.md)\. 

## Console doesn't display Inventory Dashboard \| Detailed View \| Settings tabs<a name="sysman-inventory-troubleshooting-tabs"></a>

The Inventory **Detailed View ** page is only available in AWS Regions that offer Amazon Athena\. If the following tabs aren't displayed on the Inventory page, it means Athena isn't available in the Region and you can't use the **Detailed View** to query data\.

![\[Displaying Inventory Dashboard | Detailed View | Settings tabs\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/inventory-detailed-view-for-error.png)

## UnsupportedAgent<a name="sysman-inventory-troubleshooting-unsupported-agent"></a>

If the detailed status of an inventory association shows **UnsupportedAgent**, and the **Association status** shows **Failed**, then the version of AWS Systems Manager SSM Agent on the instance isn't correct\. To create a global inventory association \(to inventory all instances in your AWS account\) for example, you must use SSM Agent version 2\.0\.790\.0 or later\. You can view the agent version running on each of your instances on the **Managed Instances** page in the **Agent version** column\. For information about how to update SSM Agent on your instances, see [Update SSM Agent by using Run Command](rc-console.md#rc-console-agentexample)\.

## Skipped<a name="sysman-inventory-troubleshooting-skipped"></a>

If the status of the inventory association for an instance shows **Skipped**, this means that you created a *global inventory association* \(to collect inventory from all instances\), but the skipped instance already had an inventory association assigned to it\. The global inventory association wasn't assigned to this instance, and no inventory was collected by the global inventory association\. However, the instance will still report inventory data when the existing inventory association runs\.

If you don't want the instance to be skipped by the global inventory association, you must delete the existing inventory association\. To view existing inventory associations, choose **State Manager** in the Systems Manager console and then locate associations that use the `AWS-GatherSoftwareInventory` SSM document\.

## Failed<a name="sysman-inventory-troubleshooting-failed"></a>

If the status of the inventory association for an instance shows **Failed**, this could mean that the instance has multiple inventory associations assigned to it\. An instance can only have one inventory association assigned at a time\. An inventory association uses the `AWS-GatherSoftwareInventory` AWS Systems Manager document \(SSM document\)\. You can run the following command by using the AWS Command Line Interface \(AWS CLI\) to view a list of associations for an instance\.

```
aws ssm describe-instance-associations-status --instance-id instance ID
```