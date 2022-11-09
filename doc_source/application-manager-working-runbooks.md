# Working with runbooks in Application Manager<a name="application-manager-working-runbooks"></a>

You can remediate issues with AWS resources from Application Manager, a capability of AWS Systems Manager, by using Automation runbooks\. An Automation runbook defines the actions that Systems Manager performs on your managed instances and other AWS resources when an automation runs\. Automation is a capability of AWS Systems Manager\. A runbook contains one or more steps that run in sequential order\. Each step is built around a single action\. Output from one step can be used as input in a later step\. 

When you choose **Start runbook** from an Application Manager application or cluster, the system displays a filtered list of available runbooks based on the type of resources in your application or cluster\. When you choose the runbook you want to start, Systems Manager opens the **Execute automation document** page\. 

Application Manager includes the following enhancements for working with runbooks\.
+ If you choose the name of a resource in Application Manager and then choose **Execute runbook**, the system displays a filtered list of runbooks for that resource type\.
+ You can initiate an automation on all resources of the same type by choosing a runbook in the list and then choosing **Run for resources of same type**\. 

**Before you begin**  
Before you start a runbook from Application Manager, do the following:
+ Verify that you have the correct permissions for starting runbooks\. For more information, see [Setting up Automation](automation-setup.md)\. 
+ Review the Automation procedure documentation about starting runbooks\. For more information, see [Running automations](running-automations.md)\.

**To start a runbook from Application Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Application Manager**\.

1. In the **Applications** section, choose a category\. If you want to open an application you created manually in Application Manager, choose **Custom applications**\.

1. Choose the application in the list\. Application Manager opens the **Overview** tab\.

1. Choose **Start runbook**\. Application Manager opens the **Execute automation document** page in a new tab\. For information about the options in the **Execute automation document** page, see [Running automations](running-automations.md)\.