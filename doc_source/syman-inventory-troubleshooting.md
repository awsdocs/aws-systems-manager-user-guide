# Troubleshooting problems with Systems Manager Inventory<a name="syman-inventory-troubleshooting"></a>

This topic includes information about how to troubleshoot common errors or problems with AWS Systems Manager Inventory\. If you're having trouble viewing your nodes in Systems Manager, see [Troubleshooting managed node availability](troubleshooting-managed-instances.md)\.

**Topics**
+ [Multiple apply all associations with document '`AWS-GatherSoftwareInventory`' are not supported](#systems-manager-inventory-troubleshooting-multiple)
+ [Inventory execution status never exits pending](#sysman-inventory-troubleshooting-pending)
+ [The `AWS-ListWindowsInventory` document fails to run](#sysman-inventory-troubleshooting-ListWindowsInventory)
+ [Console doesn't display Inventory Dashboard \| Detailed View \| Settings tabs](#sysman-inventory-troubleshooting-tabs)
+ [UnsupportedAgent](#sysman-inventory-troubleshooting-unsupported-agent)
+ [Skipped](#sysman-inventory-troubleshooting-skipped)
+ [Failed](#sysman-inventory-troubleshooting-failed)
+ [Inventory compliance failed for an Amazon EC2 instance](#sysman-inventory-troubleshooting-ec2-compliance)

## Multiple apply all associations with document '`AWS-GatherSoftwareInventory`' are not supported<a name="systems-manager-inventory-troubleshooting-multiple"></a>

An error that `Multiple apply all associations with document 'AWS-GatherSoftwareInventory' are not supported` means that one or more AWS Regions where you're trying to configure an Inventory association *for all nodes* are already configured with an inventory association for all nodes\. If necessary, you can delete the existing inventory association for all nodes and then create a new one\. To view existing inventory associations, choose **State Manager** in the Systems Manager console and then locate associations that use the `AWS-GatherSoftwareInventory` SSM document\. If the existing inventory association for all nodes was created across multiple Regions, and you want to create a new one, you must delete the existing association from each Region where it exists\.

## Inventory execution status never exits pending<a name="sysman-inventory-troubleshooting-pending"></a>

There are two reasons why inventory collection never exits the `Pending` status:
+ No nodes in the selected AWS Region:

  If you create a global inventory association by using Systems Manager Quick Setup, the status of the inventory association \(`AWS-GatherSoftwareInventory` document\) shows `Pending` if there are no nodes available in the selected Region\.****
+ Insufficient permissions:

  An inventory association shows `Pending` if one or more nodes don't have permission to run Systems Manager Inventory\. Verify that the AWS Identity and Access Management \(IAM\) instance profile includes the **AmazonSSMManagedInstanceCore** managed policy\. For information about how to add this policy to an instance profile, see [Task 2: Add permissions to a Systems Manager instance profile \(console\)](setup-instance-profile.md#instance-profile-add-permissions)\.

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

If the detailed status of an inventory association shows **UnsupportedAgent**, and the **Association status** shows **Failed**, then the version of AWS Systems Manager SSM Agent on the managed node isn't correct\. To create a global inventory association \(to inventory all nodes in your AWS account\) for example, you must use SSM Agent version 2\.0\.790\.0 or later\. You can view the agent version running on each of your nodes on the **Managed Instances** page in the **Agent version** column\. For information about how to update SSM Agent on your nodes, see [Updating the SSM Agent using Run Command](run-command-tutorial-update-software.md#rc-console-agentexample)\.

## Skipped<a name="sysman-inventory-troubleshooting-skipped"></a>

If the status of the inventory association for a node shows **Skipped**, this means that you created a *global inventory association* \(to collect inventory from all nodes\), but the skipped node already had an inventory association assigned to it\. The global inventory association wasn't assigned to this node, and no inventory was collected by the global inventory association\. However, the node will still report inventory data when the existing inventory association runs\.

If you don't want the node to be skipped by the global inventory association, you must delete the existing inventory association\. To view existing inventory associations, choose **State Manager** in the Systems Manager console and then locate associations that use the `AWS-GatherSoftwareInventory` SSM document\.

## Failed<a name="sysman-inventory-troubleshooting-failed"></a>

If the status of the inventory association for a node shows **Failed**, this could mean that the node has multiple inventory associations assigned to it\. A node can only have one inventory association assigned at a time\. An inventory association uses the `AWS-GatherSoftwareInventory` AWS Systems Manager document \(SSM document\)\. You can run the following command by using the AWS Command Line Interface \(AWS CLI\) to view a list of associations for a node\.

```
aws ssm describe-instance-associations-status --instance-id instance ID
```

## Inventory compliance failed for an Amazon EC2 instance<a name="sysman-inventory-troubleshooting-ec2-compliance"></a>

Inventory compliance for an Amazon Elastic Compute Cloud \(Amazon EC2\) instance can fail if you assign multiple inventory associations to the instance\. 

 To resolve this issue, delete one or more inventory associations assigned to the instance\. For more information, see [Deleting an association](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-state-manager-delete-association.html)\. 

**Note**  
Be aware of the following behavior if you create multiple inventory associations for a managed node:  
Each node can be assigned an inventory association that targets *all* nodes \(\-\-targets "Key=InstanceIds,Values=\*"\)\.
Each node can also be assigned a specific association that uses either tag key\-value pairs or an AWS resource group\.
If a node is assigned multiple inventory associations, the status shows *Skipped* for the association that hasn't run\. The association that ran most recently displays the actual status of the inventory association\. 
If a node is assigned multiple inventory associations and each uses a tag key\-value pair, then those inventory associations fail to run on the node because of the tag conflict\. The association still runs on nodes that don't have the tag key\-value conflict\. 