# Creating OpsItems manually<a name="OpsCenter-manually-create-OpsItems"></a>

This section includes information about how to manually create OpsItems in AWS Systems Manager OpsCenter\.

**Before You Begin**  
When you manually create an OpsItem, you can specify an Amazon Resource Name \(ARN\) for an impacted resource\. If you specify an ARN, then OpsCenter automatically creates a deep link to detailed information about the resource\. For example, if you specify the ARN of an impacted Amazon EC2 instance, then OpsCenter creates a deep link to the details about that instance\. For information about how to create an ARN, see the [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *Amazon Web Services General Reference*\.

**Note**  
OpsCenter doesn't support creating deep links for all ARN types\. To view a list of resources the support deep links based on ARNs, see [Supported resources reference](OpsCenter-related-resources-reference.md)\.

**Topics**
+ [Creating OpsItems by using the console](#OpsCenter-manually-create-OpsItems-console)
+ [Creating OpsItems by using the AWS CLI](#OpsCenter-manually-create-OpsItems-CLI)
+ [Creating OpsItems by using AWS Tools for Windows PowerShell](#OpsCenter-manually-create-OpsItems-PowerShell)

## Creating OpsItems by using the console<a name="OpsCenter-manually-create-OpsItems-console"></a>

The following procedure describes how to create an OpsItem by using the Systems Manager console\.

**To manually create an OpsItem by using the console**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose **Create OpsItem**\. If you don't see this button, then choose the **OpsItems** tab, and then choose **Create OpsItem**\.

1. For **Title**, enter a descriptive name to help you understand the purpose of the OpsItem\.

1. For **Source**, enter the type of impacted AWS resource or other source information to help users understand the origin of the OpsItem\.
**Note**  
You can't edit the **Source** field after you create the OpsItem\.

1. \(Optional\) For **Priority**, choose the priority level\.

1. \(Optional\) For **Severity**, choose the severity level\.

1. \(Optional\) For **Category**, choose a category\.

1. For **Description**, enter information about this OpsItem including \(if applicable\) steps for reproducing the issue\. 

1. For **Deduplication string**, enter words the system should use to check for duplicate OpsItems\. For more information about deduplication strings, see [Reducing duplicate OpsItems](OpsCenter-working-deduplication.md)\. 

1. \(Optional\) For **Notifications**, specify the Amazon SNS topic ARN where you want notifications sent when this OpsItem is updated\. You must specify an Amazon SNS ARN that is in the same AWS Region as the OpsItem\.

1. \(Optional\) Under **Related resources**, choose **Add** to specify the ID or an Amazon Resource Name \(ARN\) of the impacted resource and any related resources\.

1. Choose **Create OpsItem**\.

If successful, the OpsItem opens\. For information about how to configure the options in an OpsItem, see [Working with OpsItems](OpsCenter-working-with-OpsItems.md)\.

## Creating OpsItems by using the AWS CLI<a name="OpsCenter-manually-create-OpsItems-CLI"></a>

The following procedure describes how to create an OpsItem by using the AWS Command Line Interface \(AWS CLI\)\.

**To manually create an OpsItem by using the AWS CLI**

1. Install and configure the AWS Command Line Interface \(AWS CLI\), if you haven't already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Open the AWS Command Line Interface \(AWS CLI\) and run the following command to create an OpsItem\.

   ```
   aws ssm create-ops-item --title "Descriptive_title" --description "Information_about_the_issue" --priority Number_between_1_and_5 --source Source_of_the_issue --operational-data Up_to_20_KB_of_data_or_path_to_JSON_file --notifications Arn="SNS_ARN_in_same_Region" --tags "Key=key_name,Value=a_value"
   ```

   Here are some examples\.

   **Linux management portal macOS**

   ```
   aws ssm create-ops-item --title "EC2 instance disk full" --description "Log clean up may have failed which caused the disk to be full" --priority 2 --source ec2 --operational-data '{"EC2":{"Value":"12345","Type":"SearchableString"}}' --notifications Arn="arn:aws:sns:us-west-1:12345678:TestUser1" --tags "Key=EC2,Value=ProductionServers"
   ```

   The following command uses the `/aws/resources` key in OperationalData to create an OpsItem with an Amazon DynamoDB related resource\.

   ```
   aws ssm create-ops-item --title "EC2 instance disk full" --description "Log clean up may have failed which caused the disk to be full" --priority 2 --source ec2 --operational-data '{"/aws/resources":{"Value":"[{\"arn\": \"arn:aws:dynamodb:us-west-2:12345678:table/OpsItems\"}]","Type":"SearchableString"}}' --notifications Arn="arn:aws:sns:us-west-2:12345678:TestUser"
   ```

   The following command uses the `/aws/automations` key in OperationalData to create an OpsItem that specifies the `AWS-ASGEnterStandby` document as an associated Automation runbook\.

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

   The following command uses the `/aws/automations` key in OperationalData to create an OpsItem that specifies the AWS `-RestartEC2Instance` document as an associated Automation runbook\.

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
For information about how to enter JSON\-formatted parameters on the command line on different local operating systems, see [Using quotation marks with strings in the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-parameters-quoting-strings.html) in the *AWS Command Line Interface User Guide*\.

   The system returns information like the following\.

   ```
   {
       "OpsItemId": "oi-1a2b3c4d5e6f"
   }
   ```

1. Run the following command to view details about the OpsItem you created\.

   ```
   aws ssm get-ops-item --ops-item-id ID
   ```

   The system returns information like the following\.

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

## Creating OpsItems by using AWS Tools for Windows PowerShell<a name="OpsCenter-manually-create-OpsItems-PowerShell"></a>



1. Open AWS Tools for Windows PowerShell and run the following command to specify your credentials\. 

   ```
   Set-AWSCredentials –AccessKey key-name –SecretKey key-name
   ```

1. Run the following command to set the region for your PowerShell session\.

   ```
   Set-DefaultAWSRegion -Region Region
   ```

1. Run the following command to create a new OpsItem\. This command specifies a Systems Manager Automation runbook for remediating this OpsItem\. 

   ```
   $opsItem = New-Object Amazon.SimpleSystemsManagement.Model.OpsItemDataValue
   $opsItem.Type = [Amazon.SimpleSystemsManagement.OpsItemDataType]::SearchableString 
   $opsItem.Value = '[{\"automationId\":\"runbook_name\",\"automationType\":\"AWS::SSM::Automation\"}]'
   $newHash = @{" /aws/automations"=[Amazon.SimpleSystemsManagement.Model.OpsItemDataValue]$opsItem}
   New-SSMOpsItem -Title "title" -Description "description" -Priority priority_number -Source AWS_service -OperationalData $newHash
   ```

   If successful, the command outputs the ID of the new OpsItem\.

   The following example specifies the ARN of an impaired Amazon EC2 instance\.

   ```
   $opsItem = New-Object Amazon.SimpleSystemsManagement.Model.OpsItemDataValue
   $opsItem.Type = [Amazon.SimpleSystemsManagement.OpsItemDataType]::SearchableString 
   $opsItem.Value = '[{\"arn\":\"arn:aws:ec2:us-east-1:123456789012:instance/i-1234567890abcdef0\"}]'
   $newHash = @{" /aws/resources"=[Amazon.SimpleSystemsManagement.Model.OpsItemDataValue]$opsItem}
   New-SSMOpsItem -Title "EC2 instance disk full still" -Description "Log clean up may have failed which caused the disk to be full" -Priority 2 -Source ec2 -OperationalData $newHash
   ```