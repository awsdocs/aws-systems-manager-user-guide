# Querying an Inventory Collection<a name="sysman-inventory-query"></a>

After you collect inventory data, you can use the filter capabilities in Systems Manager to query a list of managed instances that meet certain filter criteria\. 

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To query instances based on inventory filters \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Inventory**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Inventory** in the navigation pane\.

1. In the **Filter by resource groups, tags or inventory types** section, choose the filter box\. A list of predefined filters appears\.

1. Choose an attribute to filter on\. For example, choose **AWS:Application**\. If prompted, choose a secondary attribute to filter\. For example, choose **AWS:Application\.Name**\. 

1. Choose a delimiter from the list\. For example, choose **Begin with**\. A text box appears in the filter\.

1. Type a value in the text box\. For example, type *Amazon* \(SSM Agent is named *Amazon SSM Agent*\)\. 

1. Press Enter\. The system returns a list of managed instances that include an application name that begins with the word *Amazon*\.

**To query instances based on inventory filters \(Amazon EC2 Systems Manager\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Shared Resources** in the navigation pane, and then choose **Managed Instances**\. 

1. Choose the **Inventory** tab\.

1. In the **Inventory Type** list, choose an attribute to filter on\. For example: **AWS:Application**\.

1. Choose the filter bar below the **Inventory Type** list to view a list of attributes on which to filter\. 

1. Choose a delimiter from the list\. For example, choose **begins\-with**\.

1. Type a value\. For example, type "ssm" and then choose the search icon at the left of the filter bar\. The system returns all relevant managed instances\.

**Note**  
You can combine multiple filters to refine your search\.