# AWSConfigRemediation\-DeleteUnusedEBSVolume<a name="automation-aws-delete-ebs-volume"></a>

**Description**

The AWSConfigRemediation\-DeleteUnusedEBSVolume runbook deletes an unused Amazon Elastic Block Store \(Amazon EBS\) volume\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-DeleteUnusedEBSVolume)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ CreateSnapshot

  Type: Boolean

  Description: \(Optional\) If set to `True`, the automation creates a snapshot of the Amazon EBS volume before it is deleted\.
+ VolumeId

  Type: String

  Description: \(Required\) The ID of the Amazon EBS volume that you want to delete\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `ec2:CreateSnapshot`
+ `ec2:DeleteVolume`
+ `ec2:DescribeVolume`

**Document Steps**
+ aws:executeScript \- Verifies the Amazon EBS volume you specify in the `VolumeId` parameter is not in use, and creates a snapshot depending on the value you choose for the `CreateSnapshot` parameter\.
+ aws:branch \- Branches based on the value you chose for the `CreateSnapshot` parameter\.
+ aws:waitForAwsResourceProperty \- Waits for the snapshot to complete\.
+ aws:executeAwsApi \- Deletes the snapshot if the snapshot creation failed\.
+ aws:executeAwsApi \- Deletes the Amazon EBS volume you specify in the `VolumeId` parameter\.
+ aws:executeScript \- Verifies the Amazon EBS volume has been deleted\.