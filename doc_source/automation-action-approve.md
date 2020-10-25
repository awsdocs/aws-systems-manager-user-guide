# aws:approve – Pause an execution for manual approval<a name="automation-action-approve"></a>

Temporarily pauses an Automation execution until designated principals either approve or reject the action\. After the required number of approvals is reached, the Automation execution resumes\. You can insert the approval step any place in the mainSteps section of your Automation document\. 

**Note**  
The default timeout for this action is 7 days \(604800 seconds\)\. You can limit or extend the timeout by specifying the `timeoutSeconds` parameter for an aws:approve step\. If the automation step reaches the timeout value before receiving all required approval decisions, then the step and the automation stop running and return a status of Timed Out\.

In the following example, the aws:approve action temporarily pauses the Automation workflow until one approver either accepts or rejects the workflow\. Upon approval, the document runs a simple PowerShell command\. 

------
#### [ YAML ]

```
---
description: RunInstancesDemo1
schemaVersion: '0.3'
assumeRole: "{{ assumeRole }}"
parameters:
  assumeRole:
    type: String
  message:
    type: String
mainSteps:
- name: approve
  action: aws:approve
  timeoutSeconds: 1000
  onFailure: Abort
  inputs:
    NotificationArn: arn:aws:sns:us-east-2:12345678901:AutomationApproval
    Message: "{{ message }}"
    MinRequiredApprovals: 1
    Approvers:
    - arn:aws:iam::12345678901:user/AWS-User-1
- name: run
  action: aws:runCommand
  inputs:
    InstanceIds:
    - i-1a2b3c4d5e6f7g
    DocumentName: AWS-RunPowerShellScript
    Parameters:
      commands:
      - date
```

------
#### [ JSON ]

```
{
   "description":"RunInstancesDemo1",
   "schemaVersion":"0.3",
   "assumeRole":"{{ assumeRole }}",
   "parameters":{
      "assumeRole":{
         "type":"String"
      },
      "message":{
         "type":"String"
      }
   },
   "mainSteps":[
      {
         "name":"approve",
         "action":"aws:approve",
         "timeoutSeconds":1000,
         "onFailure":"Abort",
         "inputs":{
            "NotificationArn":"arn:aws:sns:us-east-2:12345678901:AutomationApproval",
            "Message":"{{ message }}",
            "MinRequiredApprovals":1,
            "Approvers":[
               "arn:aws:iam::12345678901:user/AWS-User-1"
            ]
         }
      },
      {
         "name":"run",
         "action":"aws:runCommand",
         "inputs":{
            "InstanceIds":[
               "i-1a2b3c4d5e6f7g"
            ],
            "DocumentName":"AWS-RunPowerShellScript",
            "Parameters":{
               "commands":[
                  "date"
               ]
            }
         }
      }
   ]
}
```

------

You can approve or deny Automations that are waiting for approval in the console\.

**To approve or deny waiting Automations**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Automation**\.

1. Choose the option next to an Automation with a status of **Waiting**\.  
![\[Accessing the Approve/Deny Automation page\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/automation-approve-action-aws.png)

1. Choose **Approve/Deny**\.

1. Review the details of the Automation\.

1. Choose either **Approve** or **Deny**, type an optional comment, and then choose **Submit**\.

**Input**

------
#### [ YAML ]

```
NotificationArn: arn:aws:sns:us-west-1:12345678901:Automation-ApprovalRequest
Message: Please approve this step of the Automation.
MinRequiredApprovals: 3
Approvers:
- IamUser1
- IamUser2
- arn:aws:iam::12345678901:user/IamUser3
- arn:aws:iam::12345678901:role/IamRole
```

------
#### [ JSON ]

```
{
   "NotificationArn":"arn:aws:sns:us-west-1:12345678901:Automation-ApprovalRequest",
   "Message":"Please approve this step of the Automation.",
   "MinRequiredApprovals":3,
   "Approvers":[
      "IamUser1",
      "IamUser2",
      "arn:aws:iam::12345678901:user/IamUser3",
      "arn:aws:iam::12345678901:role/IamRole"
   ]
}
```

------

NotificationArn  
The ARN of an Amazon SNS topic for Automation approvals\. When you specify an `aws:approve` step in an Automation document, Automation sends a message to this topic letting principals know that they must either approve or reject an Automation step\. The title of the Amazon SNS topic must be prefixed with "Automation"\.  
Type: String  
Required: No

Message  
The information you want to include in the SNS topic when the approval request is sent\. The maximum message length is 4096 characters\.   
Type: String  
Required: No

MinRequiredApprovals  
The minimum number of approvals required to resume the Automation execution\. If you don't specify a value, the system defaults to one\. The value for this parameter must be a positive number\. The value for this parameter can't exceed the number of approvers defined by the `Approvers` parameter\.   
Type: Integer  
Required: No

Approvers  
A list of AWS authenticated principals who are able to either approve or reject the action\. The maximum number of approvers is 10\. You can specify principals by using any of the following formats:  
+ An AWS Identity and Access Management \(IAM\) user name
+ An IAM user ARN
+ An IAM role ARN
+ An IAM assume role user ARN
Type: StringList  
Required: Yes

**Output**

ApprovalStatus  
The approval status of the step\. The status can be one of the following: Approved, Rejected, or Waiting\. Waiting means that Automation is waiting for input from approvers\.  
Type: String

ApproverDecisions  
A JSON map that includes the approval decision of each approver\.  
Type: MapList