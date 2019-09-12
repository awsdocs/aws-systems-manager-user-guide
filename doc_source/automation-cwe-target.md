# Running Automation Workflows with Triggers using CloudWatch Events<a name="automation-cwe-target"></a>

You can start an Automation workflow by specifying an Automation document as the target of an Amazon CloudWatch event\. You can start workflows according to a schedule, or when a specific AWS system event occurs\. For example, let's say you create an Automation document named *BootStrapInstances* that installs software on an instance when an instance starts\. To specify the *BootStrapInstances* document \(and corresponding workflow\) as a target of a CloudWatch event, you first create a new CloudWatch Events rule\. \(Here's an example rule: **Service name**: EC2, **Event Type**: EC2 Instance State\-change Notification, **Specific state\(s\)**: running, **Any instance**\.\) Then you use the following procedures to specify the *BootStrapInstances* document as the target of the event using the CloudWatch console, AWS Command Line Interface \(AWS CLI\), or AWS Tools for Windows PowerShell\. When a new instance starts, the system runs the workflow and installs software\.

For information about creating Automation documents, see [Working with Automation Documents](automation-documents.md)\.

## Creating a CloudWatch Event that Runs an Automation Workflow \(Console\)<a name="automation-cwe-target-console"></a>

Use the following procedure to configure an Automation workflow as the target of a CloudWatch event\.

**To configure an Automation document as a target of a CloudWatch event rule**

1. Sign in to the AWS Management Console and open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the left navigation pane, choose **Events**, and then choose **Create rule**\.

1. Choose **Event Pattern** or **Schedule**\. **Event Pattern** lets you build a rule that generates events for specific actions in AWS services\. **Schedule** lets you build a rule that generates events according to a schedule that you specify by using the cron format\.

1. Choose the remaining options for the rule you want to create, and then choose **Add target**\.

1. In the **Select target type** list, choose **SSM Automation**\. 

1. In the **Document** list, choose an Automation document to run when your target is invoked\.

1. Expand **Configure document version**, and choose a version\. $DEFAULT was explicitly set as the default document version in Systems Manager\. You can choose a specific version, or use the latest version\.

1. Expand **Configure automation parameter\(s\)**, and either keep the default parameter values \(if available\) or enter your own values\. 
**Note**  
Required parameters have an asterisk \(\*\) next to the parameter name\. To create a target, you must specify a value for each required parameter\. If you don't, the system creates the rule, but it won't run\.

1. In the permissions section, choose an option\. CloudWatch uses the role to start the Automation workflow\. 

1. Choose **Configure details** and complete the wizard\.

## Create a CloudWatch Event that Runs an Automation Document \(Command Line\)<a name="automation-cwe-target-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to create a CloudWatch event rule and configure an Automation document as the target\.

**To configure an Automation document as a target of a CloudWatch event rule**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or Upgrade the AWS CLI](getting-started-cli.md) or [Install or Upgrade the AWS Tools for PowerShell](getting-started-ps.md)\.

1. Create a command to specify a new CloudWatch event rule\. Here are some template commands to help\.

   *Triggers based on a schedule*

------
#### [ Linux ]

   ```
   aws events put-rule \
     --name "rule_name" \
     --schedule-expression "cron_or_rate_expression"
   ```

------
#### [ Windows ]

   ```
   aws events put-rule ^
     --name "rule_name" ^
     --schedule-expression "cron_or_rate_expression"
   ```

------
#### [ PowerShell ]

   ```
   Write-CWERule `
     -Name "rule_name" `
     -ScheduleExpression "cron_or_rate_expression"
   ```

------

   The following example creates a CloudWatch event rule that triggers every day at 9:00am \(UTC\)\.

------
#### [ Linux ]

   ```
   aws events put-rule \
     --name "DailyAutomationRule" \
     --schedule-expression "cron(0 9 * * ? *)"
   ```

------
#### [ Windows ]

   ```
   aws events put-rule ^
     --name "DailyAutomationRule" ^
     --schedule-expression "cron(0 9 * * ? *)"
   ```

------
#### [ PowerShell ]

   ```
   Write-CWERule `
     -Name "DailyAutomationRule" `
     -ScheduleExpression "cron(0 9 * * ? *)"
   ```

------

   *Triggers based on an event*

------
#### [ Linux ]

   ```
   aws events put-rule \
     --name "rule_name" \
     --event-pattern "{\"source\":[\"aws.service\"],\"detail-type\":[\"service_event_detail_type\"]}"
   ```

------
#### [ Windows ]

   ```
   aws events put-rule ^
     --name "rule_name" ^
     --event-pattern "{\"source\":[\"aws.service\"],\"detail-type\":[\"service_event_detail_type\"]}"
   ```

------
#### [ PowerShell ]

   ```
   Write-CWERule `
     -Name "rule_name" `
     -EventPattern '{"source":["aws.service"],"detail-type":["service_event_detail_type"]}'
   ```

------

   The following example creates a CloudWatch event rule that triggers when any Amazon EC2 instance in the region changes state\.

------
#### [ Linux ]

   ```
   aws events put-rule \
     --name "EC2InstanceStateChanges" \
     --event-pattern "{\"source\":[\"aws.ec2\"],\"detail-type\":[\"EC2 Instance State-change Notification\"]}"
   ```

------
#### [ Windows ]

   ```
   aws events put-rule ^
     --name "EC2InstanceStateChanges" ^
     --event-pattern "{\"source\":[\"aws.ec2\"],\"detail-type\":[\"EC2 Instance State-change Notification\"]}"
   ```

------
#### [ PowerShell ]

   ```
   Write-CWERule `
     -Name "EC2InstanceStateChanges" `
     -EventPattern '{"source":["aws.ec2"],"detail-type":["EC2 Instance State-change Notification"]}'
   ```

------

   The command returns details for the new CloudWatch rule similar to the following\.

------
#### [ Linux ]

   ```
   {
       "RuleArn": "arn:aws:events:us-east-1:123456789012:rule/automationrule"
   }
   ```

------
#### [ Windows ]

   ```
   {
       "RuleArn": "arn:aws:events:us-east-1:123456789012:rule/automationrule"
   }
   ```

------
#### [ PowerShell ]

   ```
   arn:aws:events:us-east-1:123456789012:rule/EC2InstanceStateChanges
   ```

------

1. Create a command to specify an Automation document as a target of the CloudWatch event rule you created in step 2\. Here are some template commands to help\.

------
#### [ Linux ]

   ```
   aws events put-targets \
     --rule CW_Event_Rule_Name \
     --targets '{"Arn": "arn:aws:ssm:us-east-1:123456789012:automation-definition/Automation_Document_Name","Input":"{\"DocumentParameter\":[\"ParameterValue\"],\"AutomationAssumeRole\":[\"arn:aws:iam::123456789012:role/AutomationServiceRole\"]}","Id": "Target_Id","RoleArn": "arn:aws:iam::123456789012:role/service-role/CWE_Role_Name_To_Run_Automation"}'
   ```

------
#### [ Windows ]

   ```
   aws events put-targets ^
     --rule CW_Event_Rule_Name ^
     --targets '{"Arn": "arn:aws:ssm:us-east-1:123456789012:automation-definition/Automation_Document_Name","Input":"{\"DocumentParameter\":[\"ParameterValue\"],\"AutomationAssumeRole\":[\"arn:aws:iam::123456789012:role/AutomationServiceRole\"]}","Id": "Target_Id","RoleArn": "arn:aws:iam::123456789012:role/service-role/CWE_Role_Name_To_Run_Automation"}'
   ```

------
#### [ PowerShell ]

   ```
   $Target = New-Object Amazon.CloudWatchEvents.Model.Target
   $Target.Id = "Target_Id"
   $Target.Arn = "arn:aws:ssm:us-east-1:123456789012:automation-definition/Automation_Document_Name"
   $Target.RoleArn = "arn:aws:iam::123456789012:role/service-role/CWE_Role_Name_To_Run_Automation"
   $Target.Input = '{"DocumentParameter":["DocumentValue"],"AutomationAssumeRole":["arn:aws:iam::123456789012:role/AutomationServiceRole"]}'
   
   Write-CWETarget `
     -Rule "CW_Event_Rule_Name" `
     -Target $Target
   ```

------

   The following example creates a CloudWatch event target that starts the specified instance ID using the document `AWS-StartEC2Instance`\.

------
#### [ Linux ]

   ```
   aws events put-targets \
     --rule DailyAutomationRule \
     --targets '{"Arn": "arn:aws:ssm:us-east-1:123456789012:automation-definition/AWS-StartEC2Instance","Input":"{\"InstanceId\":[\"i-02573cafcfEXAMPLE\"],\"AutomationAssumeRole\":[\"arn:aws:iam::123456789012:role/AutomationServiceRole\"]}","Id": "Target1","RoleArn": "arn:aws:iam::123456789012:role/service-role/AWS_Events_Invoke_Start_Automation_Execution_1213609520"}'
   ```

------
#### [ Windows ]

   ```
   aws events put-targets ^
     --rule DailyAutomationRule ^
     --targets '{"Arn": "arn:aws:ssm:us-east-1:123456789012:automation-definition/AWS-StartEC2Instance","Input":"{\"InstanceId\":[\"i-02573cafcfEXAMPLE\"],\"AutomationAssumeRole\":[\"arn:aws:iam::123456789012:role/AutomationServiceRole\"]}","Id": "Target1","RoleArn": "arn:aws:iam::123456789012:role/service-role/AWS_Events_Invoke_Start_Automation_Execution_1213609520"}'
   ```

------
#### [ PowerShell ]

   ```
   $Target = New-Object Amazon.CloudWatchEvents.Model.Target
   $Target.Id = "Target1"
   $Target.Arn = "arn:aws:ssm:us-east-1:123456789012:automation-definition/AWS-StartEC2Instance"
   $Target.RoleArn = "arn:aws:iam::123456789012:role/service-role/AWS_Events_Invoke_Start_Automation_Execution_1213609520"
   $Target.Input = '{"InstanceId":["i-02573cafcfEXAMPLE"],"AutomationAssumeRole":["arn:aws:iam::123456789012:role/AutomationServiceRole"]}'
   
   Write-CWETarget `
     -Rule "DailyAutomationRule" `
     -Target $Target
   ```

------

   The system returns information like the following\.

------
#### [ Linux ]

   ```
   {
       "FailedEntries": [],
       "FailedEntryCount": 0
   }
   ```

------
#### [ Windows ]

   ```
   {
       "FailedEntries": [],
       "FailedEntryCount": 0
   }
   ```

------
#### [ PowerShell ]

   There is no output if the command succeeds for PowerShell\.

------