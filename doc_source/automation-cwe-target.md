# Running automations with triggers using EventBridge<a name="automation-cwe-target"></a>

You can start an automation by specifying a runbook as the target of an Amazon EventBridge event\. You can start automations according to a schedule, or when a specific AWS system event occurs\. For example, let's say you create a runbook named *BootStrapInstances* that installs software on an instance when an instance starts\. To specify the *BootStrapInstances* runbook \(and corresponding automation\) as a target of an EventBridge event, you first create a new EventBridge rule\. \(Here's an example rule: **Service name**: EC2, **Event Type**: EC2 Instance State\-change Notification, **Specific state\(s\)**: running, **Any instance**\.\) Then you use the following procedures to specify the *BootStrapInstances* runbook as the target of the event using the EventBridge console and AWS Command Line Interface \(AWS CLI\)\. When a new instance starts, the system runs the automation and installs software\.

For information about creating runbooks, see [Working with runbooks](automation-documents.md)\.

## Creating an EventBridge event that uses a runbook \(console\)<a name="automation-cwe-target-console"></a>

Use the following procedure to configure a runbook as the target of a EventBridge event\.

**To configure a runbook as a target of a EventBridge event rule**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**, and then choose **Create rule**\.

   \-or\-

   If the Amazon EventBridge home page opens first, choose **Create rule**\.

1. Enter a name and description for the rule\.

   A rule can't have the same name as another rule in the same Region and on the same event bus\.

1. For **Define pattern**, choose either **Event pattern** or **Schedule**\. Use **Event pattern** to build a rule that generates events for specific actions in AWS services\. Use **Schedule** to build a rule that generates events according to a schedule that you specify by using the cron format\.

1. Choose the remaining options for the rule you want to create, and then choose **Add target**\.

   For information about creating EventBridge rules, see [Getting Started with Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-getting-set-up.html) in the *Amazon EventBridge User Guide*\.

1. For **Select event bus**, choose the event bus that you want to associate with this rule\. If you want this rule to initiate on matching events that come from your own AWS account, select **AWS default event bus**\. When an AWS service in your account emits an event, it always goes to your account’s default event bus\. 

1. For **Target**, choose **Systems Manager Automation**\. 

1. For **Document**, choose a runbook to use when your target is invoked\.

1. Expand **Configure document version**, and choose a version\. $DEFAULT was explicitly set as the default runbook version in Systems Manager\. You can choose a specific version, or use the latest version\.

1. Expand **Configure automation parameter\(s\)**, and either keep the default parameter values \(if available\) or enter your own values\. 
**Note**  
Required parameters have an asterisk \(\*\) next to the parameter name\. To create a target, you must specify a value for each required parameter\. If you don't, the system creates the rule, but it won't run\.

1. At the bottom of the **Select targets** area, choose a role to grant EventBridge permission to start the automation with the specified runbook and parameters\. EventBridge uses the role to start the automation\. You can let EventBridge create a new role or use a role that already has the needed permissions\.

1. \(Optional\) Enter one or more tags for the rule\. For more information, see [Tagging Your Amazon EventBridge Resources](https://docs.aws.amazon.com/eventbridge/latest/userguide/eventbridge-tagging.html) in the *Amazon EventBridge User Guide*\.

1. Choose **Create** and complete the wizard\.

## Create an EventBridge event that uses a runbook \(command line\)<a name="automation-cwe-target-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to create an EventBridge event rule and configure a runbook as the target\.

**To configure a runbook as a target of an EventBridge event rule**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you haven't already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Create a command to specify a new EventBridge event rule\. Replace each *example resource placeholder* with your own information\.

   *Triggers based on a schedule*

------
#### [ Linux & macOS ]

   ```
   aws events put-rule \
       --name "rule name" \
       --schedule-expression "cron or rate expression"
   ```

------
#### [ Windows ]

   ```
   aws events put-rule ^
       --name "rule name" ^
       --schedule-expression "cron or rate expression"
   ```

------
#### [ PowerShell ]

   ```
   Write-CWERule `
       -Name "rule name" `
       -ScheduleExpression "cron or rate expression"
   ```

------

   The following example creates an EventBridge event rule that starts every day at 9:00 AM \(UTC\)\.

------
#### [ Linux & macOS ]

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
#### [ Linux & macOS ]

   ```
   aws events put-rule \
       --name "rule name" \
       --event-pattern "{\"source\":[\"aws.service\"],\"detail-type\":[\"service event detail type\"]}"
   ```

------
#### [ Windows ]

   ```
   aws events put-rule ^
       --name "rule name" ^
       --event-pattern "{\"source\":[\"aws.service\"],\"detail-type\":[\"service event detail type\"]}"
   ```

------
#### [ PowerShell ]

   ```
   Write-CWERule `
       -Name "rule name" `
       -EventPattern '{"source":["aws.service"],"detail-type":["service event detail type"]}'
   ```

------

   The following example creates an EventBridge event rule that starts when any EC2 instance in the Region changes state\.

------
#### [ Linux & macOS ]

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

   The command returns details for the new EventBridge rule similar to the following\.

------
#### [ Linux & macOS ]

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

1. Create a command to specify a runbook as a target of the EventBridge event rule you created in step 2\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

   ```
   aws events put-targets \
       --rule rule name \
       --targets '{"Arn": "arn:aws:ssm:us-east-1:123456789012:automation-definition/runbook name","Input":"{\"input parameter\":[\"value\"],\"AutomationAssumeRole\":[\"arn:aws:iam::123456789012:role/AutomationServiceRole\"]}","Id": "target ID","RoleArn": "arn:aws:iam::123456789012:role/service-role/EventBridge service role"}'
   ```

------
#### [ Windows ]

   ```
   aws events put-targets ^
       --rule rule name ^
       --targets '{"Arn": "arn:aws:ssm:us-east-1:123456789012:automation-definition/runbook name","Input":"{\"input parameter\":[\"value\"],\"AutomationAssumeRole\":[\"arn:aws:iam::123456789012:role/AutomationServiceRole\"]}","Id": "target ID","RoleArn": "arn:aws:iam::123456789012:role/service-role/EventBridge service role"}'
   ```

------
#### [ PowerShell ]

   ```
   $Target = New-Object Amazon.CloudWatchEvents.Model.Target
   $Target.Id = "target ID"
   $Target.Arn = "arn:aws:ssm:us-east-1:123456789012:automation-definition/runbook name"
   $Target.RoleArn = "arn:aws:iam::123456789012:role/service-role/EventBridge service role"
   $Target.Input = '{"input parameter":["value"],"AutomationAssumeRole":["arn:aws:iam::123456789012:role/AutomationServiceRole"]}'
   
   Write-CWETarget `
       -Rule "rule name" `
       -Target $Target
   ```

------

   The following example creates an EventBridge event target that starts the specified instance ID using the runbook `AWS-StartEC2Instance`\.

------
#### [ Linux & macOS ]

   ```
   aws events put-targets \
       --rule DailyAutomationRule \
       --targets '{"Arn": "arn:aws:ssm:us-east-1::automation-definition/AWS-StartEC2Instance","Input":"{\"InstanceId\":[\"i-02573cafcfEXAMPLE\"],\"AutomationAssumeRole\":[\"arn:aws:iam::123456789012:role/AutomationServiceRole\"]}","Id": "Target1","RoleArn": "arn:aws:iam::123456789012:role/service-role/AWS_Events_Invoke_Start_Automation_Execution_1213609520"}'
   ```

------
#### [ Windows ]

   ```
   aws events put-targets ^
       --rule DailyAutomationRule ^
       --targets '{"Arn": "arn:aws:ssm:us-east-1::automation-definition/AWS-StartEC2Instance","Input":"{\"InstanceId\":[\"i-02573cafcfEXAMPLE\"],\"AutomationAssumeRole\":[\"arn:aws:iam::123456789012:role/AutomationServiceRole\"]}","Id": "Target1","RoleArn": "arn:aws:iam::123456789012:role/service-role/AWS_Events_Invoke_Start_Automation_Execution_1213609520"}'
   ```

------
#### [ PowerShell ]

   ```
   $Target = New-Object Amazon.CloudWatchEvents.Model.Target
   $Target.Id = "Target1"
   $Target.Arn = "arn:aws:ssm:us-east-1::automation-definition/AWS-StartEC2Instance"
   $Target.RoleArn = "arn:aws:iam::123456789012:role/service-role/AWS_Events_Invoke_Start_Automation_Execution_1213609520"
   $Target.Input = '{"InstanceId":["i-02573cafcfEXAMPLE"],"AutomationAssumeRole":["arn:aws:iam::123456789012:role/AutomationServiceRole"]}'
   
   Write-CWETarget `
       -Rule "DailyAutomationRule" `
       -Target $Target
   ```

------

   The system returns information like the following\.

------
#### [ Linux & macOS ]

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
