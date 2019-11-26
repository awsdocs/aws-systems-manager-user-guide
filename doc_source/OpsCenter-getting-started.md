# Getting Started with OpsCenter<a name="OpsCenter-getting-started"></a>

Set up for Systems Manager OpsCenter is integrated with set up for Systems Manager Explorer\. Explorer is a customizable operations dashboard that reports information about your AWS resources\. Explorer displays an aggregated view of operations data \(OpsData\) for your AWS accounts and across Regions\. In Explorer, OpsData includes metadata about your Amazon EC2 instances, patch compliance details, and operational work items \(OpsItems\)\. Explorer provides context about how OpsItems are distributed across your business units or applications, how they trend over time, and how they vary by category\. You can group and filter information in Explorer to focus on items that are relevant to you and that require action\. When you identify high priority issues, you can use Systems Manager OpsCenter to run Automation runbooks and quickly resolve those issues\. 

If you already set up OpsCenter, you still need to complete Integrated Setup to verify settings and options\. If you have not set up OpsCenter, then you can use Integrated Setup to get started with both capabilities\. For more information, see [Getting Started with Systems Manager Explorer and OpsCenter](Explorer-setup.md)\.

## \(Optional\) Create OpsItem Guidelines for Your Organization<a name="OpsCenter-getting-started-guidelines"></a>

We recommend that each organization create a simple set of guidelines that promote consistency when creating and editing OpsItems\. Guidelines make it easier for users to locate and resolve OpsItems\. The guidelines for your organization should define best practices when users enter information into the following OpsItem fields\.

**Note**  
Amazon CloudWatch Events populates the **Title**, **Source**, and **Description** fields of automatically generated OpsItems\. You can edit the **Title** and the **Description** fields, but you can't edit the **Source** field\.


****  

| Field | Description | 
| --- | --- | 
|  **Title**  |  Guidelines should encourage a consistent OpsItem naming experience\. For example, your guidelines might require that each title include information about the impacted resource, the status, the environment, and the name or the alias of the engineer actively working the issue, if applicable\. All OpsItems created by CloudWatch include a title that describes the event that caused the creation of the OpsItem, but you can edit these titles\.You can search OpsItems for *Title*:*contains*\. If your naming guidelines encourage consistent use of keywords, you improve your search results\.  | 
|  **Source**  |  Guidelines can include specifying IDs, software version numbers \(if applicable\) or other relevant data to help users identify the origin of the issue\. You can't edit the **Source** field after the OpsItem is created\.  | 
|  **Priority**  |  \(Optional\) Guidelines include determining the highest and lowest priority for your organization, and any service\-level agreements based on priority\. You can specify priority from 1 to 5\.  | 
|  **Description**  |  Guidelines should suggest how much detail about the issue to include and any steps \(if applicable\) for reproducing the issue\.   | 
|  **Notifications**  |  Guidelines should suggest which Amazon Simple Notification Service \(SNS\) topic Amazon Resource Name \(ARN\) to specify when creating or editing OpsItems\. Be aware that SNS notifications are region\-specific\. This means you must specify an ARN that is in the same AWS Region as the OpsItem\.  | 
|  **Related Resources**  |  Guidelines can include details about which Resources should or shouldn't have an ARN specified\. For supported AWS resource types, the ARN creates a deep link to details about the Resource\.   | 
|  **Operational data**  |  You can specify custom data for each OpsItem that provides context about the issue and other relevant data for future reference\. You can specify searchable custom data\. All users with access to the OpsItem Overview page can search for and view this data\. You can also specify private custom data that is only viewable by users who have access to this OpsItem\. Guidelines could specify structure and standards for key\-value pairs\. These key\-value pairs can describe operational data and resolution details, leading to improved search results\.  | 