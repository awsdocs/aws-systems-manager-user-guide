# AWSSupport\-RecoverWorkSpace<a name="automation-awssupport-recover-workspace"></a>

**Description**

The AWSSupport\-RecoverWorkSpace runbook performs recovery steps on the Amazon WorkSpaces virtual desktop, known as a WorkSpace, you specify\. The runbook reboots the WorkSpace, and if the state is still `UNHEALTHY`, restores or rebuilds the WorkSpace based on the values you specify for the input parameters\. Before using this runbook we recommend reviewing [Troubleshooting Amazon WorkSpaces Issues](https://docs.aws.amazon.com/workspaces/latest/adminguide/amazon-workspaces-troubleshooting.html) in the *Amazon WorkSpaces Administration Guide*\.

**Important**  
Restoring or rebuilding a WorkSpace is a potentially destructive action that can result in the loss of data\. This is because the WorkSpace is restored from the last available snapshot and data recovered from snapshots can be as old as 12 hours\.  
The restore option recreates both the root volume and user volume based on the most recent snapshots\. The rebuild option recreates the user volume from the most recent snapshot and recreates the WorkSpace from the image associated with the bundle the WorkSpace was created from\. Applications that were installed or system settings that were changed after the WorkSpace was created are lost\. For more information about restoring and rebuilding WorkSpaces, see [Restore a WorkSpace](https://docs.aws.amazon.com/workspaces/latest/adminguide/restore-workspace.html) and [Rebuild a WorkSpace](https://docs.aws.amazon.com/workspaces/latest/adminguide/rebuild-workspace.html) in the *Amazon WorkSpaces Administration Guide*\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-RecoverWorkSpace)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ Acknowledge

  Type: String

  Valid values: Yes

  Description: \(Required\) Entering yes means that you understand the restore and rebuild actions will try to recover the WorkSpace from the most recent snapshot, and that data restored from these snapshots can be as old as 12 hours\.
+ Reboot

  Type: String

  Valid values: Yes \| No

  Default: Yes

  Description: \(Required\) Determines whether the WorkSpace is rebooted\.
+ Rebuild

  Type: String

  Valid values: Yes \| No

  Default: No

  Description: \(Required\) Determines whether the WorkSpace is rebuilt\.
+ Restore

  Type: String

  Valid values: Yes \| No

  Default: No

  Description: \(Required\) Determines whether the WorkSpace is restored\.
+ WorkspaceId

  Type: String

  Description: \(Required\) The ID of the WorkSpace you want to recover\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully run the Automation document\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `workspaces:DescribeWorkspaces`
+ `workspaces:DescribeWorkspaceSnapshots`
+ `workspaces:RebootWorkspaces`
+ `workspaces:RebuildWorkspaces`
+ `workspaces:RestoreWorkspace`
+ `workspaces:StartWorkspaces`

**Document Steps**
+ aws:executeAwsApi \- Gathers the state of the WorkSpace you specify in the `WorkspaceId` parameter\.
+ aws:assertAwsResourceProperty \- Verifies the state of the WorkSpace is `AVAILABLE`, `ERROR`, `IMPAIRED`, `STOPPED`, or `UNHEALTHY`\.
+ aws:branch \- Branches based on the state of the WorkSpace\.
+ aws:executeAwsApi \- Starts the WorkSpace\.
+ aws:branch \- Branches based on the value you specify for the `Action` parameter\.
+ aws:waitForAwsResourceProperty \- Waits for the WorkSpace status after being started\.
+ aws:waitForAwsResourceProperty \- Waits for the WorkSpace state to change to `AVAILABLE`, `ERROR`, `IMPAIRED`, or `UNHEALTHY` after being started\.
+ aws:executeAwsApi \- Gathers the state of the WorkSpace after being started\.
+ aws:branch \- Branches based on the state of the WorkSpace after being started\.
+ aws:executeAwsApi \- Gathers the available snapshots for restoring or rebuilding the WorkSpace\.
+ aws:branch \- Branches based on the value you specify for the `Reboot` parameter\.
+ aws:executeAwsApi \- Reboots the WorkSpace\.
+ aws:executeAwsApi \- Gathers the state of the WorkSpace after being started\.
+ aws:waitForAwsResourceProperty \- Waits for the state of the WorkSpace to change to `REBOOTING`\.
+ aws:waitForAwsResourceProperty \- Waits for the WorkSpace state to change to `AVAILABLE`, `ERROR`, or `UNHEALTHY` after being rebooted\.
+ aws:executeAwsApi \- Gathers the state of the WorkSpace after being rebooted\.
+ aws:branch \- Branches based on the state of the WorkSpace after rebooting\.
+ aws:branch \- Branches based on the value you specify for the `Restore` parameter\.
+ aws:executeAwsApi \- Restores the WorkSpace\. If the restore fails, the runbook tries to rebuild the WorkSpace\.
+ aws:waitForAwsResourceProperty \- Waits for the state of the WorkSpace to change to `RESTORING`\.
+ aws:waitForAwsResourceProperty \- Waits for the WorkSpace state to change to `AVAILABLE`, `ERROR`, or `UNHEALTHY` after being restored\.
+ aws:executeAwsApi \- Gathers the state of the WorkSpace after being restored\.
+ aws:branch \- Branches based on the state of the WorkSpace after restoring\.
+ aws:branch \- Branches based on the value you specify for the `Rebuild` parameter\.
+ aws:executeAwsApi \- Rebuilds the WorkSpace\.
+ aws:waitForAwsResourceProperty \- Waits for the state of the WorkSpace to change to `REBUILDING`\.
+ aws:waitForAwsResourceProperty \- Waits for the WorkSpace state to change to `AVAILABLE`, `ERROR`, or `UNHEALTHY` after being rebuilt\.
+ aws:executeAwsApi \- Gathers the state of the WorkSpace after being rebuilt\.
+ aws:assertAwsResourceProperty \- Confirms the state of the WorkSpace is `AVAILABLE`\.