# QuickStart \#3: Use Delegated Administration to Enhance Automation Security<a name="automation-quickstart-delegated"></a>

When you run a AWS Systems Manager Automation, by default, the Automation runs in the context of the AWS Identity and Access Management \(IAM\) user who initiated the execution\. This means, for example, if your IAM user account has administrator permissions, then the Automation runs with administrator permissions and full access to the resources being configured by the Automation\. As a security best practice, we recommend that you run Automations by using an IAM service role \(also called an *assume* role\) that is configured with the AmazonSSMAutomationRole managed policy\. Using an IAM service role to run Automation is called *delegated administration*\.

When you use a service role, the Automation is allowed to run against the AWS resources, but the user who ran the Automation has restricted access \(or no access\) to those resources\. For example, you can configure a service role and use it with Automation to restart one or more Amazon EC2 instances\. The Automation restarts the instances, but the service role does not give the user permission to access those instances\.

You can specify a service role at runtime when you run an Automation, or you can create custom Automation documents and specify the service role directly in the document\. If you specify a service role, either at runtime or in an Automation document, then the service runs in the context of the specified service role\. If you don't specify a service role, then the system creates a temporary session in the context of the user and runs the Automation\.

**Note**  
You must specify a service role for Automations that you expect to run longer than 12 hours\. If you start a long\-running Automation in the context of a user, the user's temporary session expires after 12 hours\.

Delegated administration ensures higher security and control of your AWS resources\. It also enables an enhanced auditing experience because actions are being performed against your resources by a central service role instead of multiple IAM accounts\.

To properly illustrate how delegated administration can work in an organization, this topic walks you through the following tasks as though these tasks were performed by three different people in an organization:
+ Create a test IAM user account called AutomationRestrictedOperator \(Administrator\)
+ Create an IAM service role for Automation \(Administrator\)
+ Create a simple Automation document \(based on a preexisting Automation document\) that specifies the service role \(SSM Document Author\)
+ Run the Automation as the test user \(Restricted Operator\)

In some organizations, all three of these tasks are performed by the same person, but identifying the different roles here can help you understand how delegated administration enables enhanced security in complex organizations\.

**Important**  
As a security best practice, we recommend that you always use a service role to run Automations, even if you are an administrator who performs all of these tasks\.

The procedures in this section link to topics in other AWS guides or other Systems Manager topics\. We recommend that you open links to other topics in a new tab in your web browser so you don't lose your place in this topic\.

**Topics**
+ [Create a Test User Account](#automation-quickstart-delegated-operator)
+ [Create an IAM Service Role for Automation](#automation-quickstart-delegated-service-role)
+ [Create a custom Automation document](#automation-quickstart-delegated-document)
+ [Run the custom Automation Document](#automation-quickstart-delegated-execute)

## Create a Test User Account<a name="automation-quickstart-delegated-operator"></a>

This section describes how to create an IAM test user account with restricted permissions\. The permissions set allows the user to run Automations, but the user doesn't have access to the AWS resources targeted by the Automations\. The operator can also view the results of the Automations\. You start by creating the custom IAM permissions policy, and then you create the user account and assign permissions to it\. 

**Create an IAM Test User**

1. Create a permissions policy named OperatorRestrictedPermissions\. For information about how to create a new IAM permissions policy, see [Create an IAM Policy \(Console\)](http://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html#access_policies_create-start) in the IAM User Guide\. Create the policy on the JSON tab, and specify the following permissions set\.

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

1. Create a new IAM user account named AutomationRestrictedOperator\. For information about how to create a new IAM user, see [Creating IAM Users \(Console\) ](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html#id_users_create_console) in the IAM User Guide\. When prompted, choose **Attach existing policies directly**, and choose the policy you just created\.

1. Make a note of the user name, password, and the **Console login link**\. You will log into this account later in this topic\.

## Create an IAM Service Role for Automation<a name="automation-quickstart-delegated-service-role"></a>

The following procedure links to other topics to help you create the service role and to configure Automation to trust this role\.

**To create the service role and enable Automation to trust it**

1. Create the Automation service role\. For information, see [Task 1: Create a Service Role for Automation](automation-permissions.md#automation-role)\.

1. Make a note of the service role Amazon Resource Name \(ARN\)\. You will specify this ARN in the next procedure\.

1. Configure a trust policy so that Automation trusts the service role\. For more information, see [Task 2: Add a Trust Relationship for Automation](automation-permissions.md#automation-trust2)\.

## Create a custom Automation document<a name="automation-quickstart-delegated-document"></a>

This section describes how to create a custom Automation document that restarts Amazon EC2 instances\. AWS provides a default SSM document for restarting instances called AWS\-RestartEC2Instance\. The following procedure copies the content of that document for the purpose of showing you how to enter the service role in a document when you create your own\. By specifying the service directly in the document, the user executing the document does not require iam:PassRole permissions\. Without iam:PassRole permissions, the user can't use the service role elsewhere in AWS\.

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
     "assumeRole": "service role ARN",
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

## Run the custom Automation Document<a name="automation-quickstart-delegated-execute"></a>

The following procedure describes how to run the document you just created as the restricted operator you created earlier in this topic\. The user can run the document you created earlier because their IAM account permissions enable them to see and run the document\. The user can't, however, log on to the instances that you will restart with this Automation\.

1. In the [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/), copy the instance IDs for one or more instances that you want to restart by using the following Automation\.

1. Log out of the AWS Management Console, and then log back in by using the test user account **Console login link** that you copied earlier\. 

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Automation**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Automation**\.

1. Choose **Execute automation**\. 

1. Choose the custom Automation document you created earlier in this topic\. 

1. In the **Document details** section, verify that **Document version** is set to **1 \(Default\)**\. 

1. In the **Execution mode** section, choose **Execute the entire automation at once**\.

1. Leave the **Targets and Rate Control** option disabled\.

1. In the **Input parameters** section, type one or more instance IDs that you want to restart, and then choose **Execute automation**\. 

The **Execution details** describes the status of the Automation\. Step 1 stops the instances\. Step 2 restarts them\.