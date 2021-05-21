# Creating change templates using Editor<a name="change-templates-custom-editor"></a>

Use the steps in this topic to configure a change template in Change Manager, a capability of AWS Systems Manager, by entering JSON or YAML instead of using the console controls\.

**To create a change template using Editor**

1. In the navigation pane, choose **Change Manager**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Change Manager**\.

1. Choose **Create template**\.

1. For **Name**, enter a name for the template that makes its purpose easy to identify, such as **RestartEC2LinuxInstance**\.

1. Above **Change template details**, choose **Editor**\.

1. In the **Document editor** section, choose **Edit**, and then enter the JSON or YAML content for your change template\. 

   The following is an example\.
**Note**  
This example demonstrates two levels of approvals\. You can specify up to five levels of approvals, but only one level is required\.

------
#### [ YAML ]

   ```
   description: >-
     This change template demonstrates the feature set available for creating
     change templates for Change Manager. This template starts a Runbook workflow
     for the Automation runbook called AWS-HelloWorld.
   templateInformation: >
     ### Document Name: HelloWorldChangeTemplate
   
     ## What does this document do?
   
     This change template demonstrates the feature set available for creating
     change templates for Change Manager. This template starts a Runbook workflow
     for the Automation runbook called AWS-HelloWorld.
   
     ## Input Parameters
   
     * ApproverSnsTopicArn: (Required) Amazon Simple Notification Service ARN for
     approvers.
   
     * Approver: (Required) The name of the approver to send this request to.
   
     * ApproverType: (Required) The type of reviewer.
       * Allowed Values: IamUser, IamGroup, IamRole, SSOGroup, SSOUser
   
     ## Output Parameters
   
     This document has no outputs
   schemaVersion: '0.3'
   parameters:
     ApproverSnsTopicArn:
       type: String
       description: Amazon Simple Notification Service ARN for approvers.
     Approver:
       type: String
       description: IAM approver
     ApproverType:
       type: String
       description: >-
         Approver types for the request. Allowed values include IamUser, IamGroup,
         IamRole, SSOGroup, and SSOUser.
   executableRunBooks:
     - name: AWS-HelloWorld
       version: '1'
   emergencyChange: false
   mainSteps:
     - name: ApproveAction1
       action: 'aws:approve'
       timeoutSeconds: 3600
       inputs:
         Message: >-
           A sample change request has been submitted for your review in Change
           Manager. You can approve or reject this request.
         EnhancedApprovals:
           NotificationArn: '{{ ApproverSnsTopicArn }}'
           Approvers:
             - approver: '{{ Approver }}'
               type: '{{ ApproverType }}'
               minRequiredApprovals: 1
     - name: ApproveAction2
       action: 'aws:approve'
       timeoutSeconds: 3600
       inputs:
         Message: >-
           A sample change request has been submitted for your review in Change
           Manager. You can approve or reject this request.
         EnhancedApprovals:
           NotificationArn: '{{ ApproverSnsTopicArn }}'
           Approvers:
             - approver: '{{ Approver }}'
               type: '{{ ApproverType }}'
               minRequiredApprovals: 1
   ```

------
#### [ JSON ]

   ```
   {
      "description": "This change template demonstrates the feature set available for creating
     change templates for Change Manager. This template starts a Runbook workflow
     for the Automation runbook called AWS-HelloWorld",
      "templateInformation": "### Document Name: HelloWorldChangeTemplate\n\n
       ## What does this document do?\n
       This change template demonstrates the feature set available for creating change templates for Change Manager. 
       This template starts a Runbook workflow for the Automation runbook called AWS-HelloWorld.\n\n
       ## Input Parameters\n* ApproverSnsTopicArn: (Required) Amazon Simple Notification Service ARN for approvers.\n
       * Approver: (Required) The name of the approver to send this request to.\n
       * ApproverType: (Required) The type of reviewer.  * Allowed Values: IamUser, IamGroup, IamRole, SSOGroup, SSOUser\n\n
       ## Output Parameters\nThis document has no outputs\n",
      "schemaVersion": "0.3",
      "parameters": {
         "ApproverSnsTopicArn": {
            "type": "String",
            "description": "Amazon Simple Notification Service ARN for approvers."
         },
         "Approver": {
            "type": "String",
            "description": "IAM approver"
         },
         "ApproverType": {
            "type": "String",
            "description": "Approver types for the request. Allowed values include IamUser, IamGroup, IamRole, SSOGroup, and SSOUser."
         }
      },
      "executableRunBooks": [
         {
            "name": "AWS-HelloWorld",
            "version": "1"
         }
      ],
      "emergencyChange": false,
      "mainSteps": [
         {
            "name": "ApproveAction1",
            "action": "aws:approve",
            "timeoutSeconds": 3600,
            "inputs": {
               "Message": "A sample change request has been submitted for your review in Change Manager. You can approve or reject this request.",
               "EnhancedApprovals": {
                  "NotificationArn": "{{ ApproverSnsTopicArn }}",
                  "Approvers": [
                     {
                        "approver": "{{ Approver }}",
                        "type": "{{ ApproverType }}",
                        "minRequiredApprovals": 1
                     }
                  ]
               }
            }
         },
           {
            "name": "ApproveAction2",
            "action": "aws:approve",
            "timeoutSeconds": 3600,
            "inputs": {
               "Message": "A sample change request has been submitted for your review in Change Manager. You can approve or reject this request.",
               "EnhancedApprovals": {
                  "NotificationArn": "{{ ApproverSnsTopicArn }}",
                  "Approvers": [
                     {
                        "approver": "{{ Approver }}",
                        "type": "{{ ApproverType }}",
                        "minRequiredApprovals": 1
                     
                     }
                  ]
               }
            }
         }
      ]
   }
   ```

------

1. Choose **Save and preview**\.

1. Review the details of the change template you are creating\.

   If you want to make change to the change template before submitting it for review, choose **Actions, Edit**\.

   If you are satisfied with the contents of the change template, choose **Submit for review**\. The users in your organization or account who have been specified as template reviewers on the **Settings** tab in Change Manager are notified that a new change template is pending their review\. 

   If an Amazon Simple Notification Service \(Amazon SNS\) topic has been specified for change templates, notifications are sent when the change template is rejected or approved\. If you don't receive notifications related to this change template, you can return to Change Manager later to check on its status\.