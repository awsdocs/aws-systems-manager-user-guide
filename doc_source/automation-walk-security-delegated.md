# Running an automation by using delegated administration<a name="automation-walk-security-delegated"></a>

When you run an automation, by default, the automation runs in the context of the AWS Identity and Access Management \(IAM\) user who initiated the execution\. This means, for example, if your IAM user account has administrator permissions, then the automation runs with administrator permissions and full access to the resources being configured by the automation\. As a security best practice, we recommend that you run automation by using an IAM service role \(also called an *assumed* role\) that is configured with the AmazonSSMAutomationRole managed policy\. Using an IAM service role to run automation is called *delegated administration*\.

When you use a service role, the automation is allowed to run against the AWS resources, but the user who ran the automation has restricted access \(or no access\) to those resources\. For example, you can configure a service role and use it with Automation to restart one or more EC2 instances\. The automation restarts the instances, but the service role does not give the user permission to access those instances\.

You can specify a service role at runtime when you run an automation, or you can create custom Automation documents and specify the service role directly in the document\. If you specify a service role, either at runtime or in an Automation document, then the service runs in the context of the specified service role\. If you don't specify a service role, then the system creates a temporary session in the context of the user and runs the automation\.

**Note**  
You must specify a service role for automation that you expect to run longer than 12 hours\. If you start a long\-running automation in the context of a user, the user's temporary session expires after 12 hours\.

Delegated administration ensures elevated security and control of your AWS resources\. It also enables an enhanced auditing experience because actions are being performed against your resources by a central service role instead of multiple IAM accounts\.

To properly illustrate how delegated administration can work in an organization, this topic describes the following tasks as though these tasks were performed by three different people in an organization:
+ Create a test IAM user account called AutomationRestrictedOperator \(Administrator\)\.
+ Create an IAM service role for Automation \(Administrator\)\.
+ Create a simple Automation document \(based on a preexisting Automation document\) that specifies the service role \(SSM Document Author\)\.
+ Run the automation as the test user \(Restricted Operator\)\.

In some organizations, all three of these tasks are performed by the same person, but identifying the different roles here shows how delegated administration enables enhanced security in complex organizations\.

**Important**  
As a security best practice, we recommend that you always use a service role to run automations, even if you are an administrator who performs all of these tasks\.

The procedures in this section link to topics in other AWS guides or other Systems Manager topics\. We recommend that you open links to other topics in a new tab in your web browser so you don't lose your place in this topic\.

**Topics**
+ [Create a test user account](#automation-walk-security-operator)
+ [Create an IAM service role for Automation](#automation-walk-security-service-role)
+ [Create a custom Automation document](#automation-walk-security-document)
+ [Run the custom Automation document](#automation-walk-security-execute)

## Create a test user account<a name="automation-walk-security-operator"></a>

This section describes how to create an IAM test user account with restricted permissions\. The permissions set allows the user to run automations, but the user doesn't have access to the AWS resources targeted by Automation\. The operator can also view the results of the automations\. You start by creating the custom IAM permissions policy, and then you create the user account and assign permissions to it\. 

**Create an IAM test user**

1. Create a permissions policy named OperatorRestrictedPermissions\. For information about how to create a new IAM permissions policy, see [Create an IAM Policy \(Console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-start) in the IAM User Guide\. Create the policy on the JSON tab, and specify the following permissions set\.

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
            "Effect":"Allow",
            "Action":[
               "ssm:DescribeAutomationExecutions",
               "ssm:DescribeAutomationStepExecutions",
               "ssm:DescribeDocument",
               "ssm:GetAutomationExecution",
               "ssm:GetDocument",
               "ssm:ListDocuments",
               "ssm:ListDocumentVersions",
               "ssm:StartAutomationExecution"
            ],
            "Resource":"*"
         }
      ]
   }
   ```

1. Create a new IAM user account named AutomationRestrictedOperator\. For information about how to create a new IAM user, see [Creating IAM Users \(Console\) ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html#id_users_create_console) in the IAM User Guide\. When prompted, choose **Attach existing policies directly**, and choose the policy you just created\.

1. Note the user name, password, and the **Console login link**\. You will log into this account later in this topic\.

## Create an IAM service role for Automation<a name="automation-walk-security-service-role"></a>

The following procedure links to other topics to help you create the service role and to configure Automation to trust this role\.

**To create the service role and enable Automation to trust it**

1. Create the Automation service role\. For information, see [Task 1: Create a service role for Automation](automation-permissions.md#automation-role)\.

1. Note the service role Amazon Resource Name \(ARN\)\. You will specify this ARN in the next procedure\.

## Create a custom Automation document<a name="automation-walk-security-document"></a>

This section describes how to create a custom Automation document that restarts EC2 instances\. AWS provides a default SSM document for restarting instances called AWS\-RestartEC2Instance\. The following procedure copies the content of that document to show you how to enter the service role in a document when you create your own\. By specifying the service role directly in the document, the user running the document does not require iam:PassRole permissions\. Without iam:PassRole permissions, the user can't use the service role elsewhere in AWS\.

**To create a custom Automation document**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose **Create document**\. 

1. In the **Name** field, type a name for the document, such as Restart\-EC2InstanceDemo\.

1. In the **Document type** list, choose **Automation document**\. 

1. In the **Content** section, choose **JSON**, and then paste the following content\. Replace **AssumeRoleARN** with the ARN of the service role you created in the previous procedure\.

   ```
   {
     "description": "Restart EC2 instances(s)",
     "schemaVersion": "0.3",
     "assumeRole": "AssumeRoleARN",
     "parameters": {
       "InstanceId": {
         "type": "StringList",
         "description": "(Required) EC2 Instance to restart"
       }
     },
     "mainSteps": [
       {
         "name": "stopInstances",
         "action": "aws:changeInstanceState",
         "inputs": {
           "InstanceIds": "{{ InstanceId }}",
           "DesiredState": "stopped"
         }
       },
       {
         "name": "startInstances",
         "action": "aws:changeInstanceState",
         "inputs": {
           "InstanceIds": "{{ InstanceId }}",
           "DesiredState": "running"
         }
       }
     ]
   }
   ```

1. Choose **Create document**\. 

## Run the custom Automation document<a name="automation-walk-security-execute"></a>

The following procedure describes how to run the document you just created using the restricted operator role you created earlier in this topic\. The user can run the document you created earlier because their IAM account permissions enable them to see and run the document\. The user can't, however, log on to the instances that you will restart with this automation\.

1. In the [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/), copy the instance IDs for one or more instances that you want to restart by using the following automation\.

1. Sign out of the AWS Management Console, and then sign back in by using the test user account **Console login link** that you copied earlier\. 

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Automation**\.

1. Choose **Execute automation**\. 

1. Choose the custom Automation document you created earlier in this topic\. 

1. In the **Document details** section, verify that **Document version** is set to **1 \(Default\)**\. 

1. Choose **Next**\.

1. In the **Execution mode** section, choose **Simple execution**\.

1. In the **Input parameters** section, type one or more instance IDs that you want to restart, and then choose **Execute**\. 

**Execution details** describes the status of the automation\. Step 1 stops the instances\. Step 2 starts the instances\.