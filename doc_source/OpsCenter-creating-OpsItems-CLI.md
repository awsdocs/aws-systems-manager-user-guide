# Creating OpsItems manually \(AWS CLI\)<a name="OpsCenter-creating-OpsItems-CLI"></a>

The following procedure describes how to create an OpsItem by using the AWS Command Line Interface \(AWS CLI\)\.

**To create an OpsItem using the AWS CLI**

1. Install and configure the AWS Command Line Interface \(AWS CLI\), if you haven't already\.

   For information, see [Installing or updating the latest version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)\.

1. Open the AWS CLI and run the following command to create an OpsItem\. Replace each *example resource placeholder* with your own information\.

   ```
   aws ssm create-ops-item \
       --title "Descriptive_title" \
       --description "Information_about_the_issue" \
       --priority Number_between_1_and_5 \
       --source Source_of_the_issue \
       --operational-data Up_to_20_KB_of_data_or_path_to_JSON_file \
       --notifications Arn="SNS_ARN_in_same_Region" \
       --tags "Key=key_name,Value=a_value"
   ```

   **Specify operational data from a file**

   When you create an OpsItem, you can specify operational data from a file\. The file must be a JSON file, and the contents of the file must use the following format\.

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
   aws ssm create-ops-item ^
       --title "EC2 instance disk full" ^
       --description "Log clean up may have failed which caused the disk to be full" ^
       --priority 2 ^
       --source ec2 ^
       --operational-data file:///Users/TestUser1/Desktop/OpsItems/opsData.json ^
       --notifications Arn="arn:aws:sns:us-west-1:12345678:TestUser1" ^
       --tags "Key=EC2,Value=Production"
   ```
**Note**  
For information about how to enter JSON\-formatted parameters on the command line on different local operating systems, see [Using quotation marks with strings in the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-parameters-quoting-strings.html) in the *AWS Command Line Interface User Guide*\.

   The system returns information like the following\.

   ```
   {
       "OpsItemId": "oi-1a2b3c4d5e6f"
   }
   ```

1. Run the following command to view details about the OpsItem that you created\.

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

1. Run the following command again to verify that the status changed to `InProgress`\.

   ```
   aws ssm get-ops-item --ops-item-id ID
   ```

## Examples of creating an OpsItem<a name="OpsCenter_creating_OpsItems-CLI_examples"></a>

The following code examples show you how to create an OpsItem by using the Linux management portal, macOS, or Windows\. 

**Linux management portal or macOS**

The following command creates an OpsItem when an Amazon Elastic Compute Cloud \(Amazon EC2\) instance disk is full\. 

```
aws ssm create-ops-item \
    --title "EC2 instance disk full" \
    --description "Log clean up may have failed which caused the disk to be full" \
    --priority 2 \
    --source ec2 \
    --operational-data '{"EC2":{"Value":"12345","Type":"SearchableString"}}' \
    --notifications Arn="arn:aws:sns:us-west-1:12345678:TestUser1" \
    --tags "Key=EC2,Value=ProductionServers"
```

The following command uses the `/aws/resources` key in `OperationalData` to create an OpsItem with an Amazon DynamoDB related resource\.

```
aws ssm create-ops-item \
    --title "EC2 instance disk full" \
    --description "Log clean up may have failed which caused the disk to be full" \
    --priority 2 \
    --source ec2 \
    --operational-data '{"/aws/resources":{"Value":"[{\"arn\": \"arn:aws:dynamodb:us-west-2:12345678:table/OpsItems\"}]","Type":"SearchableString"}}' \
    --notifications Arn="arn:aws:sns:us-west-2:12345678:TestUser"
```

The following command uses the `/aws/automations` key in `OperationalData` to create an OpsItem that specifies the `AWS-ASGEnterStandby` document as an associated Automation runbook\.

```
aws ssm create-ops-item \
    --title "EC2 instance disk full" \
    --description "Log clean up may have failed which caused the disk to be full" \
    --priority 2 \
    --source ec2 \
    --operational-data '{"/aws/automations":{"Value":"[{\"automationId\": \"AWS-ASGEnterStandby\", \"automationType\": \"AWS::SSM::Automation\"}]","Type":"SearchableString"}}' \
    --notifications Arn="arn:aws:sns:us-west-2:12345678:TestUser"
```

**Windows**

The following command creates an OpsItem when an Amazon Relational Database Service \(Amazon RDS\) instance is not responding\. 

```
aws ssm create-ops-item ^
    --title "RDS instance not responding" ^
    --description "RDS instance not responding to ping" ^
    --priority 1 ^
    --source RDS ^
    --operational-data={\"RDS\":{\"Value\":\"abcd\",\"Type\":\"SearchableString\"}} ^
    --notifications Arn="arn:aws:sns:us-west-1:12345678:TestUser1" ^
    --tags "Key=RDS,Value=ProductionServers"
```

The following command uses the `/aws/resources` key in `OperationalData` to create an OpsItem with an Amazon EC2 instance related resource\.

```
aws ssm create-ops-item ^
    --title "EC2 instance disk full" ^
    --description "Log clean up may have failed which caused the disk to be full" ^
    --priority 2 ^
    --source ec2 ^
    --operational-data={\"/aws/resources\":{\"Value\":\"[{\\"""arn\\""":\\"""arn:aws:ec2:us-east-1:123456789012:instance/i-1234567890abcdef0\\"""}]\",\"Type\":\"SearchableString\"}}
```

The following command uses the `/aws/automations` key in `OperationalData` to create an OpsItem that specifies the `AWS-RestartEC2Instance` runbook as an associated Automation runbook\.

```
aws ssm create-ops-item ^
    --title "EC2 instance disk full" ^
    --description "Log clean up may have failed which caused the disk to be full" ^
    --priority 2 ^
    --source ec2 ^
    --operational-data={\"/aws/automations\":{\"Value\":\"[{\\"""automationId\\""":\\"""AWS-RestartEC2Instance\\”"",\\"""automationType\\""":\\"""AWS::SSM::Automation\\"""}]\",\"Type\":\"SearchableString\"}}
```