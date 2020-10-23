# Automatically update SSM Agent \(CLI\)<a name="sysman-state-cli"></a>

The following procedure walks you through the process of creating a State Manager association using the AWS Command Line Interface \(AWS CLI\)\. The association automatically updates the SSM Agent according to a schedule that you specify\. For more information about the SSM Agent, see [Working with SSM Agent](ssm-agent.md)\.

**Note**  
Note the following details about automatically updating SSM Agent:  
Beginning September 21, 2020, auto\-update installs SSM Agent version 3\.0\. For more information, see [SSM Agent version 3](ssm-agent-v3.md)\.
To be notified about SSM Agent updates, subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md) page on GitHub\.

**Before you begin**  
Before you complete the following procedure, verify that you have at least one running EC2 instance for Linux or Windows Server that is configured for Systems Manager\. For more information, see [Systems Manager prerequisites](systems-manager-prereqs.md)\. 

**Note**  
If you create an association by using either the AWS CLI or AWS Tools for Windows PowerShell, use the `--Targets` parameter to target instances, as shown in the following example\. Don't use the `--InstanceID` parameter\. The `--InstanceID` parameter is a legacy parameter\.

**To create an association for automatically updating SSM Agent**

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to create an association by targeting instances using Amazon EC2 tags\. The `Schedule` parameter sets a schedule to run the association every Sunday morning at 2:00 a\.m\. \(UTC\)\.

------
#### [ Linux ]

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
**Note**  
State Manager associations do not support all cron and rate expressions\. For more information about creating cron and rate expressions for associations, see [Reference: Cron and rate expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

   If you want, you can also target multiple instances by specifying instances IDs in a comma\-separated list\.

------
#### [ Linux ]

   ```
   aws ssm create-association \
   --targets Key=instanceids,Values=instance_ID,instance_ID,instance_ID \
   --name document_name \
   --schedule-expression "cron(0 2 ? * SUN *)"
   ```

------
#### [ Windows ]

   ```
   aws ssm create-association ^
   --targets Key=instanceids,Values=instance_ID,instance_ID,instance_ID ^
   --name document_name ^
   --schedule-expression "cron(0 2 ? * SUN *)"
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

   The system attempts to create the association on the instance\(s\) and immediately apply the state\. The association status shows `Pending`\.

1. Run the following command to view an updated status of the association you just created\. 

   ```
   aws ssm list-associations
   ```
**Note**  
If your instances *aren't* running the most recent version of the SSM Agent, the status shows `Failed`\. This is expected behavior\. When a new version of SSM Agent is published, the association automatically installs the new agent, and the status shows `Success`\.