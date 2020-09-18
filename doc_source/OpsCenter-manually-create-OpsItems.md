# Creating OpsItems manually<a name="OpsCenter-manually-create-OpsItems"></a>

This section includes procedures for manually create OpsItems for issues that aren't automatically created by Amazon EventBridge\.

**Before You Begin**  
If you manually create an OpsItem for an impacted AWS resource, then collect information about that resource so that you can create an Amazon Resource Name \(ARN\)\. If you specify an ARN when you create an OpsItem, then OpsCenter automatically creates a deep link to detailed information about the resource\. For example, if you specify the ARN of an impacted EC2 instance, then OpsCenter creates a deep link to the details about that instance\. For information about how to create an ARN, see the [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *Amazon Web Services General Reference*\.

**Note**  
OpsCenter does not support creating deep links for all ARN types\. To view a list of resources the support deep links based on ARNs, see [Supported resources reference](OpsCenter-related-resources-reference.md)\.

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

1. For **Deduplication string**, enter words the system should use to check for duplicate OpsItems\. For more information about deduplication strings, see [Reducing duplicate OpsItems](OpsCenter-working-with-OpsItems.md#OpsCenter-working-deduplication)\. 

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

   The following command uses the `/aws/resources` key in OperationalData to create an OpsItem with an EC2 instance related resource\.

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
For information about how to enter JSON\-formatted parameters on the command line on different local operating systems, see [Using quotation marks with strings](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-parameters.html#quoting-strings) in the *AWS Command Line Interface User Guide*\.

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