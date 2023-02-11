# Manage OpsItems<a name="OpsCenter-working-with-OpsItems"></a>

OpsCenter, a capability of AWS Systems Manager, tracks OpsItems from their creation to resolution\. If you set up OpsCenter for cross\-account administration, a delegated administrator or management account can manage OpsItems from their account\. For more information, see [\(Optional\) Setting up OpsCenter to centrally manage OpsItems across accounts](OpsCenter-getting-started-multiple-accounts.md)\. 

You can view and manage OpsItems by using the following pages in the Systems Manager console: 
+ **Summary** – Displays a count of open and in\-progress OpsItems, count of OpsItems by source and age, and operational insights\. You can filter OpsItems by source and OpsItems status\. 
+ **OpsItems** – Displays a list of OpsItems with multiple fields of information, such as title, ID, priority, description, the source of the OpsItem, and the date and time of last update\. Using this page, you can manually create OpsItems, configure sources, change the status of an OpsItem, and filter OpsItems by new incidents\. You can choose an OpsItem to display its **OpsItems details** page\. 
+ **OpsItem details** – Provides detailed insights and tools that you can use to manage an OpsItem\. The OpsItems details page has the following tabs: 
  + **Overview** – Displays related resources, runbooks that ran in the last 30 days, and a list of available runbooks that you can run\. You can also view similar OpsItems, add operational data, and add related OpsItems\.
  + **Related resource details** – Displays information about the resource from several AWS services\. Expand the **Resource details** section to view information about this resource as provided by the AWS service that hosts it\. You can also toggle through other related resources associated with this OpsItem by using the **Related resources** list\. 

For more information about how to manage OpsItems, see the following topics\.

**Topics**
+ [Viewing details of an OpsItem](OpsCenter-working-with-OpsItems-viewing-details.md)
+ [Editing an OpsItem](OpsCenter-working-with-OpsItems-editing-details.md)
+ [Adding related resources to an OpsItem](OpsCenter-working-with-OpsItems-adding-related-resources.md)
+ [Adding related OpsItems to an OpsItem](OpsCenter-working-with-OpsItems-adding-related-OpsItems.md)
+ [Adding operational data to an OpsItem](OpsCenter-working-with-OpsItems-adding-operational-data.md)
+ [Creating an incident for an OpsItem](OpsCenter-working-with-OpsItems-create-an-incident.md)
+ [Managing duplicate OpsItems](OpsCenter-working-deduplication.md)
+ [Analyzing operational insights to reduce duplicate OpsItems](OpsCenter-working-operational-insights.md)
+ [Viewing OpsCenter logs and reports](OpsCenter-logging-auditing.md)