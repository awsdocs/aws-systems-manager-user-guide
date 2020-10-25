# Walkthrough: Create a maintenance window to update SSM Agent \(AWS CLI\)<a name="mw-walkthrough-cli"></a>

The following walkthrough shows you how to use the AWS CLI to create an AWS Systems Manager maintenance window\. The walkthrough also describes how to register your managed instances as targets and register a Run Command task to update SSM Agent\.

**Before you begin**  
Before you complete the following procedure, you must either have administrator privileges on the instances you want to configure or you must have been granted the appropriate permissions in AWS Identity and Access Management \(IAM\)\. Additionally, verify that you have at least one running EC2 instance for Linux or Windows Server that is configured for Systems Manager\. For more information, see [Systems Manager prerequisites](systems-manager-prereqs.md)\. 

**Topics**
+ [Step 1: Get started](#mw-walkthrough-cli-settings)
+ [Step 2: Create the maintenance window](#mw-walkthrough-cli-create-mw)
+ [Step 3: Register maintenance window targets \(AWS CLI\)](#mw-walkthrough-cli-targets)
+ [Step 4: Register a Run Command task for the maintenance window to update SSM Agent](#mw-walkthrough-cli-tasks)

## Step 1: Get started<a name="mw-walkthrough-cli-settings"></a>

**To run commands using the AWS CLI**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Verify that an instance is ready to be registered as a target for a maintenance window\.

   Run the following command to view which instances are online\.

   ```
   aws ssm describe-instance-information --query "InstanceInformationList[*]"
   ```

   Run the following command to view details about a particular instance\.

   ```
   aws ssm describe-instance-information --instance-information-filter-list key=InstanceIds,valueSet=instance-id
   ```

## Step 2: Create the maintenance window<a name="mw-walkthrough-cli-create-mw"></a>

Use the following procedure to create a maintenance window and specify its basic options, such as schedule and duration\.

**Create a maintenance window \(AWS CLI\)**

1. Open the AWS CLI and run the following commands to create a maintenance window that runs weekly on Sundays at 02:00, in the United States Pacific time zone, with a one hour cutoff:

------
#### [ Linux ]

   ```
   aws ssm create-maintenance-window \
       --name "My-First-Maintenance-Window" \
       --schedule "cron(0 2 ? * SUN *)" \
       --duration 2 \
       --schedule-timezone "America/Los_Angeles" \
       --cutoff 1 \
       --no-allow-unassociated-targets
   ```

------
#### [ Windows ]

   ```
   aws ssm create-maintenance-window ^
       --name "My-First-Maintenance-Window" ^
       --schedule "cron(0 2 ? * SUN *)" ^
       --duration 2 ^
       --schedule-timezone "America/Los_Angeles" ^
       --cutoff 1 ^
       --no-allow-unassociated-targets
   ```

------

   For information about creating cron expressions for the `schedule` parameter, see [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

   For an explanation of how the various schedule\-related options for maintenance windows relate to one another, see [Reference: Maintenance window scheduling and active period options](maintenance-windows-schedule-options.md)\.

   For more information about working with the `--schedule` option, see [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

   The system returns information like the following:

   ```
   {
      "WindowId":"mw-0c50858d01EXAMPLE"
   }
   ```

1. To list this and any other maintenance windows created in your AWS account in your current AWS Region, run the following command:

   ```
   aws ssm describe-maintenance-windows
   ```

   The system returns information like the following:

   ```
   {
       "WindowIdentities": [
           {
               "Cutoff": 1,
               "Name": "My-First-Maintenance-Window",
               "NextExecutionTime": "2019-02-03T02:00-08:00",
               "Enabled": true,
               "WindowId": "mw-0c50858d01EXAMPLE",
               "Duration": 2
           }
       ]
   }
   ```

## Step 3: Register maintenance window targets \(AWS CLI\)<a name="mw-walkthrough-cli-targets"></a>

Use the following procedure to register a target with your maintenance window created in Step 2\. By registering a target, you specify which instances to update\.

**To register maintenance window targets \(AWS CLI\)**

1. Run the following command:

------
#### [ Linux ]

   ```
   aws ssm register-target-with-maintenance-window \
       --window-id "mw-0c50858d01EXAMPLE" \
       --target "Key=InstanceIds,Values=i-02573cafcfEXAMPLE" \
       --resource-type "INSTANCE"
   ```

------
#### [ Windows ]

   ```
   aws ssm register-target-with-maintenance-window ^
       --window-id "mw-0c50858d01EXAMPLE" ^
       --target "Key=InstanceIds,Values=i-02573cafcfEXAMPLE" ^
       --resource-type "INSTANCE"
   ```

------

   The system returns information like the following, which includes a maintenance window target ID\. Copy or note the WindowTargetId value\. You must specify this ID in the next step to register a task for this maintenance window\.

   ```
   {
      "WindowTargetId":"1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d-1a2"
   }
   ```

**Alternative commands**  
Use the following command to register multiple managed instances:

------
#### [ Linux ]

   ```
   aws ssm register-target-with-maintenance-window \
       --window-id "mw-0c50858d01EXAMPLE" \
       --targets "Key=InstanceIds,Values=i-02573cafcfEXAMPLE,i-0471e04240EXAMPLE" \
       --resource-type "INSTANCE"
   ```

------
#### [ Windows ]

   ```
   aws ssm register-target-with-maintenance-window ^
       --window-id "mw-0c50858d01EXAMPLE" ^
       --targets "Key=InstanceIds,Values=i-02573cafcfEXAMPLE,i-0471e04240EXAMPLE" ^
       --resource-type "INSTANCE"
   ```

------

   Use the following command to register instances by using Amazon EC2 tags\. For example:

------
#### [ Linux ]

   ```
   aws ssm register-target-with-maintenance-window \
       --window-id "mw-0c50858d01EXAMPLE" \
       --targets "Key=tag:Environment,Values=Prod" "Key=tag:Role,Values=Web" \
       --resource-type "INSTANCE"
   ```

------
#### [ Windows ]

   ```
   aws ssm register-target-with-maintenance-window ^
       --window-id "mw-0c50858d01EXAMPLE" ^
       --targets "Key=tag:Environment,Values=Prod" "Key=tag:Role,Values=Web" ^
       --resource-type "INSTANCE"
   ```

------

1. Run the following command to display the targets for a maintenance window:

   ```
   aws ssm describe-maintenance-window-targets --window-id "mw-0c50858d01EXAMPLE"
   ```

   The system returns information like the following:

   ```
   {
       "Targets": [
           {
               "ResourceType": "INSTANCE",
               "WindowId": "mw-0c50858d01EXAMPLE",
               "Targets": [
                   {
                       "Values": [
                           "i-02573cafcfEXAMPLE"
                       ],
                       "Key": "InstanceIds"
                   }
               ],
               "WindowTargetId": "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE"
           },
           {
               "ResourceType": "INSTANCE",
               "WindowId": "mw-0c50858d01EXAMPLE",
               "Targets": [
                   {
                       "Values": [
                           "Prod"
                       ],
                       "Key": "tag:Environment"
                   },
                   {
                       "Values": [
                           "Web"
                       ],
                       "Key": "tag:Role"
                   }
               ],
               "WindowTargetId": "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE"
           }
       ]
   }
   ```

## Step 4: Register a Run Command task for the maintenance window to update SSM Agent<a name="mw-walkthrough-cli-tasks"></a>

Use the following procedure to register a Run Command task for the maintenance window you created in Step 2\. The Run Command task updates SSM Agent on the registered targets\.

**To register a Run Command task for a maintenance window to update SSM Agent \(AWS CLI\)**

1. Run the following command to register a Run Command task for the maintenance window using the WindowTargetId value in Step 3\. The task updates SSM Agent by using the `AWS-UpdateSSMAgent` document\.

------
#### [ Linux ]

   ```
   aws ssm register-task-with-maintenance-window \
       --window-id "mw-0c50858d01EXAMPLE" \
       --task-arn "AWS-UpdateSSMAgent" \
       --name "UpdateSSMAgent" \
       --targets "Key=WindowTargetIds,Values=e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" \
       --service-role-arn "arn:aws:iam::1122334455:role/MW-Role" \
       --task-type "RUN_COMMAND" \
       --max-concurrency 1 --max-errors 1 --priority 10
   ```

------
#### [ Windows ]

   ```
   aws ssm register-task-with-maintenance-window ^
       --window-id "mw-0c50858d01EXAMPLE" ^
       --task-arn "AWS-UpdateSSMAgent" ^
       --name "UpdateSSMAgent" ^
       --targets "Key=WindowTargetIds,Values=e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE" ^
       --service-role-arn "arn:aws:iam::1122334455:role/MW-Role" ^
       --task-type "RUN_COMMAND" ^
       --max-concurrency 1 --max-errors 1 --priority 10
   ```

------
**Note**  
If the targets you registered in the preceding step are Windows Server 2012 R2 or earlier, you must use the `AWS-UpdateEC2Config` document\.

   The system returns information like the following:

   ```
   {
      "WindowTaskId": "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE"
   }
   ```

1. Run the following command to list all registered tasks for a maintenance window\.

   ```
   aws ssm describe-maintenance-window-tasks --window-id "mw-0c50858d01EXAMPLE"
   ```

   The system returns information like the following:

   ```
   {
       "Tasks": [
           {
               "ServiceRoleArn": "arn:aws:iam::111122223333:role/MW-Role",
               "MaxErrors": "1",
               "TaskArn": "AWS-UpdateSSMAgent",
               "MaxConcurrency": "1",
               "WindowTaskId": "4f7ca192-7e9a-40fe-9192-5cb15EXAMPLE",
               "TaskParameters": {},
               "Priority": 10,
               "WindowId": "mw-0c50858d01EXAMPLE",
               "Type": "RUN_COMMAND",
               "Targets": [
                   {
                       "Values": [
                           "e32eecb2-646c-4f4b-8ed1-205fbEXAMPLE"
                       ],
                       "Key": "WindowTargetIds"
                   }
               ],
               "Name": "UpdateSSMAgent"
           }
       ]
   }
   ```