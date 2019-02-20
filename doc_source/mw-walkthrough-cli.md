# Walkthrough: Create a Maintenance Window to Update SSM Agent \(AWS CLI\)<a name="mw-walkthrough-cli"></a>

The following walkthrough shows you how to use the AWS CLI to create an AWS Systems Manager Maintenance Window\. The walkthrough also describes how to register your managed instances as targets and register a Run Command task to update SSM Agent\.

**Before You Begin**  
Before you complete the following procedure, you must either have administrator privileges on the instances you want to configure or you must have been granted the appropriate permissions in AWS Identity and Access Management \(IAM\)\. Additionally, verify that you have at least one running Amazon EC2 instance \(Linux or Windows\) that is configured for Systems Manager\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\. 

**Topics**
+ [Step 1: Get Started](#mw-walkthrough-cli-settings)
+ [Step 2: Create the Maintenance Window](#mw-walkthrough-cli-create-mw)
+ [Step 3: Register Maintenance Window Targets \(AWS CLI\)](#mw-walkthrough-cli-targets)
+ [Step 4: Register a Run Command Task for the Maintenance Window to Update SSM Agent](#mw-walkthrough-cli-tasks)

## Step 1: Get Started<a name="mw-walkthrough-cli-settings"></a>

**To run commands using the AWS CLI**

1. Run the following command to specify your credentials and the AWS Region:

   ```
   aws configure
   ```

1. The system prompts you to specify the following\.

   ```
   AWS Access Key ID [None]: key_name
   AWS Secret Access Key [None]: key_name
   Default region name [None]: region-id
   Default output format [None]: ENTER
   ```

   *region* represents the Region identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager Table of Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) topic in the *AWS General Reference*\.

1. Verify that an instance is ready to be registered as a target for a Maintenance Window\.

   Run the following command to view which instances are online\.

   ```
   aws ssm describe-instance-information --query "InstanceInformationList[*]"
   ```

   Run the following command to view details about a particular instance\.

   ```
   aws ssm describe-instance-information --instance-information-filter-list key=InstanceIds,valueSet=instance ID
   ```

## Step 2: Create the Maintenance Window<a name="mw-walkthrough-cli-create-mw"></a>

Use the following procedure to create a Maintenance Window and specify its basic options, such as schedule and duration\.

**Create a Maintenance Window \(AWS CLI\)**

1. Open the AWS CLI and run the following commands to create a Maintenance Window that runs weekly on Sundays at 02:00, in the United States Pacific time zone, with a 1 hour cutoff:

   ```
   aws ssm create-maintenance-window --name "My-First-Maintenance-Window" --schedule "cron(0 2 ? * SUN *)" --duration 2 --schedule-timezone "America/Los_Angeles" --cutoff 1 --no-allow-unassociated-targets
   ```

   For information about creating cron expressions for the `schedule` parameter, see [Reference: Cron and Rate Expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

   For an explanation of how the various schedule\-related options for Maintenance Windows relate to one another, see [Reference: Maintenance Window Scheduling and Active Period Options](reference-maintenance-windows-schedule-options.md)\.

   The system returns information like the following:

   ```
   {
      "WindowId":"mw-0c5ed765acEXAMPLE"
   }
   ```

1. To list this and any other Maintenance Windows created in your AWS account in your current AWS Region, run the following command:

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
               "WindowId": "mw-0c5ed765acEXAMPLE",
               "Duration": 2
           }
       ]
   }
   ```

## Step 3: Register Maintenance Window Targets \(AWS CLI\)<a name="mw-walkthrough-cli-targets"></a>

Use the following procedure to register a target with your Maintenance Window created in Step 2\. By registering a target, you specify which instances to update\.

**To register Maintenance Window targets \(AWS CLI\)**

1. Run the following command:

   ```
   aws ssm register-target-with-maintenance-window --window-id "mw-0c5ed765acEXAMPLE" --target "Key=InstanceIds,Values=i-1234567890EXAMPLE" --resource-type "INSTANCE"
   ```

   The system returns information like the following, which includes a Maintenance Window target ID\. Copy or note the WindowTargetId value\. You must specify this ID in the next step to register a task for this Maintenance Window\.

   ```
   {
      "WindowTargetId":"1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d-1a2"
   }
   ```

**Alternative commands**  
Use the following command to register multiple managed instances:

   ```
   aws ssm register-target-with-maintenance-window --window-id "mw-0c5ed765acEXAMPLE" --targets "Key=InstanceIds,Values=i-1234567890EXAMPLE,i-abcdefghiEXAMPLE" --resource-type "INSTANCE"
   ```

   Use the following command to register instances by using Amazon EC2 tags\. For example:

   ```
   aws ssm register-target-with-maintenance-window --window-id "mw-0c5ed765acEXAMPLE" --targets "Key=tag:Environment,Values=Prod" "Key=tag:Role,Values=Web" --resource-type "INSTANCE"
   ```

1. Run the following command to display the targets for a Maintenance Window:

   ```
   aws ssm describe-maintenance-window-targets --window-id "mw-0c5ed765acEXAMPLE"
   ```

   The system returns information like the following:

   ```
   {
       "Targets": [
           {
               "ResourceType": "INSTANCE",
               "WindowId": "mw-0c5ed765acEXAMPLE",
               "Targets": [
                   {
                       "Values": [
                           "i-1234567890EXAMPLE"
                       ],
                       "Key": "InstanceIds"
                   }
               ],
               "WindowTargetId": "d6k2n5j2-mw84-tg4d-r3g4-1d4d1EXAMPLE"
           },
           {
               "ResourceType": "INSTANCE",
               "WindowId": "mw-0c5ed765acEXAMPLE",
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
               "WindowTargetId": "d6k2n5j2-mw84-tg4d-r3g4-1d4d1EXAMPLE"
           }
       ]
   }
   ```

## Step 4: Register a Run Command Task for the Maintenance Window to Update SSM Agent<a name="mw-walkthrough-cli-tasks"></a>

Use the following procedure to register a Run Command task for the Maintenance Window you created in Step 2\. The Run Command task updates SSM Agent on the registered targets\.

**To register a Run Command task for a Maintenance Window to update SSM Agent \(AWS CLI\)**

1. Run the following command to register a Run Command task for the Maintenance Window using the WindowTargetId value in Step 3\. The task updates SSM Agent by using the `AWS-UpdateSSMAgent` document\.

   ```
   aws ssm register-task-with-maintenance-window --window-id "mw-0c5ed765acEXAMPLE" --task-arn "AWS-UpdateSSMAgent" --name "UpdateSSMAgent" --targets "Key=WindowTargetIds,Values=d6k2n5j2-mw84-tg4d-r3g4-1d4d1EXAMPLE" --service-role-arn "arn:aws:iam::1122334455:role/MW-Role" --task-type "RUN_COMMAND" --max-concurrency 1 --max-errors 1 --priority 10
   ```
**Note**  
If the targets you registered in the preceding step are Windows Server 2012 R2 or earlier, you must use the `AWS-UpdateEC2Config` document\.

   The system returns information like the following:

   ```
   {
      "WindowTaskId": "j2l8d5b5c-mw66-tk4d-r3g9-1d4d1EXAMPLE"
   }
   ```

1. Run the following command to list all registered tasks for a Maintenance Window\.

   ```
   aws ssm describe-maintenance-window-tasks --window-id "mw-0c5ed765acEXAMPLE"
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
               "WindowTaskId": "j2l8d5b5c-mw66-tk4d-r3g9-1d4d1EXAMPLE",
               "TaskParameters": {},
               "Priority": 10,
               "WindowId": "mw-0c5ed765acEXAMPLE",
               "Type": "RUN_COMMAND",
               "Targets": [
                   {
                       "Values": [
                           "d6k2n5j2-mw84-tg4d-r3g4-1d4d1EXAMPLE"
                       ],
                       "Key": "WindowTargetIds"
                   }
               ],
               "Name": "UpdateSSMAgent"
           }
       ]
   }
   ```