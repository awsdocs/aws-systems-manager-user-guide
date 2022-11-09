# Run an automation with approvers<a name="running-automations-require-approvals"></a>

The following procedures describe how to use the AWS Systems Manager console and AWS Command Line Interface \(AWS CLI\) to run an automation with approvals using simple execution\. The automation uses the automation action `aws:approve`, which temporarily pauses the automation until the designated principals either approve or deny the action\. The automation runs in the context of the current AWS Identity and Access Management \(IAM\) user\. This means that you don't need to configure additional IAM permissions as long as you have permission to use the runbook, and any actions called by the runbook\. If you have administrator permissions in IAM, then you already have permission to use this runbook\.

**Before you begin**  
In addition to the standard inputs required by the runbook, the `aws:approve` action requires the following two parameters: 
+ A list of approvers\. The list of approvers must contain at least one approver in the form of an IAM user or a user ARN\. If multiple approvers are provided, a corresponding minimum approval count must also be specified within the runbook\. 
+ An Amazon Simple Notification Service \(Amazon SNS\) topic ARN\. The Amazon SNS topic name must start with `Automation`\.

This procedure assumes that you have already created an Amazon SNS topic, which is required to deliver the approval request\. For information, see [Create a Topic](https://docs.aws.amazon.com/sns/latest/dg/sns-getting-started.html#CreateTopic) in the *Amazon Simple Notification Service Developer Guide*\.

## Running an automation with approvers \(console\)<a name="approval-console"></a>

**To run an automation with approvers**

The following procedure describes how to use the Systems Manager console to run an automation with approvers\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**, and then choose **Execute automation**\.

1. In the **Automation document** list, choose a runbook\. Choose one or more options in the **Document categories** pane to filter SSM documents according to their purpose\. To view a runbook that you own, choose the **Owned by me** tab\. To view a runbook that is shared with your account, choose the **Shared with me** tab\. To view all runbooks, choose the **All documents** tab\.
**Note**  
You can view information about a runbook by choosing the runbook name\.

1. In the **Document details** section, verify that **Document version** is set to the version that you want to run\. The system includes the following version options: 
   + **Default version at runtime**: Choose this option if the Automation runbook is updated periodically and a new default version is assigned\.
   + **Latest version at runtime**: Choose this option if the Automation runbook is updated periodically, and you want to run the version that was most recently updated\.
   + **1 \(Default\)**: Choose this option to run the first version of the document, which is the default\.

1. Choose **Next**\.

1. On the **Execute automation document** page, choose **Simple execution**\.

1. In the **Input parameters** section, specify the required input parameters\.

   For example, if you chose the `AWS-StartEC2InstanceWithApproval` runbook, then you must specify or choose instance IDs for the **InstanceId** parameter\. 

1. In the **Approvers** section, specify the IAM users or user ARNs of approvers for the automation action\.

1. In the **SNSTopicARN** section, specify the SNS topic ARN to use for sending approval notification\. The SNS topic name must start with **Automation**\.

1. Optionally, you can choose an IAM service role from the **AutomationAssumeRole** list\.

1. Choose **Execute automation**\. 

The specified approver receives an Amazon SNS notification with details to approve or reject the automation\. This approval action is valid for 7 days from the date of issue and can be issued using the Systems Manager console or the AWS Command Line Interface \(AWS CLI\)\.

If you chose to approve the automation, the automation continues to run the steps included in the specified runbook\. The console displays the status of the automation\. If the automation fails to run, see [Troubleshooting Systems Manager Automation](automation-troubleshooting.md)\.

**To approve or deny an automation**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**, and then select the automation that was run in the previous procedure\.

1. Choose **Actions** and then choose **Approve/Deny**\.

1. Choose to **Approve** or **Deny** and optionally provide a comment\.

1. Choose **Submit**\.

## Running an automation with approvers \(command line\)<a name="approval-cli"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to run an automation with approvers\.

**To run an automation with approvers**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you haven't already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Run the following command to run an automation with approvers\. Replace each *example resource placeholder* with your own information\. In the document name section, specify a runbook that includes the automation action, `aws:approve`\.

   For `Approvers`, specify the IAM users or user ARNs of approvers for the action\. For `SNSTopic`, specify the SNS topic ARN to use to send approval notification\. The Amazon SNS topic name must start with `Automation`\.
**Note**  
The specific names of the parameter values for approvers and the SNS topic depend on the values specified within the runbook you choose\. 

------
#### [ Linux & macOS ]

   ```
   aws ssm start-automation-execution \
       --document-name "AWS-StartEC2InstanceWithApproval" \
       --parameters "InstanceId=i-02573cafcfEXAMPLE,Approvers=arn:aws:iam::123456789012:role/Administrator,SNSTopicArn=arn:aws:sns:region:123456789012:AutomationApproval"
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name "AWS-StartEC2InstanceWithApproval" ^
       --parameters "InstanceId=i-02573cafcfEXAMPLE,Approvers=arn:aws:iam::123456789012:role/Administrator,SNSTopicArn=arn:aws:sns:region:123456789012:AutomationApproval"
   ```

------
#### [ PowerShell ]

   ```
   Start-SSMAutomationExecution `
       -DocumentName AWS-StartEC2InstanceWithApproval `
       -Parameters @{
           "InstanceId"="i-02573cafcfEXAMPLE"
           "Approvers"="arn:aws:iam::123456789012:role/Administrator"
           "SNSTopicArn"="arn:aws:sns:region:123456789012:AutomationApproval"
       }
   ```

------

   The system returns information like the following\.

------
#### [ Linux & macOS ]

   ```
   {
       "AutomationExecutionId": "df325c6d-b1b1-4aa0-8003-6cb7338213c6"
   }
   ```

------
#### [ Windows ]

   ```
   {
       "AutomationExecutionId": "df325c6d-b1b1-4aa0-8003-6cb7338213c6"
   }
   ```

------
#### [ PowerShell ]

   ```
   df325c6d-b1b1-4aa0-8003-6cb7338213c6
   ```

------

**To approve an automation**
+ Run the following command to approve an automation\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

  ```
  aws ssm send-automation-signal \
      --automation-execution-id "df325c6d-b1b1-4aa0-8003-6cb7338213c6" \
      --signal-type "Approve" \
      --payload "Comment=your comments"
  ```

------
#### [ Windows ]

  ```
  aws ssm send-automation-signal ^
      --automation-execution-id "df325c6d-b1b1-4aa0-8003-6cb7338213c6" ^
      --signal-type "Approve" ^
      --payload "Comment=your comments"
  ```

------
#### [ PowerShell ]

  ```
  Send-SSMAutomationSignal `
      -AutomationExecutionId df325c6d-b1b1-4aa0-8003-6cb7338213c6 `
      -SignalType Approve `
      -Payload @{"Comment"="your comments"}
  ```

------

  There is no output if the command succeeds\.

**To deny an automation**
+ Run the following command to deny an automation\. Replace each *example resource placeholder* with your own information\.

------
#### [ Linux & macOS ]

  ```
  aws ssm send-automation-signal \
      --automation-execution-id "df325c6d-b1b1-4aa0-8003-6cb7338213c6" \
      --signal-type "Deny" \
      --payload "Comment=your comments"
  ```

------
#### [ Windows ]

  ```
  aws ssm send-automation-signal ^
      --automation-execution-id "df325c6d-b1b1-4aa0-8003-6cb7338213c6" ^
      --signal-type "Deny" ^
      --payload "Comment=your comments"
  ```

------
#### [ PowerShell ]

  ```
  Send-SSMAutomationSignal `
      -AutomationExecutionId df325c6d-b1b1-4aa0-8003-6cb7338213c6 `
      -SignalType Deny `
      -Payload @{"Comment"="your comments"}
  ```

------

  There is no output if the command succeeds\.