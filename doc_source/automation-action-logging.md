# Logging Automation action output with CloudWatch Logs<a name="automation-action-logging"></a>

AWS Systems Manager Automation integrates with Amazon CloudWatch Logs \(CloudWatch Logs\)\. As a result, you can send the output from `aws:executeScript` actions in your runbooks to the log group you specify\. You can use the action output stored in your CloudWatch Logs log group for debugging and troubleshooting purposes\. If you choose a log group that is encrypted, the action output is also encrypted\. Logging output from `aws:executeScript` actions is an account\-level setting\.

**To send action output to CloudWatch Logs \(console\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**\.

1. Choose the **Preferences** tab, and then choose **Edit**\.

1. Select the check box next to **Send output to CloudWatch Logs**\.

1. \(Recommended\) Select the check box next to **Encrypt log data**\. With this option enabled, log data is encrypted using the server\-side encryption key specified for the log group\. If you do not want to encrypt the log data that is sent to CloudWatch Logs, clear the check box\. You must also clear the check box if encryption is not enabled on the log group\.

1. For **CloudWatch Logs log group**, to specify the existing CloudWatch Logs log group in your AWS account that you want to send action output to, select one of the following:
   + **Send output to the default log group** – If the default log group doesn't exist \(/aws/ssm/automation/executeScript\), Automation creates it for you\.
   + **Choose from a list of log groups** – Select a log group that has already been created in your account to store action output\.
   + Enter the name of a log group in the text box that has already been created in your account to store action output\.

1. Choose **Save**\.

**To send action output to CloudWatch Logs \(command line\)**

1. Open your preferred command line tool and run the following command to update the action output destination\.

------
#### [ Linux ]

   ```
   aws ssm update-service-setting \
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/automation/customer-script-log-destination \
       --setting-value CloudWatch
   ```

------
#### [ Windows ]

   ```
   aws ssm update-service-setting ^
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/automation/customer-script-log-destination ^
       --setting-value CloudWatch
   ```

------
#### [ PowerShell ]

   ```
   Update-SSMServiceSetting `
       -SettingId "arn:aws:ssm:region:aws-account-id:servicesetting/ssm/automation/customer-script-log-destination" `
       -SettingValue "CloudWatch"
   ```

------

   There is no output if the command succeeds\.

1. Run the following command to specify the log group you want to send action output to\.

------
#### [ Linux ]

   ```
   aws ssm update-service-setting \
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/automation/customer-script-log-group-name \
       --setting-value my-log-group
   ```

------
#### [ Windows ]

   ```
   aws ssm update-service-setting ^
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/automation/customer-script-log-group-name ^
       --setting-value my-log-group
   ```

------
#### [ PowerShell ]

   ```
   Update-SSMServiceSetting `
       -SettingId "arn:aws:ssm:region:aws-account-id:servicesetting/ssm/automation/customer-script-log-group-name" `
       -SettingValue "my-log-group"
   ```

------

   There is no output if the command succeeds\.

1. Run the following command to view the current service settings for Automation action logging preferences in the current AWS account and AWS Region\.

------
#### [ Linux ]

   ```
   aws ssm get-service-setting \
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/automation/customer-script-log-destination
   ```

------
#### [ Windows ]

   ```
   aws ssm get-service-setting ^
       --setting-id arn:aws:ssm:region:aws-account-id:servicesetting/ssm/automation/customer-script-log-destination
   ```

------
#### [ PowerShell ]

   ```
   Get-SSMServiceSetting `
       -SettingId "arn:aws:ssm:region:aws-account-id:servicesetting/ssm/automation/customer-script-log-destination"
   ```

------

   The command returns information like the following\.

   ```
   {
       "ServiceSetting": {
           "Status": "Customized",
           "LastModifiedDate": 1613758617.036,
           "SettingId": "/ssm/automation/customer-script-log-destination",
           "LastModifiedUser": "arn:aws:sts::123456789012:assumed-role/Administrator/User_1",
           "SettingValue": "CloudWatch",
           "ARN": "arn:aws:ssm:us-east-2:123456789012:servicesetting/ssm/automation/customer-script-log-destination"
       }
   }
   ```