# Tutorial: Patch a server environment \(AWS CLI\)<a name="patch-manager-patch-servers-using-the-aws-cli"></a>

The following procedure describes how to patch a server environment by using a custom patch baseline, patch groups, and a maintenance window\.

**Before you begin**
+ Install or update the SSM Agent on your managed nodes\. To patch Linux managed nodes, your nodes must be running SSM Agent version 2\.0\.834\.0 or later\. For more information, see [Updating the SSM Agent using Run Command](run-command-tutorial-update-software.md#rc-console-agentexample)\.
+ Configure roles and permissions for Maintenance Windows, a capability of AWS Systems Manager\. For more information, see [Setting up Maintenance Windows](sysman-maintenance-permissions.md)\.
+ Install and configure the AWS Command Line Interface \(AWS CLI\), if you haven't already\.

  For information, see [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)\.

**To configure Patch Manager and patch managed nodes \(command line\)**

1. Run the following command to create a patch baseline for Windows named `Production-Baseline`\. This patch baseline approves patches for a production environment 7 days after they're released or last updated\. That is, we have tagged the patch baseline to indicate that it's for a production environment\.
**Note**  
The `OperatingSystem` parameter and `PatchFilters` vary depending on the operating system of the target managed nodes the patch baseline applies to\. For more information, see [OperatingSystem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-OperatingSystem) and [PatchFilter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PatchFilter.html)\.

------
#### [ Linux & macOS ]

   ```
   aws ssm create-patch-baseline \
       --name "Production-Baseline" \
       --operating-system "WINDOWS" \
       --tags "Key=Environment,Value=Production" \
       --approval-rules "PatchRules=[{PatchFilterGroup={PatchFilters=[{Key=MSRC_SEVERITY,Values=[Critical,Important]},{Key=CLASSIFICATION,Values=[SecurityUpdates,Updates,ServicePacks,UpdateRollups,CriticalUpdates]}]},ApproveAfterDays=7}]" \
       --description "Baseline containing all updates approved for production systems"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-patch-baseline ^
       --name "Production-Baseline" ^
       --operating-system "WINDOWS" ^
       --tags "Key=Environment,Value=Production" ^
       --approval-rules "PatchRules=[{PatchFilterGroup={PatchFilters=[{Key=MSRC_SEVERITY,Values=[Critical,Important]},{Key=CLASSIFICATION,Values=[SecurityUpdates,Updates,ServicePacks,UpdateRollups,CriticalUpdates]}]},ApproveAfterDays=7}]" ^
       --description "Baseline containing all updates approved for production systems"
   ```

------

   The system returns information like the following\.

   ```
   {
      "BaselineId":"pb-0c10e65780EXAMPLE"
   }
   ```

1. Run the following commands to register the "Production\-Baseline" patch baseline for two patch groups\. The groups are named "Database Servers" and "Front\-End Servers"\.

------
#### [ Linux & macOS ]

   ```
   aws ssm register-patch-baseline-for-patch-group \
       --baseline-id pb-0c10e65780EXAMPLE \
       --patch-group "Database Servers"
   ```

------
#### [ Windows ]

   ```
   aws ssm register-patch-baseline-for-patch-group ^
       --baseline-id pb-0c10e65780EXAMPLE ^
       --patch-group "Database Servers"
   ```

------

   The system returns information like the following\.

   ```
   {
      "PatchGroup":"Database Servers",
      "BaselineId":"pb-0c10e65780EXAMPLE"
   }
   ```

------
#### [ Linux & macOS ]

   ```
   aws ssm register-patch-baseline-for-patch-group \
       --baseline-id pb-0c10e65780EXAMPLE \
       --patch-group "Front-End Servers"
   ```

------
#### [ Windows ]

   ```
   aws ssm register-patch-baseline-for-patch-group ^
       --baseline-id pb-0c10e65780EXAMPLE ^
       --patch-group "Front-End Servers"
   ```

------

   The system returns information like the following\.

   ```
   {
      "PatchGroup":"Front-End Servers",
      "BaselineId":"pb-0c10e65780EXAMPLE"
   }
   ```

1. Run the following commands to create two maintenance windows for the production servers\. The first window runs every Tuesday at 10 PM\. The second window runs every Saturday at 10 PM\. In addition, the maintenance window is tagged to indicate that it's for a production environment\.

------
#### [ Linux & macOS ]

   ```
   aws ssm create-maintenance-window \
       --name "Production-Tuesdays" \
       --tags "Key=Environment,Value=Production" \
       --schedule "cron(0 0 22 ? * TUE *)" \
       --duration 1 \
       --cutoff 0 \
       --no-allow-unassociated-targets
   ```

------
#### [ Windows ]

   ```
   aws ssm create-maintenance-window ^
       --name "Production-Tuesdays" ^
       --tags "Key=Environment,Value=Production" ^
       --schedule "cron(0 0 22 ? * TUE *)" ^
       --duration 1 ^
       --cutoff 0 ^
       --no-allow-unassociated-targets
   ```

------

   The system returns information like the following\.

   ```
   {
      "WindowId":"mw-0c50858d01EXAMPLE"
   }
   ```

------
#### [ Linux & macOS ]

   ```
   aws ssm create-maintenance-window \
       --name "Production-Saturdays" \
       --tags "Key=Environment,Value=Production" \
       --schedule "cron(0 0 22 ? * SAT *)" \
       --duration 2 \
       --cutoff 0 \
       --no-allow-unassociated-targets
   ```

------
#### [ Windows ]

   ```
   aws ssm create-maintenance-window ^
       --name "Production-Saturdays" ^
       --tags "Key=Environment,Value=Production" ^
       --schedule "cron(0 0 22 ? * SAT *)" ^
       --duration 2 ^
       --cutoff 0 ^
       --no-allow-unassociated-targets
   ```

------

   The system returns information like the following\.

   ```
   {
      "WindowId":"mw-9a8b7c6d5eEXAMPLE"
   }
   ```

1. Run the following commands to register the `Database` and `Front-End` servers patch groups with their respective maintenance windows\.

------
#### [ Linux & macOS ]

   ```
   aws ssm register-target-with-maintenance-window \
       --window-id mw-0c50858d01EXAMPLE \
       --targets "Key=tag:PatchGroup,Values=Database Servers" \
       --owner-information "Database Servers" \
       --resource-type "INSTANCE"
   ```

------
#### [ Windows ]

   ```
   aws ssm register-target-with-maintenance-window ^
       --window-id mw-0c50858d01EXAMPLE ^
       --targets "Key=tag:PatchGroup,Values=Database Servers" ^
       --owner-information "Database Servers" ^
       --resource-type "INSTANCE"
   ```

------

   The system returns information like the following\.

   ```
   {
      "WindowTargetId":"e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE"
   }
   ```

------
#### [ Linux & macOS ]

   ```
   aws ssm register-target-with-maintenance-window \
   --window-id mw-9a8b7c6d5eEXAMPLE \
   --targets "Key=tag:PatchGroup,Values=Front-End Servers" \
   --owner-information "Front-End Servers" \
   --resource-type "INSTANCE"
   ```

------
#### [ Windows ]

   ```
   aws ssm register-target-with-maintenance-window ^
       --window-id mw-9a8b7c6d5eEXAMPLE ^
       --targets "Key=tag:PatchGroup,Values=Front-End Servers" ^
       --owner-information "Front-End Servers" ^
       --resource-type "INSTANCE"
   ```

------

   The system returns information like the following\.

   ```
   {
      "WindowTargetId":"faa01c41-1d57-496c-ba77-ff9caEXAMPLE"
   }
   ```

1. Run the following commands to register a patch task that installs missing updates on the `Database` and `Front-End` servers during their respective maintenance windows\.

------
#### [ Linux & macOS ]

   ```
   aws ssm register-task-with-maintenance-window \
       --window-id mw-0c50858d01EXAMPLE \
       --targets "Key=WindowTargetIds,Values=e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" \
       --task-arn "AWS-RunPatchBaseline" \
       --service-role-arn "arn:aws:iam::123456789012:role/MW-Role" \
       --task-type "RUN_COMMAND" \
       --max-concurrency 2 \
       --max-errors 1 \
       --priority 1 \
       --task-invocation-parameters "RunCommand={Parameters={Operation=Install}}"
   ```

------
#### [ Windows ]

   ```
   aws ssm register-task-with-maintenance-window ^
       --window-id mw-0c50858d01EXAMPLE ^
       --targets "Key=WindowTargetIds,Values=e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" ^
       --task-arn "AWS-RunPatchBaseline" ^
       --service-role-arn "arn:aws:iam::123456789012:role/MW-Role" ^
       --task-type "RUN_COMMAND" ^
       --max-concurrency 2 ^
       --max-errors 1 ^
       --priority 1 ^
       --task-invocation-parameters "RunCommand={Parameters={Operation=Install}}"
   ```

------

   The system returns information like the following\.

   ```
   {
      "WindowTaskId":"4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE"
   }
   ```

------
#### [ Linux & macOS ]

   ```
   aws ssm register-task-with-maintenance-window \
       --window-id mw-9a8b7c6d5eEXAMPLE \
       --targets "Key=WindowTargetIds,Values=faa01c41-1d57-496c-ba77-ff9caEXAMPLE" \
       --task-arn "AWS-RunPatchBaseline" \
       --service-role-arn "arn:aws:iam::123456789012:role/MW-Role" \
       --task-type "RUN_COMMAND" \
       --max-concurrency 2 \
       --max-errors 1 \
       --priority 1 \
       --task-invocation-parameters "RunCommand={Parameters={Operation=Install}}"
   ```

------
#### [ Windows ]

   ```
   aws ssm register-task-with-maintenance-window ^
       --window-id mw-9a8b7c6d5eEXAMPLE ^
       --targets "Key=WindowTargetIds,Values=faa01c41-1d57-496c-ba77-ff9caEXAMPLE" ^
       --task-arn "AWS-RunPatchBaseline" ^
       --service-role-arn "arn:aws:iam::123456789012:role/MW-Role" ^
       --task-type "RUN_COMMAND" ^
       --max-concurrency 2 ^
       --max-errors 1 ^
       --priority 1 ^
       --task-invocation-parameters "RunCommand={Parameters={Operation=Install}}"
   ```

------

   The system returns information like the following\.

   ```
   {
      "WindowTaskId":"8a5c4629-31b0-4edd-8aea-33698EXAMPLE"
   }
   ```

1. Run the following command to get the high\-level patch compliance summary for a patch group\. The high\-level patch compliance summary includes the number of managed nodes with patches in the respective patch states\.
**Note**  
It's expected to see zeroes for the number of managed nodes in the summary until the patch task runs during the first maintenance window\.

------
#### [ Linux & macOS ]

   ```
   aws ssm describe-patch-group-state \
       --patch-group "Database Servers"
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-patch-group-state ^
       --patch-group "Database Servers"
   ```

------

   The system returns information like the following\.

   ```
   {
      "Instances": number,
      "InstancesWithFailedPatches": number,
      "InstancesWithInstalledOtherPatches": number,
      "InstancesWithInstalledPatches": number,
      "InstancesWithInstalledPendingRebootPatches": number,
      "InstancesWithInstalledRejectedPatches": number,
      "InstancesWithMissingPatches": number,
      "InstancesWithNotApplicablePatches": number,
      "InstancesWithUnreportedNotApplicablePatches": number
   }
   ```

1. Run the following command to get patch summary states per\-managed node for a patch group\. The per\-managed node summary includes a number of patches in the respective patch states per managed node for a patch group\.

------
#### [ Linux & macOS ]

   ```
   aws ssm describe-instance-patch-states-for-patch-group \
       --patch-group "Database Servers"
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-instance-patch-states-for-patch-group ^
       --patch-group "Database Servers"
   ```

------

   The system returns information like the following\.

   ```
   {
      "InstancePatchStates": [ 
         { 
            "BaselineId": "string",
            "FailedCount": number,
            "InstalledCount": number,
            "InstalledOtherCount": number,
            "InstalledPendingRebootCount": number,
            "InstalledRejectedCount": number,
            "InstallOverrideList": "string",
            "InstanceId": "string",
            "LastNoRebootInstallOperationTime": number,
            "MissingCount": number,
            "NotApplicableCount": number,
            "Operation": "string",
            "OperationEndTime": number,
            "OperationStartTime": number,
            "OwnerInformation": "string",
            "PatchGroup": "string",
            "RebootOption": "string",
            "SnapshotId": "string",
            "UnreportedNotApplicableCount": number
         }
      ]
   }
   ```

For examples of other AWS CLI commands you can use for your Patch Manager configuration tasks, see [Working with Patch Manager \(AWS CLI\)](patch-manager-cli-commands.md)\.