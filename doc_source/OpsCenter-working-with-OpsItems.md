# Working with OpsItems<a name="OpsCenter-working-with-OpsItems"></a>

This section describes how to configure the options available in an AWS Systems Manager OpsItem\. For information about creating OpsItems, see [Creating OpsItems](OpsCenter-creating-OpsItems.md)\. 

**Topics**
+ [Working with related resources](#OpsCenter-working-with-OpsItems-related-resources)
+ [Viewing other OpsItems for a specific resource](#OpsCenter-working-other-OpsItems-for-a-resource)
+ [Editing OpsItem details](#OpsCenter-working-with-OpsItems-editing-details)
+ [Working with similar and related OpsItems](#OpsCenter-working-with-OpsItems-similar)
+ [Working with operational data](#OpsCenter-working-operational-data)

## Working with related resources<a name="OpsCenter-working-with-OpsItems-related-resources"></a>

A *related resource* is the impacted AWS resource that needs to be investigated or that initiated an Amazon EventBridge event\. Each OpsItem has a **Related resources** section\. If EventBridge creates the OpsItem, then the system automatically populates the OpsItem with the Amazon Resource Name \(ARN\) of the resource\. You can also manually specify ARNs of related resources\. For some ARN types, AWS Systems Manager OpsCenter automatically creates a deep link that displays details about the resource without having to visit other console pages to view that information\. For example, you can specify the ARN of an Amazon EC2 instance\. In OpsCenter, you can then view all of the details that Amazon EC2 provides about that instance\. To view a list of resource types that automatically create deep links to related resource, see [Supported resources reference](OpsCenter-related-resources-reference.md)\.

**Note**  
You can manually add the ARNs of additional related resources\. Each OpsItem can list a maximum of 100 related resource ARNs\.

**To view and add related resources**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose the **OpsItems** tab\.

1. Choose an OpsItem ID\.  
![\[A new OpsItem on the OpsCenter Overview page\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_scenario_1.png)

1. To view information about the impacted resource, choose the **Related resources details** tab\.  
![\[Viewing the Related Resources tab for an OpsItem\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_scenario_1_5.png)

   This tab displays information about the resource from several AWS services\. Expand the **Resource details** section to view information about this resource as provided by the AWS service that hosts it\. You can also toggle through other related resources associated with this OpsItem by using the **Related resources** list\.

1. To add additional related resources, choose the **Overview** tab\.

1. In the **Related resources** section, choose **Add**\.

1. For **Resource type**, choose a resource from the list\.

1. For **Resource ID**, enter either the ID or the Amazon Resource Name \(ARN\)\. The type of information you choose depends o the resource you chose in the previous step\.

## Viewing other OpsItems for a specific resource<a name="OpsCenter-working-other-OpsItems-for-a-resource"></a>

To help you investigate issues and provide context for a problem, you can view a list of OpsItems for a specific AWS resource\. The list displays the status, severity, and title of each OpsItem\. The list also includes deep links to each OpsItem so you can quickly open them to view more information\.

**To view a list of OpsItems for a specific resource**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose an OpsItem ID to open the details page\.

1. Choose the **Related resource details** tab\.

1. Expand **Other OpsItems for this resource**\.

## Editing OpsItem details<a name="OpsCenter-working-with-OpsItems-editing-details"></a>

The **OpsItem details** section includes information about the OpsItem, including the description, title, source, OpsItem ID, and the status, to name a few\.

![\[Viewing details in the console about an OpsItem\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_scenario_3.png)

For OpsItems that were created automatically, Amazon EventBridge populates the **Title**, **Source**, and **Description** fields\. You can edit the **Title** and the **Description** fields, but you can't edit the **Source** field\.

**About OpsItem Status**  
When you edit an OpsItem, you can specify a status\. The **Status** list includes the following options:


****  

| Status | Details | 
| --- | --- | 
|  Open  |  Active in the system, but *not* being worked on by an engineer\.  | 
|  In progress  |  Active in the system and being worked on by an engineer\.   | 
|  Resolved  |  Not active in the system, but available in Search and when using the **Resolved** filter on the OpsItem **Overview** page\. You can edit a resolved OpsItem to change the status to **Open** or **In progress**\.  | 

You can view reports about OpsItem statuses on the **Summary** tab\. For more information, see [Viewing OpsCenter summary reports](OpsCenter-reports.md)\.

**About OpsItem Priority**  
When you edit an OpsItem, you can choose a priority for that OpsItem by choosing a value between 1 and 5\. We recommend that your organization determine what each priority level means and a corresponding service level agreement for each\.

**About the Notifications Field**  
When you edit an OpsItem, you can specify the ARN of an Amazon SNS topic in the **Notifications** field\. By specifying an ARN, you ensure that all stakeholders receive a notification when the OpsItem is edited, including a status change\. You might find it helpful to create different ARNs for notifications about different types of AWS resources or different environments\. The Amazon SNS topic must exist in the same AWS Region as the OpsItems\. If they're in different Regions, the system returns an error\. For more information, see the [https://docs.aws.amazon.com/sns/latest/dg/](https://docs.aws.amazon.com/sns/latest/dg/)\.

**Important**  
The Amazon SNS topic must exist in the same AWS Region as the OpsItem\. If they're in different Regions, the system returns an error\.

**To edit OpsItem details**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose an OpsItem ID to open the details page\.

1. In the **OpsItem details** section, choose **Edit**\.

1. Edit the details of the OpsItem according to the requirements and guidelines specified by your organization\.

1. When you're finished, choose **Save**\.

## Working with similar and related OpsItems<a name="OpsCenter-working-with-OpsItems-similar"></a>

The **Similar OpsItems** and **Related OpsItems** features are designed to help you investigate operations issues while providing context about the scope of an issue\.

The **Similar OpsItems** feature is a system\-generated list of OpsItems that might be related or of interest to you\. To generate the list, the system scans the titles and descriptions of all OpsItems and returns OpsItems that use similar words\.

![\[Viewing similar OpsItems.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_scenario_5.png)

In the **Related OpsItems** section, you can specify a maximum of 10 IDs for other OpsItems that are related to the current OpsItem\. OpsItems can be related in different ways, including a parent\-child relationship between OpsItems, a root cause, or a duplicate\. 

![\[Viewing related OpsItems.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_scenario_4.png)

To help you organize OpsItems, you can associate one OpsItem with another so that it is displayed in the **Related OpsItems** section\.

**To add a related OpsItem**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose an OpsItem ID to open the details page\.

1. In the **Related OpsItem** section, choose **Add**\.

1. For **OpsItem ID**, specify an ID\.

1. Choose **Add**\.

## Working with operational data<a name="OpsCenter-working-operational-data"></a>

Operational data is custom data that provides useful reference details about the OpsItem\. For example, you can specify log files, error strings, license keys, troubleshooting tips, or other relevant data\. You enter operational data as key\-value pairs\. The key has a maximum length of 128 characters\. The value has a maximum size of 20 KB\. You can enter multiple key\-value pairs of operational data\.

**Important**  
Operational data keys *can't* begin with the following: `amazon`, `aws`, `amzn`, `ssm`, `/amazon`, `/aws`, `/amzn`, `/ssm`\.

![\[Viewing operational data for an OpsItem.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_scenario_7.png)

You can choose to make the data searchable by other users in the account or you can restrict search access\. Searchable data means that all users with access to the OpsItem Overview page \(as provided by the [DescribeOpsItems](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeOpsItems.html) API operation\) can view and search on the specified data\. Operational data that isn't searchable is only viewable by users who have access to the OpsItem \(as provided by the [GetOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetOpsItem.html) API operation\)\.

**To add operational data to an OpsItem**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose an OpsItem ID to open the details page\.

1. Expand either **Operational data**\.

1. If no operational data exists for the OpsItem, then choose **Add**\. If operational data already exists for the OpsItem, choose **Manage**\.

1. For **Key**, specify a word or words to help users understand the purpose of the data\. The key can't begin with the following: `amazon`, `aws`, `amzn`, `ssm`, `/amazon`, `/aws`, `/amzn`, `/ssm`\.

1. For **Value**, specify the data\.

1. Choose **Save**\.

After you create operational data, you can edit the key and the value, remove the operational data, or add additional key\-value pairs by choosing **Manage**\. 

**Note**  
You can filter OpsItems by using the **Operational data** operator on the OpsItems page\. In the Search box, choose **Operational data**, and then enter a key\-value pair in JSON\. You must enter the key\-value pair by using the following format: `{"key":"key_name","value":"a_value"}`