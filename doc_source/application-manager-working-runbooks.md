# Working with runbooks<a name="application-manager-working-runbooks"></a>

You can remediate issues with AWS resources from Application Manager, a capability of AWS Systems Manager, by using Systems Manager Automation runbooks\. When you choose **Start runbook** from an Application Manager application, the system displays a filtered list of runbooks based on the type of resources in your application\. When you choose the runbook you want to start, Systems Manager opens the **Execute automation document** page\. 

**Before you begin**  
Before you start a runbook from Application Manager, do the following:
+ Verify that you have the correct permissions for starting runbooks\. For more information, see [Setting up Automation](automation-setup.md)\. 
+ Review the Automation procedure documentation about starting runbooks\. For more information, see [Working with automations](automation-working.md)\.
+ If you intend to start runbooks on multiple resources at one time, review the documentation about using targets and rate controls\. For more information, see [Running automations that use targets and rate controls](automation-working-targets-and-rate-controls.md)\.

**To start a runbook from Application Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Application Manager**\.

1. In the **Applications** section, choose a category\. If you want to open an application you created manually in Application Manager, choose **Custom applications**\.

1. Choose the application in the list\. Application Manager opens the **Overview** tab\.

1. Choose **Start runbook**\. Application Manager opens the **Execute automation document** page in a new tab\. For information about the options in the **Execute automation document** page, see [Working with automations](automation-working.md)\.