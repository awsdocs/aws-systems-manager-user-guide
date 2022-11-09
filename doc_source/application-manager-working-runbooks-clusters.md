# Working with runbooks for clusters<a name="application-manager-working-runbooks-clusters"></a>

You can remediate issues with AWS resources from Application Manager, a capability of AWS Systems Manager, by using Systems Manager Automation runbooks\. When you choose **Start runbook** from an Application Manager cluster, the system displays a filtered list of runbooks based on the type of resources in your cluster\. When you choose the runbook you want to start, Systems Manager opens the **Execute automation document** page\. 

**Before you begin**  
Before you start a runbook from Application Manager, do the following:
+ Verify that you have the correct permissions for starting runbooks\. For more information, see [Setting up Automation](automation-setup.md)\. 
+ Review the Automation procedure documentation about starting runbooks\. For more information, see [Running automations](running-automations.md)\.
+ If you intend to start runbooks on multiple resources at one time, review the documentation about using targets and rate controls\. For more information, see [Run automations at scale](running-automations-scale.md)\.

**To start a runbook for clusters from Application Manager**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Application Manager**\.

1. In the **Container clusters** section, choose a container type\. 

1. Choose the cluster in the list\. Application Manager opens the **Overview** tab\.

1. On the **Runbooks** tab, choose **Start runbook**\. Application Manager opens the **Execute automation document** page in a new tab\. For information about the options in the **Execute automation document** page, see [Running automations](running-automations.md)\.