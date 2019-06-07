# Creating OpsItems<a name="OpsCenter-creating-OpsItems"></a>

You can create OpsItems automatically or manually\. To automatically create OpsItems, you can choose a bulk option that configures multiple services in Amazon CloudWatch Events to create OpsItems for common resource issues\. Or, if you prefer, you can selectively configure **SSM OpsItems** as the target of specific events in CloudWatch Events\.

This section includes the following topics\.
+ [Enabling the Default CloudWatch Events Rules for Automatically Creating OpsItems](#OpsCenter-automatically-create-OpsItems-1)
+ [Configuring CloudWatch Events to Automatically Create OpsItems for Specific Events](#OpsCenter-automatically-create-OpsItems-2)
+ [Creating OpsItems Manually](#OpsCenter-manually-create-OpsItems)

## Enabling the Default CloudWatch Events Rules for Automatically Creating OpsItems<a name="OpsCenter-automatically-create-OpsItems-1"></a>

This section describes how to configure the default CloudWatch Events rules for automatically creating OpsItems in response to the following events from AWS services\. Enabling these default rules is optional, but recommended\. If you don't want CloudWatch Events to create OpsItems for these events, then you can specify OpsCenter as the target of specific CloudWatch Events events\. For more information, see [Configuring CloudWatch Events to Automatically Create OpsItems for Specific Events](#OpsCenter-automatically-create-OpsItems-2)\.


****  

| AWS Service | Event | 
| --- | --- | 
|  Amazon EC2  |  `Instance State Change (Stopped, Terminated)`  | 
|  Amazon EC2  |  `SSM Maintenance Window Execution (Failed, Timed Out`  | 
|  Amazon EBS  |  `Snapshot Notification (Copy, Create, Share, Failed)`  | 
|  Amazon EC2 Auto Scaling  |  `EC2 Instance (Launch Unsuccessful, Termination Unsuccessful`  | 
|  AWS Health  |  `Amazon Relational Database Service (RDS) Maintenance Scheduled`  | 
|  AWS Health  |  `RDS Issue Notification` \(example: `RDS Connectivity Issue`\)  | 
|  AWS Health  |  `EC2 Maintenance Scheduled`  | 
|  AWS Health  |  `EC2 Issue Notification` \(example: `Instance Auto Recovery Failure`\)  | 
|  AWS Health  |  `EBS Issue Notification` \(example: `Degraded EBS Volume Performance`\)  | 

**Note**  
In the following procedure, you must specify an IAM role with a permissions policy that enables CloudWatch Events to create OpsItems in OpsCenter\. For information about how to create this role, see [Getting Started with OpsCenter](OpsCenter-getting-started.md)\. 

**To configure CloudWatch Events to automatically create OpsItems from multiple AWS services**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose **Configure sources**\.

1. Under **Step 2: OpsItem source setup**, for **IAM Role**, paste the ARN of the IAM role ARN that enables CloudWatch Events to create OpsItems in OpsCenter\.

1. Choose **Set up**\.

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Rules**\. Verify that the list includes new rules that begin with **SSMOpsCenter\-\***, such as **SSMOpsCenter\-AutoScaling\-Instance\-Launch\-Failure** and **SSMOpsCenter\-RDS\-Issue**\. 
**Note**  
If you want, you can edit the new rules in CloudWatch\.

After an OpsItem is created from an event, you can view the event details by opening the OpsItem and scrolling down to the **Operational data** section\.

![\[Viewing operational data to view event details.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_scenario_2.png)

For information about how to configure the options in an OpsItem, see [Working with OpsItems](OpsCenter-working-with-OpsItems.md)\.

## Configuring CloudWatch Events to Automatically Create OpsItems for Specific Events<a name="OpsCenter-automatically-create-OpsItems-2"></a>

Use the following procedure to configure **SSM OpsItems** as the target of a CloudWatch event\. When CloudWatch receives the event, it creates a new OpsItem\.

**To configure OpsCenter as a target of a CloudWatch event**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Events**, and then either choose to create a new rule or edit an existing rule\.

1. After specifying or verifying the details of the rule, choose **Add target**\.

1. In the **Select target type** list, choose **SSM OpsItem**\. 

1. For **Configure input**, verify that **Matched event** is selected\.

1. In the permissions section, choose **Create a new role for this specific resource** to create a new role with the required permissions\. Or, choose **Use existing role** and choose the IAM role you created that gives CloudWatch permission to create OpsItems in OpsCenter\. For more information about the required role and permissions, see [Getting Started with OpsCenter](OpsCenter-getting-started.md)\.

1. Choose **Configure details**\. CloudWatch opens the **Step 2: Configure rule details** page\.

1. For **Name**, type a descriptive name that identifies the purpose of this rule\.

1. For **Description**, type information about this rule\.

1. Verify that the **Enabled** option is selected, and then choose **Create rule**\.

1. In the navigation pane, choose **Rules**\. Verify that the list includes a new rule that begins with AwsSSMOpsCenter\-\*, such as AwsSSMOpsCenter\-EC2 or AwsSSMOpsCenter\-DynamoDB\.

After an OpsItem is created from an event, you can view the event details by opening the OpsItem and scrolling down to the **Private operational data** section\.

![\[Viewing operational data to view event details.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_scenario_2.png)

For information about how to configure the options in an OpsItem, see [Working with OpsItems](OpsCenter-working-with-OpsItems.md)\.

## Creating OpsItems Manually<a name="OpsCenter-manually-create-OpsItems"></a>

This section includes procedures for manually create OpsItems for issues that aren't automatically created by Amazon CloudWatch Events\.

**Before You Begin**  
If you manually create an OpsItem for an impacted AWS resource, then collect information about that resource so that you can create an Amazon Resource Name \(ARN\)\. If you specify an ARN when you create an OpsItem, then OpsCenter automatically creates a deep link to detailed information about the resource\. For example, if you specify the ARN of an impacted EC2 instance, then OpsCenter creates a deep link to the details about that instance\. For information about how to create an ARN, see the [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *Amazon Web Services General Reference*\.

**Note**  
OpsCenter does not support creating deep links for all ARN types\. To view a list of resources the support deep links based on ARNs, see [Supported Resources Reference](OpsCenter-related-resources-reference.md)\.

This section includes the following procedures\.
+ [To manually create an OpsItem \(console\)](#OpsCenter-manually-create-OpsItems-console)
+ [To manually create an OpsItem \(AWS CLI\)](#OpsCenter-manually-create-OpsItems-cli)<a name="OpsCenter-manually-create-OpsItems-console"></a>

**To manually create an OpsItem \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose **Create OpsItem**\. If you don't see this button, then choose the **OpsItems** tab, and then choose **Create OpsItem**\.

1. For **Title**, enter a descriptive name to help you understand the purpose of the OpsItem\.

1. For **Source**, enter the type of impacted AWS resource or other source information to help users understand the origin of the OpsItem\.
**Note**  
You can't edit the **Source** field after you create the OpsItem\.

1. For **Priority**, choose the priority level\.

1. For **Description**, enter information about this OpsItem including \(if applicable\) steps for reproducing the issue\. 

1. For **Deduplication string**, enter words the system should use to check for duplicate OpsItems\. For more information about deduplication strings, see [Reducing Duplicate OpsItems](OpsCenter-working-with-OpsItems.md#OpsCenter-working-deduplication)\. 

1. \(Optional\) For **Notifications**, specify the SNS topic ARN where you want notifications sent when this OpsItem is updated\. You must specify an Amazon SNS ARN that is in the same AWS Region as the OpsItem\.

1. \(Optional\) Under **Related resources**, choose **Add** to specify the ARN of the impacted resource and any related resources\.

1. Choose **Create OpsItem**\.

If successful, the OpsItem opens\. For information about how to configure the options in an OpsItem, see [Working with OpsItems](OpsCenter-working-with-OpsItems.md)\.<a name="OpsCenter-manually-create-OpsItems-cli"></a>

**To manually create an OpsItem \(AWS CLI\)**

1. Open the AWS CLI and run the following command to create an OpsItem\.

   ```
   aws ssm create-ops-item --title "Descriptive_title" --description "Information_about_the_issue" --priority Number_between_1_and_5 --source Source_of_the_issue --operational-data Up_to_20_KB_of_data_or_path_to_JSON_file --notifications Arn="SNS_ARN_in_same_Region" --tags "Key=key_name,Value=a_value"
   ```

   Here are some examples\.

   **Linux**

   ```
   aws ssm create-ops-item --title "EC2 instance disk full" --description "Log clean up may have failed which caused the disk to be full" --priority 2 --source ec2 --operational-data '{"EC2":{"Value":"12345","Type":"SearchableString"}}' --notifications Arn="arn:aws:sns:us-west-1:12345678:TestUser1" --tags "Key=EC2,Value=ProductionServers"
   ```

   The following command uses the `/aws/resources` key in OperationalData to create an OpsItem with an Amazon DynamoDB related resource\.

   ```
   aws ssm create-ops-item --title "EC2 instance disk full" --description "Log clean up may have failed which caused the disk to be full" --priority 2 --source ec2 --operational-data '{"/aws/resources":{"Value":"[{\"arn\": \"arn:aws:dynamodb:us-west-2:12345678:table/OpsItems\"}]","Type":"SearchableString"}}' --notifications Arn="arn:aws:sns:us-west-2:12345678:TestUser"
   ```

   The following command uses the `/aws/automations` key in OperationalData to create an OpsItem that specifies the AWS\-ASGEnterStandby document as an associated automation runbook\.

   ```
   aws ssm create-ops-item --title "EC2 instance disk full" --description "Log clean up may have failed which caused the disk to be full" --priority 2 --source ec2 --operational-data '{"/aws/automations":{"Value":"[{\"automationId\": \"AWS-ASGEnterStandby\", \"automationType\": \"AWS::SSM::Automation\"}]","Type":"SearchableString"}}' --notifications Arn="arn:aws:sns:us-west-2:12345678:TestUser"
   ```

   **Windows**

   ```
   aws ssm create-ops-item --title "RDS instance not responding" --description "RDS instance not responding to ping" --priority 1 --source RDS --operational-data={\"RDS\":{\"Value\":\"abcd\",\"Type\":\"SearchableString\"}} --notifications Arn="arn:aws:sns:us-west-1:12345678:TestUser1" --tags "Key=RDS,Value=ProductionServers"
   ```

   The following command uses the `/aws/resources` key in OperationalData to create an OpsItem with an Amazon EC2 instance related resource\.

   ```
   aws ssm create-ops-item --title "EC2 instance disk full" --description "Log clean up may have failed which caused the disk to be full" --priority 2 --source ec2 --operational-data={\"/aws/resources\":{\"Value\":\"[{\\"""arn\\""":\\"""arn:aws:ec2:us-east-1:123456789012:instance/i-1234567890abcdef0\\"""}]\",\"Type\":\"SearchableString\"}}
   ```

   The following command uses the `/aws/automations` key in OperationalData to create an OpsItem that specifies the AWS\-RestartEC2Instance document as an associated automation runbook\.

   ```
   aws ssm create-ops-item --title "EC2 instance disk full" --description "Log clean up may have failed which caused the disk to be full" --priority 2 --source ec2 --operational-data={\"/aws/automations\":{\"Value\":\"[{\\"""automationId\\""":\\"""AWS-RestartEC2Instance\\”"",\\"""automationType\\""":\\"""AWS::SSM::Automation\\"""}]\",\"Type\":\"SearchableString\"}}
   ```

   **Specify operational data from a file**

   When you create an OpsItem, you can specify operational data from a file\. The file must be a JSON file, and the contents of the file must use the following format:

   ```
   {
     "key_name": {
       "Type": "SearchableString",
       "Value": "Up to 20 KB of data"
     }
   }
   ```

   Here is an example\.

   ```
   aws ssm create-ops-item --title "EC2 instance disk full" --description "Log clean up may have failed which caused the disk to be full" --priority 2 --source ec2 --operational-data file:///Users/TestUser1/Desktop/OpsItems/opsData.json --notifications Arn="arn:aws:sns:us-west-1:12345678:TestUser1" --tags "Key=EC2,Value=Production"
   ```
**Note**  
For information about how to enter JSON\-formatted parameters on the command line on different local operating systems, see [Using Quotation Marks with Strings](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-parameters.html#quoting-strings) in the *AWS Command Line Interface User Guide*\.

   The system returns information like the following:

   ```
   {
       "OpsItemId": "oi-1a2b3c4d5e6f"
   }
   ```

1. Run the following command to view details about the OpsItem you created\.

   ```
   aws ssm get-ops-item --ops-item-id ID
   ```

   The system returns information like the following:

   ```
   {
       "OpsItem": {
           "CreatedBy": "arn:aws:iam::12345678:user/TestUser",
           "CreatedTime": 1558386334.995,
           "Description": "Log clean up may have failed which caused the disk to be full",
           "LastModifiedBy": "arn:aws:iam::12345678:user/TestUser",
           "LastModifiedTime": 1558386334.995,
           "Notifications": [
               {
                   "Arn": "arn:aws:sns:us-west-1:12345678:TestUser"
               }
           ],
           "Priority": 2,
           "RelatedOpsItems": [],
           "Status": "Open",
           "OpsItemId": "oi-1a2b3c4d5e6f",
           "Title": "EC2 instance disk full",
           "Source": "ec2",
           "OperationalData": {
               "EC2": {
                   "Value": "12345",
                   "Type": "SearchableString"
               }
           }
       }
   }
   ```

1. Run the following command to update the OpsItem\. This command changes the status from `Open` \(the default\) to `InProgress`\.

   ```
   aws ssm update-ops-item --ops-item-id ID --status InProgress
   ```

   The command has no output\.

1. Run the following command again to verify that the status changed to `InProgress`

   ```
   aws ssm get-ops-item --ops-item-id ID
   ```