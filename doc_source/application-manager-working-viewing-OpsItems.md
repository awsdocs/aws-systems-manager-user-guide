# Viewing OpsItems for an application<a name="application-manager-working-viewing-OpsItems"></a>

The **OpsItems** tab displays operational work items \(OpsItems\) for resources in the selected application\. You can configure AWS Systems Manager OpsCenter to automatically create OpsItems from Amazon CloudWatch alarms and Amazon EventBridge events\. You can also manually create OpsItems\.

**Actions you can perform on this tab**  
You can perform the following actions on this page:
+ Filter the list of OpsItems by using the search field\. You can filter by OpsItem name, ID, source ID, or severity\. You can also filter the list based on status\. OpsItems support the following statuses: Open, In progress, Open and In progress, Resolved, or All\.
+ Change the status of an OpsItem by choosing the option button beside it and then choosing an option in the **Set status** menu\. 
+ Open Systems Manager OpsCenter to create an OpsItem by choosing **Create OpsItem**\.

**To open the **OpsItems** tab**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Application Manager**\.

1. In the **Applications** section, choose a category\. If you want to open an application you created manually in Application Manager, choose **Custom applications**\.

1. Choose the application in the list\. Application Manager opens the **Overview** tab\.

1. Choose the **OpsItems** tab\.