# Walkthrough: Automatically Update SSM Agent \(CLI\)<a name="sysman-state-cli"></a>

The following procedure walks you through the process of creating a State Manager association using the AWS Command Line Interface \(AWS CLI\)\. The association automatically updates the SSM Agent according to a schedule that you specify\. For more information about the SSM Agent, see [Installing and Configuring SSM Agent](ssm-agent.md)\.

To view details about the different versions of SSM Agent, see the [release notes](https://github.com/aws/amazon-ssm-agent/blob/master/RELEASENOTES.md)\.

**Before You Begin**  
Before you complete the following procedure, verify that you have at least one running Amazon EC2 instance \(Linux or Windows\) that is configured for Systems Manager\. For more information, see [Systems Manager Prerequisites](systems-manager-prereqs.md)\. 

**To create an association for automatically updating SSM Agent**

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

1. Execute the following command to create an association by targeting instances using Amazon EC2 tags\. The `Schedule` parameter sets a schedule to run the association every Sunday morning at 2:00 a\.m\. \(UTC\)\.

   ```
   aws ssm create-association --targets Key=tag:TagKey,Values=TagValue --name AWS-UpdateSSMAgent --schedule-expression "cron(0 0 2 ? * SUN *)"
   ```
**Note**  
State Manager associations do not support all cron and rate expressions\. For more information about creating cron and rate expressions for associations, see [Reference: Cron and Rate Expressions for Systems Manager](reference-cron-and-rate-expressions.md)\.

   If you want, you can also target multiple instances by specifying instances IDs in a comma\-separated list\.

   ```
   aws ssm create-association --targets Key=instanceids,Values=InstanceID,InstanceID,InstanceID --name your document name --schedule-expression "cron(0 0 2 ? * SUN *)"
   ```

   The system returns information like the following\.

   ```
   {
       "AssociationDescription": {
           "ScheduleExpression": "cron(0 0 2 ? * SUN *)",
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
   aws ssm  list-associations
   ```
**Note**  
If your instances are currently running the most recent version of the SSM Agent, the status shows `Failed`\. This is expected behavior\. When a new version of SSM Agent is published, the association automatically installs the new agent, and the status shows `Success`\.