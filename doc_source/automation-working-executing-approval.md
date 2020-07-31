# Running an automation with approvers<a name="automation-working-executing-approval"></a>

The following procedures describe how to use the AWS Systems Manager console, AWS Command Line Interface \(AWS CLI\), and AWS Tools for Windows PowerShell to run an AWS Systems Manager Automation workflow with approvals using simple execution\. The workflow uses the automation action `aws:approve`, which temporarily pauses the automation until the designated principals either approve or deny the action\. The automation runs in the context of the current AWS Identity and Access Management \(IAM\) user\. This means that you don't need to configure additional IAM permissions as long as you have permission to run the Automation document, and any actions called by the document\. If you have administrator permissions in IAM, then you already have permission to run this automation\.

**Note**  
For information about how to run an automation that uses an IAM service role or more advanced forms of delegated administration, see [Running automations by using different security models](automation-walk-security.md)\. 

**Before You Begin**  
In addition to the standard inputs required by the Automation document, the `aws:approve` action requires the following two parameters: 
+ A list of approvers\. The list of approvers must contain at least one approver in the form of an IAM user or a user ARN\. If multiple approvers are provided, a corresponding minimum approval count must also be specified within the Automation document\. 
+ An Amazon Simple Notification Service \(Amazon SNS\) topic ARN\. The Amazon SNS topic name must start with `Automation`\.

This procedure assumes that you have already created an Amazon SNS topic, which is required to deliver the approval request\. For information, see [Create a Topic](https://docs.aws.amazon.com/sns/latest/dg/sns-getting-started.html#CreateTopic) in the *Amazon Simple Notification Service Developer Guide*\.

## Running an automation with approvers \(console\)<a name="automation-working-executing-approval-console"></a>

**To run an automation with approvers**

The following procedure describes how to use the Systems Manager console to run an automation with approvers\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**, and then choose **Execute automation**\.

1. In the **Automation document** list, choose a document\. Choose one or more options in the **Document categories** pane to filter SSM documents according to their purpose\. To view a document that you own, choose the **Owned by me** tab\. To view a document that is shared with your account, choose the **Shared with me** tab\. To view all documents, choose the **All documents** tab\.
**Note**  
You can view information about a document by choosing the document name\.

1. In the **Document details** section, verify that **Document version** is set to the version that you want to run\. The system includes the following version options: 
   + **Default version at runtime**: Choose this option if the Automation document is updated periodically and a new default version is assigned\.
   + **Latest version at runtime**: Choose this option if the Automation document is updated periodically, and you want to run the version that was most recently updated\.
   + **1 \(Default\)**: Choose this option to run the first version of the document, which is the default\.

1. Choose **Next**\.

1. On the **Execute automation document** page, choose **Simple execution**\.

1. In the **Input parameters** section, specify the required input parameters\.

   For example, if you chose the **AWS\-StartEC2InstanceWithApproval** document, then you must specify or choose instance IDs for the **InstanceId** parameter\. 

1. In the **Approvers** section, specify the IAM users or user ARNs of approvers for the automation action\.

1. In the **SNSTopicARN** section, specify the SNS topic ARN to use for sending approval notification\. The SNS topic name must start with **Automation**\.

1. Optionally, you can choose an IAM service role from the **AutomationAssumeRole** list\.

1. Choose **Execute automation**\. 

The specified approver receives an Amazon SNS notification with details to approve or reject the automation\. This approval action is valid for 7 days from the date of issue and can be issued using the Systems Manager console or the AWS Command Line Interface \(AWS CLI\)\.

If you chose to approve the automation, the workflow continues to run the steps included in the specified Automation document\. The console displays the status of the automation\. If the automation fails to run, see [Troubleshooting Systems Manager Automation](automation-troubleshooting.md)\.

**To approve or deny an automation**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**, and then select the automation that was run in the previous procedure\.

1. Choose **Actions** and then choose **Approve/Deny**\.

1. Choose to **Approve** or **Deny** and optionally provide a comment\.

1. Choose **Submit**\.

## Running an automation with approvers \(command line\)<a name="automation-working-executing-approval-commandline"></a>

The following procedure describes how to use the AWS CLI \(on Linux or Windows\) or AWS Tools for PowerShell to run an automation with approvers\.

**To run an automation with approvers**

1. Install and configure the AWS CLI or the AWS Tools for PowerShell, if you have not already\.

   For information, see [Install or upgrade AWS command line tools](getting-started-cli.md)\.

1. Use the following command to run an automation with approvers\. In the document name section, specify an Automation document that includes the automation action, `aws:approve`\.

   For `Approvers`, specify the IAM users or user ARNs of approvers for the action\. For `SNSTopic`, specify the SNS topic ARN to use to send approval notification\. The SNS topic name must start with **Automation**\.
**Note**  
The specific names of the parameter values for approvers and the SNS topic depend on the values specified within the document you choose\. 

------
#### [ Linux ]

   ```
   aws ssm start-automation-execution \
       --document-name "AWS-StartEC2InstanceWithApproval" \
       --parameters "InstanceId=i-1234567890abcdef0,Approvers=arn:aws:iam::123456789012:role/Administrator,SNSTopicArn=arn:aws:sns:us-east-1:123456789012:AutomationApproval"
   ```

------
#### [ Windows ]

   ```
   aws ssm start-automation-execution ^
       --document-name "AWS-StartEC2InstanceWithApproval" ^
       --parameters "InstanceId=i-1234567890abcdef0,Approvers=arn:aws:iam::123456789012:role/Administrator,SNSTopicArn=arn:aws:sns:us-east-1:123456789012:AutomationApproval"
   ```

------
#### [ PowerShell ]

   ```
   Start-SSMAutomationExecution `
       -DocumentName AWS-StartEC2InstanceWithApproval `
       -Parameters @{
           "InstanceId"="i-1234567890abcdef0"
           "Approvers"="arn:aws:iam::123456789012:role/Administrator"
           "SNSTopicArn"="arn:aws:sns:us-east-1:123456789012:AutomationApproval"
       }
   ```

------

   The system returns information like the following\.

------
#### [ Linux ]

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
   462fa82a-7fff-430a-8490-0123456789ab
   ```

------

**To approve an automation**
+ Run the following command to approve an automation\.

------
#### [ Linux ]

  ```
  aws ssm send-automation-signal \
      --automation-execution-id "4105a4fc-f944-11e6-9d32-0123456789ab" \
      --signal-type "Approve" \
      --payload "Comment=Replace_This_With_Approve_Comment"
  ```

------
#### [ Windows ]

  ```
  aws ssm send-automation-signal ^
      --automation-execution-id "4105a4fc-f944-11e6-9d32-0123456789ab" ^
      --signal-type "Approve" ^
      --payload "Comment=Replace_This_With_Approve_Comment"
  ```

------
#### [ PowerShell ]

  ```
  Send-SSMAutomationSignal `
      -AutomationExecutionId 462fa82a-7fff-430a-8490-0123456789ab `
      -SignalType Approve `
      -Payload @{"Comment"="Replace_This_With_Approval_Comment"}
  ```

------

  There is no output if the command succeeds\.

**To deny an automation**
+ Run the following command to deny an automation\.

------
#### [ Linux ]

  ```
  aws ssm send-automation-signal \
      --automation-execution-id "4105a4fc-f944-11e6-9d32-0123456789ab" \
      --signal-type "Deny" \
      --payload "Comment=Replace_This_With_Deny_Comment"
  ```

------
#### [ Windows ]

  ```
  aws ssm send-automation-signal ^
      --automation-execution-id "4105a4fc-f944-11e6-9d32-0123456789ab" ^
      --signal-type "Deny" ^
      --payload "Comment=Replace_This_With_Deny_Comment"
  ```

------
#### [ PowerShell ]

  ```
  Send-SSMAutomationSignal `
      -AutomationExecutionId 462fa82a-7fff-430a-8490-0123456789ab `
      -SignalType Deny `
      -Payload @{"Comment"="Replace_This_With_Deny_Comment"}
  ```

------

  There is no output if the command succeeds\.