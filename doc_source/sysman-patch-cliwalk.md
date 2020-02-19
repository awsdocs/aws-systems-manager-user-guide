# Tutorial: Patch a Server Environment \(Command Line\)<a name="sysman-patch-cliwalk"></a>

The following procedure describes how to patch a server environment by using a custom patch baseline, patch groups, and a maintenance window\.

**Before You Begin**
+ Install or update the SSM Agent on your instances\. To patch Linux instances, your instances must be running SSM Agent version 2\.0\.834\.0 or later\. For more information, see [Update SSM Agent by using Run Command](rc-console.md#rc-console-agentexample)\.
+ Configure roles and permissions for the Maintenance Windows capability\. For more information, see [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)\.
+ Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\. For more information, see [Install or Upgrade the AWS CLI](getting-started-cli.md) or [Install or Upgrade the AWS Tools for PowerShell](getting-started-ps.md)\.

**To configure Patch Manager and patch instances \(Command Line\)**

1. Run the following command to create a patch baseline named `Production-Baseline`\. This patch baseline approves patches for a production environment seven days after they are released\. In addition, the patch baseline is tagged to indicate that it is for a production environment\.
**Note**  
The `OperatingSystem` parameter and `PatchFilters` vary depending on the operating system of the instances the patch baseline applies to\. For more information, see [OperatingSystem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreatePatchBaseline.html#systemsmanager-CreatePatchBaseline-request-OperatingSystem) and [PatchFilter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PatchFilter.html)\.

------
#### [ Linux ]

   ```
   aws ssm create-patch-baseline \
       --name "Production-Baseline" \
       --operating-system "WINDOWS" \
       --tags "Key=Environment,Value=Production" \
       --approval-rules "PatchRules=[{PatchFilterGroup={PatchFilters=[{Key=MSRC_SEVERITY,Values=[Critical,Important]},{Key=CLASSIFICATION,Values=[SecurityUpdates,Updates,UpdateRollups,CriticalUpdates]}]},ApproveAfterDays=7}]" \
       --description "Baseline containing all updates approved for production systems"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-patch-baseline ^
       --name "Production-Baseline" ^
       --operating-system "WINDOWS" ^
       --tags "Key=Environment,Value=Production" ^
       --approval-rules "PatchRules=[{PatchFilterGroup={PatchFilters=[{Key=MSRC_SEVERITY,Values=[Critical,Important]},{Key=CLASSIFICATION,Values=[SecurityUpdates,Updates,UpdateRollups,CriticalUpdates]}]},ApproveAfterDays=7}]" ^
       --description "Baseline containing all updates approved for production systems"
   ```

------
#### [ PowerShell ]

   ```
   $patchApprovalRules = New-Object Amazon.SimpleSystemsManagement.Model.PatchRule
   
   $patchApprovalRulesFilterGroup = New-Object Amazon.SimpleSystemsManagement.Model.PatchFilterGroup
   
   $severityFilter = New-Object Amazon.SimpleSystemsManagement.Model.PatchFilter
   $severityFilter.Key= "MSRC_SEVERITY"
   $severityFilter.Values.Add("Critical")
   $severityFilter.Values.Add("Important")
   
   $classificationFilter = New-Object Amazon.SimpleSystemsManagement.Model.PatchFilter
   $classificationFilter.Key = "CLASSIFICATION"
   $classificationFilter.Values.Add("Updates")
   $classificationFilter.Values.Add("CriticalUpdates")
   
   $patchApprovalRulesFilterGroup.PatchFilters.Add($severityFilter)
   $patchApprovalRulesFilterGroup.PatchFilters.Add($classificationFilter)
   
   $patchApprovalRules.PatchFilterGroup = $patchApprovalRulesFilterGroup
   
   $patchApprovalRules.ApproveAfterDays = 7
   
   New-SSMPatchBaseline `
       -Name "Production-Baseline" `
       -OperatingSystem "WINDOWS" `
       -Tags @{Key="Environment";Value="Production"}`
       -ApprovalRules_PatchRule $patchApprovalRules `
       -Description "Baseline containing all updates approved for production systems"
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
#### [ Linux ]

   ```
   aws ssm register-patch-baseline-for-patch-group `
       --baseline-id pb-0c10e65780EXAMPLE `
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
#### [ PowerShell ]

   ```
   Register-SSMPatchBaselineForPatchGroup `
       -BaselineId pb-0c10e65780EXAMPLE `
       -PatchGroup "Database Servers"
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
#### [ Linux ]

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
#### [ PowerShell ]

   ```
   Register-SSMPatchBaselineForPatchGroup `
       -BaselineId pb-0c10e65780EXAMPLE `
       -PatchGroup "Front-End Servers"
   ```

------

   The system returns information like the following\.

   ```
   {
      "PatchGroup":"Front-End Servers",
      "BaselineId":"pb-0c10e65780EXAMPLE"
   }
   ```

1. Run the following commands to create two maintenance windows for the production servers\. The first window runs every Tuesday at 10 PM\. The second window runs every Saturday at 10 PM\. In addition, the maintenance window is tagged to indicate that it is for a production environment\.

------
#### [ Linux ]

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
#### [ PowerShell ]

   ```
   New-SSMMaintenanceWindow `
       -Name "Production-Tuesdays" `
       -Tags @{Key="Environment";Value="Production"} `
       -Schedule "cron(0 0 22 ? * TUE *)" `
       -Duration 1 `
       -Cutoff 0 `
       -AllowUnassociatedTarget $false
   ```

------

   The system returns information like the following\.

   ```
   {
      "WindowId":"mw-0c66948c711a3b5bd"
   }
   ```

------
#### [ Linux ]

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
#### [ PowerShell ]

   ```
   New-SSMMaintenanceWindow `
       -Name "Production-Saturdays" `
       -Tags @{Key="Environment";Value="Production"} `
       -Schedule "cron(0 0 22 ? * SAT *)" `
       -Duration 2 `    -Cutoff 0 `
       -AllowUnassociatedTarget $false
   ```

------

   The system returns information like the following\.

   ```
   {
      "WindowId":"mw-09e2a75baadd84e85"
   }
   ```

1. Run the following commands to register the `Database` and `Front-End` servers patch groups with their respective maintenance windows\.

------
#### [ Linux ]

   ```
   aws ssm register-target-with-maintenance-window \
       --window-id mw-09e2a75baadd84e85 \
       --targets "Key=tag:Patch Group,Values=Database Servers" \
       --owner-information "Database Servers" \    
       --resource-type "INSTANCE"
   ```

------
#### [ Windows ]

   ```
   aws ssm register-target-with-maintenance-window ^
       --window-id mw-09e2a75baadd84e85 ^
       --targets "Key=tag:Patch Group,Values=Database Servers" ^
       --owner-information "Database Servers" ^
       --resource-type "INSTANCE"
   ```

------
#### [ PowerShell ]

   ```
   Register-SSMTargetWithMaintenanceWindow `
       -WindowId "mw-09e2a75baadd84e85" `
       -Targets @{Key="tag:Patch Group";Values="Database Servers"} `
       -OwnerInformation "Database Servers" `
       -ResourceType "INSTANCE"
   ```

------

   The system returns information like the following\.

   ```
   {
      "WindowTargetId":"557e7b3a-bc2f-48dd-ae05-e282b5b20760"
   }
   ```

------
#### [ Linux ]

   ```
   aws ssm register-target-with-maintenance-window \
   --window-id mw-0c66948c711a3b5bd \
   --targets "Key=tag:Patch Group,Values=Front-End Servers" \
   --owner-information "Front-End Servers" \
   --resource-type "INSTANCE"
   ```

------
#### [ Windows ]

   ```
   aws ssm register-target-with-maintenance-window ^
       --window-id mw-0c66948c711a3b5bd ^
       --targets "Key=tag:Patch Group,Values=Front-End Servers" ^
       --owner-information "Front-End Servers" ^
       --resource-type "INSTANCE"
   ```

------
#### [ PowerShell ]

   ```
   Register-SSMTargetWithMaintenanceWindow `
       -WindowId "mw-0c66948c711a3b5bd" `
       -Targets @{Key="tag:Patch Group";Values="Front-End Servers"} `
       -OwnerInformation "Front-End Servers" `
       -ResourceType "INSTANCE"
   ```

------

   The system returns information like the following\.

   ```
   {
      "WindowTargetId":"faa01c41-1d57-496c-ba77-ff9cadba4b7d"
   }
   ```

1. Run the following commands to register a patch task that installs missing updates on the `Database` and `Front-End` servers during their respective maintenance windows\.

------
#### [ Linux ]

   ```
   aws ssm register-task-with-maintenance-window \
       --window-id mw-09e2a75baadd84e85 \
       --targets "Key=WindowTargetIds,Values=557e7b3a-bc2f-48dd-ae05-e282b5b20760" \
       --task-arn "AWS-RunPatchBaseline" \
       --service-role-arn "arn:aws:iam::12345678:role/MW-Role" \
       --task-type "RUN_COMMAND" \
       --max-concurrency 2 \
       --max-errors 1 \
       --priority 1 \
       --task-parameters '{\"Operation\":{\"Values\":[\"Install\"]}}'
   ```

------
#### [ Windows ]

   ```
   aws ssm register-task-with-maintenance-window ^
       --window-id mw-09e2a75baadd84e85 ^
       --targets "Key=WindowTargetIds,Values=557e7b3a-bc2f-48dd-ae05-e282b5b20760" ^
       --task-arn "AWS-RunPatchBaseline" ^
       --service-role-arn "arn:aws:iam::12345678:role/MW-Role" ^
       --task-type "RUN_COMMAND" ^
       --max-concurrency 2 ^
       --max-errors 1 ^
       --priority 1 ^
       --task-parameters '{\"Operation\":{\"Values\":[\"Install\"]}}'
   ```

------
#### [ PowerShell ]

   ```
   $taskParameters = @{}
   $taskParameterValues = New-Object Amazon.SimpleSystemsManagement.Model.MaintenanceWindowTaskParameterValueExpression
   $taskParameterValues.Values = @("Install")
   $taskParameters.Add("Operation", $taskParameterValues)
   
   Register-SSMTaskWithMaintenanceWindow `
       -WindowId "mw-09e2a75baadd84e85" `
       -Targets @{Key="WindowTargetIds";Values="557e7b3a-bc2f-48dd-ae05-e282b5b20760"} `
       -TaskArn "AWS-RunPatchBaseline" `
       -TaskType "RUN_COMMAND" `
       -MaxConcurrency 2 `
       -MaxErrors 1 `
       -Priority 1 `    
       -TaskParameters $taskParameters
   ```

------

   The system returns information like the following\.

   ```
   {
      "WindowTaskId":"968e3b17-8591-4fb2-932a-b62389d6f635"
   }
   ```

------
#### [ Linux ]

   ```
   aws ssm register-task-with-maintenance-window \
       --window-id mw-09e2a75baadd84e85 \
       --targets "Key=WindowTargetIds,Values=767b6508-f4ac-445e-b6fe-758cc912e55c" \    
       --task-arn "AWS-RunPatchBaseline" \
       --service-role-arn "arn:aws:iam::12345678:role/MW-Role" \
       --task-type "RUN_COMMAND" \
       --max-concurrency 2 \
       --max-errors 1 \
       --priority 1 \
       --task-parameters '{\"Operation\":{\"Values\":[\"Install\"]}}'
   ```

------
#### [ Windows ]

   ```
   aws ssm register-task-with-maintenance-window ^
       --window-id mw-09e2a75baadd84e85 ^
       --targets "Key=WindowTargetIds,Values=767b6508-f4ac-445e-b6fe-758cc912e55c" ^
       --task-arn "AWS-RunPatchBaseline" ^
       --service-role-arn "arn:aws:iam::12345678:role/MW-Role" ^
       --task-type "RUN_COMMAND" ^
       --max-concurrency 2 ^    
       --max-errors 1 ^
       --priority 1 ^
       --task-parameters '{\"Operation\":{\"Values\":[\"Install\"]}}'
   ```

------
#### [ PowerShell ]

   ```
   $taskParameters = @{}
   $taskParameterValues = New-Object Amazon.SimpleSystemsManagement.Model.MaintenanceWindowTaskParameterValueExpression
   $taskParameterValues.Values = @("Install")
   $taskParameters.Add("Operation", $taskParameterValues)
   
   Register-SSMTaskWithMaintenanceWindow `
       -WindowId "mw-0c66948c711a3b5bd" `
       -Targets @{Key="WindowTargetIds";Values="faa01c41-1d57-496c-ba77-ff9cadba4b7d"} `
       -TaskArn "AWS-RunPatchBaseline" `
       -TaskType "RUN_COMMAND" `
       -MaxConcurrency 2 `
       -MaxErrors 1 `
       -Priority 1 `    
       -TaskParameters $taskParameters
   ```

------

   The system returns information like the following\.

   ```
   {
      "WindowTaskId":"09f2e873-a3a7-443f-ba0a-05cf4de5a1c7"
   }
   ```

1. Run the following command to get the high\-level patch compliance summary for a patch group\. The high\-level patch compliance summary includes the number of instances with patches in the respective patch states\.
**Note**  
It is expected to see zeroes for the number of instances in the summary until the patch task runs during the first maintenance window\.

------
#### [ Linux ]

   ```
   aws ssm describe-patch-group-state --patch-group "Database Servers"
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-patch-group-state --patch-group "Database Servers"
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMPatchGroupState -PatchGroup "Database Servers"
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

1. Run the following command to get patch summary states per\-instance for a patch group\. The per\-instance summary includes a number of patches in the respective patch states per instance for a patch group\.

------
#### [ Linux ]

   ```
   aws ssm describe-instance-patch-states-for-patch-group --patch-group "Database Servers"
   ```

------
#### [ Windows ]

   ```
   aws ssm describe-instance-patch-states-for-patch-group --patch-group "Database Servers"
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMInstancePatchStatesForPatchGroup -PatchGroup "Database Servers"
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

For examples of other AWS CLI commands you can use for your Patch Manager configuration tasks, see [AWS CLI Commands for Patch Manager](patch-manager-cli-commands.md)\.