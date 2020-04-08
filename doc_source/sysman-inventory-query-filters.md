# Querying an inventory collection by using filters<a name="sysman-inventory-query-filters"></a>

After you collect inventory data, you can use the filter capabilities in Systems Manager to query a list of managed instances that meet certain filter criteria\. 

**To query instances based on inventory filters**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Inventory**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Inventory** in the navigation pane\.

1. In the **Filter by resource groups, tags or inventory types** section, choose the filter box\. A list of predefined filters appears\.

1. Choose an attribute to filter on\. For example, choose **AWS:Application**\. If prompted, choose a secondary attribute to filter\. For example, choose **AWS:Application\.Name**\. 

1. Choose a delimiter from the list\. For example, choose **Begin with**\. A text box appears in the filter\.

1. Type a value in the text box\. For example, type *Amazon* \(SSM Agent is named *Amazon SSM Agent*\)\. 

1. Press Enter\. The system returns a list of managed instances that include an application name that begins with the word *Amazon*\.

**Note**  
You can combine multiple filters to refine your search\.