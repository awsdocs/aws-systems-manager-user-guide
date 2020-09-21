# AWSSupport\-ShareRDSSnapshot<a name="automation-aws-sharerdssnapshot"></a>

**Description**

The `AWSSupport-ShareRDSSnapshot` document provides an automated solution for the procedure outlined in the Knowledge Center article [How can I share an encrypted Amazon RDS DB snapshot with another account?](http://aws.amazon.com/premiumsupport/knowledge-center/share-encrypted-rds-snapshot-kms-key/) If your Amazon Relational Database Service \(Amazon RDS\) snapshot was encrypted using the default AWS Key Management Service \(AWS KMS\) key, you cannot share the snapshot\. In this case, you must copy the snapshot using a customer master key \(CMK\), and then share the snapshot with the target account\. This automation performs these steps using the value you specify in the `SnapshotName` parameter, or the latest snapshot found for the selected Amazon RDS DB instance or cluster\.

**Note**  
If you do not specify a value for the `KMSKey` parameter, the automation creates a new AWS KMS CMK in your account that is used to encrypt the snapshot\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-ShareRDSSnapshot)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Databases

**Parameters**
+ AccountIds

  Type: StringList

  Description: \(Required\) Comma\-separated list of account IDs to share the snapshot with\.
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ Database

  Type: String

  Description: \(Required\) The name of the Amazon RDS DB instance or cluster whose snapshot you want to share\. This parameter is optional if you specify a value for the `SnapshotName` parameter\.
+ KMSKey

  Type: String

  Description: \(Optional\) The full Amazon Resource Name \(ARN\) of the AWS KMS CMK used to encrypt the snapshot\.
+ SnapshotName

  Type: String

  Description: \(Optional\) The ID of the DB cluster or instance snapshot that you want to use\.

**Required IAM Permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `rds:DescribeDBInstances`
+ `rds:DescribeDBSnapshots`
+ `rds:CopyDBSnapshot`
+ `rds:ModifyDBSnapshotAttribute`

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document for a DB cluster\.
+ `ssm:StartAutomationExecution`
+ `rds:DescribeDBClusters`
+ `rds:DescribeDBClusterSnapshots`
+ `rds:CopyDBClusterSnapshot`
+ `rds:ModifyDBClusterSnapshotAttribute`

The IAM role used to run the automation must be added as a key user to use the AWS KMS CMK specified in the `ARNKmsKey` parameter\. For information about adding key users to a AWS KMS CMK, see [Changing a key policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policy-modifying.html) in the *AWS Key Management Service Developer Guide*\.

The `AutomationAssumeRole` requires the following additional actions to successfully run the Automation document if you do not specify a value for the `KMSKey` parameter\.
+ `kms:CreateKey`
+ `kms:ScheduleKeyDeletion`

**Document Steps**

1. aws:executeScript \- Checks whether a value was provided for the `KMSKey` parameter, and creates a AWS KMS CMK if no value is found\.

1. aws:branch \- Checks whether a value was provided for the `SnapshotName` parameter, and branches accordingly\.

1. aws:executeAwsApi \- Checks whether the snapshot provided is from a DB instance\.

1. aws:executeScript \- Formats the `SnapshotName` parameter replacing colons with a hyphen\.

1. aws:executeAwsApi \- Copies the snapshot using the specified `KMSKey`\.

1. aws:waitForAwsResourceProperty \- Waits for the copy snapshot operation to complete\.

1. aws:executeAwsApi \- Shares the new snapshot with the `AccountIds` specified\.

1. aws:executeAwsApi \- Checks whether the snapshot provided is from a DB cluster\.

1. aws:executeScript \- Formats the `SnapshotName` parameter replacing colons with a hyphen\.

1. aws:executeAwsApi \- Copies the snapshot using the specified `KMSKey`\.

1. aws:waitForAwsResourceProperty \- Waits for the copy snapshot operation to complete\.

1. aws:executeAwsApi \- Shares the new snapshot with the `AccountIds` specified\.

1. aws:executeAwsApi \- Checks whether the value provided for the `Database` parameter is a DB instance\.

1. aws:executeAwsApi \- Checks whether the value provided for the `Database` parameter is a DB cluster\.

1. aws:executeAwsApi \- Retrieves a list of snapshots for the specified `Database`\.

1. aws:executeScript \- Determines the latest snapshot available from the list assembled in the previous step\.

1. aws:executeAwsApi \- Copies the DB instance snapshot using the specified `KMSKey`\.

1. aws:waitForAwsResourceProperty \- Waits for the copy snapshot operation to complete\.

1. aws:executeAwsApi \- Shares the new snapshot with the `AccountIds` specified\.

1. aws:executeAwsApi \- Retrieves a list of snapshots for the specified `Database`\.

1. aws:executeScript \- Determines the latest snapshot available from the list assembled in the previous step\.

1. aws:executeAwsApi \- Copies the DB instance snapshot using the specified `KMSKey`\.

1. aws:waitForAwsResourceProperty \- Waits for the copy snapshot operation to complete\.

1. aws:executeAwsApi \- Shares the new snapshot with the `AccountIds` specified\.

1. aws:executeScript \- Deletes the AWS KMS CMK created by the automation if you did not specify a value for the `KMSKey` parameter and the automation fails\.