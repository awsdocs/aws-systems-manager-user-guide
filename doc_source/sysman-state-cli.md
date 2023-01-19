# Walkthrough: Automatically update SSM Agent \(CLI\)<a name="sysman-state-cli"></a>

The following procedure walks you through the process of creating a State Manager association using the AWS Command Line Interface\. The association automatically updates the SSM Agent according to a schedule that you specify\. For more information about SSM Agent, see [Working with SSM Agent](ssm-agent.md)\. To customize the update schedule for SSM Agent using the console, see [Automatically updating SSM Agent](ssm-agent-automatic-updates.md#ssm-agent-automatic-updates-console)\.

To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

**Before you begin**  
Before you complete the following procedure, verify that you have at least one running Amazon Elastic Compute Cloud \(Amazon EC2\) instance for Linux, macOS, or Windows Server that is configured for Systems Manager\. For more information, see [Systems Manager prerequisites](systems-manager-prereqs.md)\. 

If you create an association by using either the AWS CLI or AWS Tools for Windows PowerShell, use the `--Targets` parameter to target instances, as shown in the following example\. Don't use the `--InstanceID` parameter\. The `--InstanceID` parameter is a legacy parameter\.

**To create an association for automatically updating SSM Agent**

1. Install and configure the AWS Command Line Interface \(AWS CLI\), if you haven't already\.

   For information, see [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)\.

1. Run the following command to create an association by targeting instances using Amazon Elastic Compute Cloud \(Amazon EC2\) tags\. Replace each *example resource placeholder* with your own information\. The `Schedule` parameter sets a schedule to run the association every Sunday morning at 2:00 a\.m\. \(UTC\)\.

   State Manager associations don't support all cron and rate expressions\. For more information about creating cron and rate expressions for associations, see [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

------
#### [ Linux & macOS ]

   ```
   aws ssm create-association \
   --targets Key=tag:tag_key,Values=tag_value \
   --name AWS-UpdateSSMAgent \
   --schedule-expression "cron(0 2 ? * SUN *)"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
   --targets Key=tag:tag_key,Values=tag_value ^
   --name AWS-UpdateSSMAgent ^
   --schedule-expression "cron(0 2 ? * SUN *)"
   ```

------

   You can target multiple instances by specifying instances IDs in a comma\-separated list\.

------
#### [ Linux & macOS ]

   ```
   aws ssm create-association \
   --targets Key=instanceids,Values=instance_ID,instance_ID,instance_ID \
   --name AWS-UpdateSSMAgent \
   --schedule-expression "cron(0 2 ? * SUN *)"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
   --targets Key=instanceids,Values=instance_ID,instance_ID,instance_ID ^
   --name AWS-UpdateSSMAgent ^
   --schedule-expression "cron(0 2 ? * SUN *)"
   ```

------

   You can specify the version of the SSM Agent you want to update to\.

------
#### [ Linux & macOS ]

   ```
   aws ssm create-association \
   --targets Key=instanceids,Values=instance_ID,instance_ID,instance_ID \
   --name AWS-UpdateSSMAgent \
   --schedule-expression "cron(0 2 ? * SUN *)" \
   --parameters version=ssm_agent_version_number
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
   --targets Key=instanceids,Values=instance_ID,instance_ID,instance_ID ^
   --name AWS-UpdateSSMAgent ^
   --schedule-expression "cron(0 2 ? * SUN *)" ^
   --parameters version=ssm_agent_version_number
   ```

------

   The system returns information like the following\.

   ```
   {
       "AssociationDescription": {
           "ScheduleExpression": "cron(0 2 ? * SUN *)",
           "Name": "AWS-UpdateSSMAgent",
           "Overview": {
               "Status": "Pending",
               "DetailedStatus": "Creating"
           },
           "AssociationId": "123..............",
           "DocumentVersion": "$DEFAULT",
           "LastUpdateAssociationDate": 1504034257.98,
           "Date": 1504034257.98,
           "AssociationVersion": "1",
           "Targets": [
               {
                   "Values": [
                       "TagValue"
                   ],
                   "Key": "tag:TagKey"
               }
           ]
       }
   }
   ```

   The system attempts to create the association on the instance\(s\) and applies the state following creation\. The association status shows `Pending`\.

1. Run the following command to view an updated status of the association you created\. 

   ```
   aws ssm list-associations
   ```

   If your instances *aren't* running the most recent version of the SSM Agent, the status shows `Failed`\. When a new version of SSM Agent is published, the association automatically installs the new agent, and the status shows `Success`\.