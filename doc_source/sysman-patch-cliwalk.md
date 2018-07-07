# Walkthrough: Patch a Server Environment \(AWS CLI\)<a name="sysman-patch-cliwalk"></a>

The following procedure illustrates how a user might patch a server environment by using a custom patch baseline, patch groups, and a Maintenance Window\.

For a sample of other AWS CLI commands you might use for your Patch Manager configuration tasks, see [AWS CLI Commands for Patch Manager](patch-manager-cli-commands.md)\.

**Before You Begin**

Install or update the SSM Agent on your instances\. To patch Linux instances, your instances must be running SSM Agent version 2\.0\.834\.0 or later\. For information about updating the agent, see the section titled *Example: Update the SSM Agent* in [Running Commands from the Console](rc-console.md)\.

In addition, the following walkthrough runs patching during a Maintenance Window\. You must configure roles and permissions for Maintenance Windows before you begin\. For more information, see [Controlling Access to Maintenance Windows](sysman-maintenance-permissions.md)\. 

**To configure Patch Manager and patch instances by using the AWS CLI**

1. [Download](https://aws.amazon.com/cli/) the latest version of the AWS CLI to your local machine\.

1. Open the AWS CLI and run the following command to specify your credentials and a Region\. You must either have administrator privileges in Amazon EC2, or you must have been granted the appropriate permission in AWS Identity and Access Management \(IAM\)\.

   ```
   aws configure
   ```

   The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region
   Default output format [None]: ENTER
   ```

1. \(Windows\) Execute the following command to create a patch baseline named "Production\-Baseline" that approves patches for a production environment seven days after they are released\.

   ```
   aws ssm create-patch-baseline --name "Production-Baseline" --operating-system "WINDOWS" --product "WindowsServer2012R2" --approval-rules "PatchRules=[{PatchFilterGroup={PatchFilters=[{Key=MSRC_SEVERITY,Values=[Critical,Important]},{Key=CLASSIFICATION,Values=[SecurityUpdates,Updates,UpdateRollups,CriticalUpdates]}]},ApproveAfterDays=7}]" --description "Baseline containing all updates approved for production systems"
   ```

   \(Linux\) Execute the following command to create a patch baseline named "Production\-Baseline" that approves patches for a production environment seven days after they are released, including both security and non\-security patches included in the source repository\.

   ```
   aws ssm create-patch-baseline --name "Production-Baseline" --operating-system "AMAZON_LINUX" --approval-rules  "PatchRules=[{PatchFilterGroup={PatchFilters=[{Key=PRODUCT,Values=[AmazonLinux2016.03,AmazonLinux2016.09,AmazonLinux2017.03,AmazonLinux2017.09]},{Key=SEVERITY,Values=[Critical,Important]},{Key=CLASSIFICATION,Values=[Security]}]},ApproveAfterDays=7,EnableNonSecurity=true}]" --description "Baseline containing all updates approved for production systems"
   ```

   The system returns information like the following\.

   ```
   {
      "BaselineId":"pb-0c10e65780EXAMPLE""
   }
   ```

1. Execute the following commands to register the "Production\-Baseline" patch baseline for three patch groups named "Production," "Database Servers," and "Front\-End Patch Group\."

   ```
   aws ssm register-patch-baseline-for-patch-group --baseline-id pb-0c10e65780EXAMPLE" --patch-group "Production"
   ```

   The system returns information like the following\.

   ```
   {
      "PatchGroup":"Production",
      "BaselineId":"pb-0c10e65780EXAMPLE""
   }
   ```

   ```
   aws ssm register-patch-baseline-for-patch-group --baseline-id pb-0c10e65780EXAMPLE" --patch-group "Database Servers"
   ```

   The system returns information like the following\.

   ```
   {
      "PatchGroup":"Database Servers",
      "BaselineId":"pb-0c10e65780EXAMPLE""
   }
   ```

1. Execute the following commands to create two Maintenance Windows for the production servers\. The first window run every Tuesday at 10 PM\. The second window runs every Saturday at 10 PM\.

   ```
   aws ssm create-maintenance-window --name "Production-Tuesdays" --schedule "cron(0 0 22 ? * TUE *)" --duration 1 --cutoff 0 --no-allow-unassociated-targets
   ```

   The system returns information like the following\.

   ```
   {
      "WindowId":"mw-0c66948c711a3b5bd"
   }
   ```

   ```
   aws ssm create-maintenance-window --name "Production-Saturdays" --schedule "cron(0 0 22 ? * SAT *)" --duration 2 --cutoff 0 --no-allow-unassociated-targets
   ```

   The system returns information like the following\.

   ```
   {
      "WindowId":"mw-09e2a75baadd84e85"
   }
   ```

1. Execute the following commands to register the Production servers with the two production Maintenance Windows\.

   ```
   aws ssm register-target-with-maintenance-window --window-id mw-0c66948c711a3b5bd --targets "Key=tag:Patch Group,Values=Production" --owner-information "Production servers" --resource-type "INSTANCE"
   ```

   The system returns information like the following\.

   ```
   {
      "WindowTargetId":"557e7b3a-bc2f-48dd-ae05-e282b5b20760"
   }
   ```

   ```
   aws ssm register-target-with-maintenance-window --window-id mw-0c66948c711a3b5bd --targets "Key=tag:Patch Group,Values=Database Servers" --owner-information "Database servers" --resource-type "INSTANCE"
   ```

   The system returns information like the following\.

   ```
   {
      "WindowTargetId":"767b6508-f4ac-445e-b6fe-758cc912e55c"
   }
   ```

   ```
   aws ssm register-target-with-maintenance-window --window-id mw-09e2a75baadd84e85 --targets "Key=tag:Patch Group,Values=Production" --owner-information "Production servers" --resource-type "INSTANCE"
   ```

   The system returns information like the following\.

   ```
   {
      "WindowTargetId":"faa01c41-1d57-496c-ba77-ff9cadba4b7d"
   }
   ```

   ```
   aws ssm register-target-with-maintenance-window --window-id mw-09e2a75baadd84e85 --targets "Key=tag:Patch Group,Values=Database Servers" --owner-information "Database servers" --resource-type "INSTANCE"
   ```

   The system returns information like the following\.

   ```
   {
      "WindowTargetId":"673b5840-58a4-42ab-8b80-95749677cb2e"
   }
   ```

1. Execute the following commands to register a patch task that only scans the production servers for missing updates in the first production Maintenance Window\.

   ```
   aws ssm register-task-with-maintenance-window --window-id mw-0c66948c711a3b5bd --targets "Key=WindowTargetIds,Values=557e7b3a-bc2f-48dd-ae05-e282b5b20760" --task-arn "AWS-ApplyPatchBaseline" --service-role-arn "arn:aws:iam::12345678:role/MW-Role" --task-type "RUN_COMMAND" --max-concurrency 2 --max-errors 1 --priority 1 --task-parameters '{\"Operation\":{\"Values\":[\"Scan\"]}}'
   ```

   The system returns information like the following\.

   ```
   {
      "WindowTaskId":"968e3b17-8591-4fb2-932a-b62389d6f635"
   }
   ```

   ```
   aws ssm register-task-with-maintenance-window --window-id mw-0c66948c711a3b5bd --targets "Key=WindowTargetIds,Values=767b6508-f4ac-445e-b6fe-758cc912e55c" --task-arn "AWS-ApplyPatchBaseline" --service-role-arn "arn:aws:iam::12345678:role/MW-Role" --task-type "RUN_COMMAND" --max-concurrency 2 --max-errors 1 --priority 5 --task-parameters '{\"Operation\":{\"Values\":[\"Scan\"]}}'
   ```

   The system returns information like the following\.

   ```
   {
      "WindowTaskId":"09f2e873-a3a7-443f-ba0a-05cf4de5a1c7"
   }
   ```

1. Execute the following commands to register a patch task that installs missing updates on the productions servers in the second Maintenance Window\.

   ```
   aws ssm register-task-with-maintenance-window --window-id mw-09e2a75baadd84e85 --targets "Key=WindowTargetIds,Values=557e7b3a-bc2f-48dd-ae05-e282b5b20760" --task-arn "AWS-ApplyPatchBaseline" --service-role-arn "arn:aws:iam::12345678:role/MW-Role" --task-type "RUN_COMMAND" --max-concurrency 2 --max-errors 1 --priority 1 --task-parameters '{\"Operation\":{\"Values\":[\"Install\"]}}'
   ```

   The system returns information like the following\.

   ```
   {
      "WindowTaskId":"968e3b17-8591-4fb2-932a-b62389d6f635"
   }
   ```

   ```
   aws ssm register-task-with-maintenance-window --window-id mw-09e2a75baadd84e85 --targets "Key=WindowTargetIds,Values=767b6508-f4ac-445e-b6fe-758cc912e55c" --task-arn "AWS-ApplyPatchBaseline" --service-role-arn "arn:aws:iam::12345678:role/MW-Role" --task-type "RUN_COMMAND" --max-concurrency 2 --max-errors 1 --priority 5 --task-parameters '{\"Operation\":{\"Values\":[\"Install\"]}}'
   ```

   The system returns information like the following\.

   ```
   {
      "WindowTaskId":"09f2e873-a3a7-443f-ba0a-05cf4de5a1c7"
   }
   ```

1. Execute the following command to get the high\-level patch compliance summary for a patch group\. The high\-level patch compliance summary gives you the number of instances with patches in the following states for a patch group: "NotApplicable," "Missing," "Failed," "InstalledOther," and "Installed\." 

   ```
   aws ssm describe-patch-group-state --patch-group "Production"
   ```

   The system returns information like the following\.

   ```
   {
      "InstancesWithNotApplicablePatches":0,
      "InstancesWithMissingPatches":0,
      "InstancesWithFailedPatches":1,
      "InstancesWithInstalledOtherPatches":4,
      "Instances":4,
      "InstancesWithInstalledPatches":3
   }
   ```

1. Execute the following command to get patch summary states per\-instance for a patch group\. The per\-instance summary gives you a number of patches in the following states per instance for a patch group: "NotApplicable," "Missing," "Failed," "InstalledOther," and "Installed\."

   ```
   aws ssm describe-instance-patch-states-for-patch-group --patch-group "Production"
   ```

   The system returns information like the following\.

   ```
   {
      "InstancePatchStates":[
         {
            "OperationStartTime":1481259600.0,
            "FailedCount":0,
            "InstanceId":"i-08ee91c0b17045407",
            "OwnerInformation":"",
            "NotApplicableCount":2077,
            "OperationEndTime":1481259757.0,
            "PatchGroup":"Production",
            "InstalledOtherCount":186,
            "MissingCount":7,
            "SnapshotId":"b0e65479-79be-4288-9f88-81c96bc3ed5e",
            "Operation":"Scan",
            "InstalledCount":72
         },
         {
            "OperationStartTime":1481259602.0,
            "FailedCount":0,
            "InstanceId":"i-0fff3aab684d01b23",
            "OwnerInformation":"",
            "NotApplicableCount":2692,
            "OperationEndTime":1481259613.0,
            "PatchGroup":"Production",
            "InstalledOtherCount":3,
            "MissingCount":1,
            "SnapshotId":"b0e65479-79be-4288-9f88-81c96bc3ed5e",
            "Operation":"Scan",
            "InstalledCount":1
         },
         {
            "OperationStartTime":1481259547.0,
            "FailedCount":0,
            "InstanceId":"i-0a00def7faa94f1dc",
            "OwnerInformation":"",
            "NotApplicableCount":1859,
            "OperationEndTime":1481259592.0,
            "PatchGroup":"Production",
            "InstalledOtherCount":116,
            "MissingCount":1,
            "SnapshotId":"b0e65479-79be-4288-9f88-81c96bc3ed5e",
            "Operation":"Scan",
            "InstalledCount":110
         },
         {
            "OperationStartTime":1481259549.0,
            "FailedCount":0,
            "InstanceId":"i-09a618aec652973a9",
            "OwnerInformation":"",
            "NotApplicableCount":1637,
            "OperationEndTime":1481259837.0,
            "PatchGroup":"Production",
            "InstalledOtherCount":388,
            "MissingCount":2,
            "SnapshotId":"b0e65479-79be-4288-9f88-81c96bc3ed5e",
            "Operation":"Scan",
            "InstalledCount":141
         }
      ]
   }
   ```