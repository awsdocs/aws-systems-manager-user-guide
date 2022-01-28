# Viewing overview information about an application<a name="application-manager-working-viewing-overview"></a>

In Application Manager, a component of AWS Systems Manager, the **Overview** tab displays a summary of Amazon CloudWatch alarms, operational work items \(OpsItems\), CloudWatch Application Insights, and runbook history\. Choose **View all** for any card to open the corresponding tab where you can view all application insights, alarms, OpsItems, or runbook history\.

**About Application Insights**  
CloudWatch Application Insights identifies and sets up key metrics, logs, and alarms across your application resources and technology stack\. Application Insights continuously monitors metrics and logs to detect and correlate anomalies and errors\. When the system detects errors or anomalies, Application Insights generates CloudWatch Events that you can use to set up notifications or take actions\. If you choose the **Edit configuration** button on the **Monitoring** tab, the system opens the CloudWatch Application Insights console\. For more information about Application Insights, see [What is Amazon CloudWatch Application Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/appinsights-what-is.html) in the *Amazon CloudWatch User Guide*\.

**Actions you can perform on this page**  
You can perform the following actions on this page:
+ In the **Alarms** section, choose a number to open the **Monitoring** tab where you can view more details about alarms of the chosen severity\.
+ In the **OpsItems** section, choose a severity to open the **OpsItems** tab where you can view all OpsItems of the chosen severity\.
+ In the **Runbooks** section, choose a runbook to open it in the Systems Manager **Documents** page where you can view more details about the document\.
+ If you enabled AWS Cost Explorer, the **Cost Explorer** section shows cost data for a specific application or application component\. You can enable this feature by choosing the **Go to Billing console** button and then choosing **Cost Explorer** in the Billing and Cost Management console\. By default, the data is filtered to the past three months\. For a non\-container application, if you choose the **View all** button in this section, Application Manager opens the **Resources** tab\. For container applications, the **View all** button opens the AWS Cost Explorer console\.
+ Choose a **View all** button to open the corresponding tab\. You can view all alarms, OpsItems, or runbook history entries for the application\.

**To open the **Overview** tab**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Application Manager**\.

1. In the **Applications** section, choose a category\. If you want to open an application you created manually in Application Manager, choose **Custom applications**\.

1. Choose the application in the list\. Application Manager opens the **Overview** tab\.