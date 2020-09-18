# Working with OpsItems<a name="OpsCenter-working-with-OpsItems"></a>

This section describes how to configure the options available in an OpsItem\. For information about creating OpsItems, see [Creating OpsItems](OpsCenter-creating-OpsItems.md)\. 

**Topics**
+ [Working with related resources](#OpsCenter-working-with-OpsItems-related-resources)
+ [Editing OpsItem details](#OpsCenter-working-with-OpsItems-editing-details)
+ [Working with related and similar OpsItems](#OpsCenter-working-with-OpsItems-similar)
+ [Working with operational data](#OpsCenter-working-operational-data)
+ [Reducing duplicate OpsItems](#OpsCenter-working-deduplication)

## Working with related resources<a name="OpsCenter-working-with-OpsItems-related-resources"></a>

A related resource is the impacted resource \(the resource that needs to be investigated or the resource that triggered the Amazon EventBridge event that created the OpsItem\)\. Each OpsItem has a **Related resources** section\. If EventBridge creates the OpsItem, then the system automatically populates the OpsItem with the Amazon Resource Name \(ARN\) of the resource\. You can also manually specify ARNs of related resources\. For some ARN types, OpsCenter automatically creates a deep link that displays details about the resource without having to visit other console pages to view that information\. For example, you can specify the ARN of an EC2 instance\. In OpsCenter, you can then view all of the details that Amazon EC2 provides about that instance\. To view a list of resource types that automatically create deep links to related resource, see [Supported resources reference](OpsCenter-related-resources-reference.md)\.

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

1. For **Resource ARN**, enter the ARN of the related resource, and then choose **Add**\.
**Note**  
If you don't know the ARN of the resource, you can manually create it\. For information about how to create an ARN, see the [Amazon Resource Names \(ARNs\) and AWS Service Namespaces](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) in the *Amazon Web Services General Reference*\.

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
When you edit an OpsItem, you can specify the ARN of an SNS topic in the **Notifications** field\. By specifying an ARN, you ensure that all stakeholders receive a notification when the OpsItem is edited, including a status change\. You may find it helpful to create different ARNs for notifications about different types of AWS resources or different environments\. For more information, see the [https://docs.aws.amazon.com/sns/latest/dg/](https://docs.aws.amazon.com/sns/latest/dg/)\.

**Important**  
The SNS topic must exist in the same AWS Region as the OpsItem\. If they are in different regions, the system returns an error\.

**To edit OpsItem details**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose an OpsItem ID to open the details page\.

1. In the **OpsItem details** section, choose **Edit**\.

1. Edit the details of the OpsItem according to the requirements and guidelines specified by your organization\.

1. When you are finished, choose **Save**\.

## Working with related and similar OpsItems<a name="OpsCenter-working-with-OpsItems-similar"></a>

The Related and Similar OpsItem features are designed to help you investigate operations issues while providing context about the scope of an issue\. In the **Related OpsItems** section, you can specify a maximum of 10 IDs for other OpsItems that are related to the current OpsItem\. OpsItems can be related in different ways, including a parent\-child relationship between OpsItems, a root cause, or a duplicate\. 

![\[Viewing related OpsItems.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_scenario_4.png)

The **Similar OpsItems** feature is a system\-generated list of OpsItems that may be related or of interest to you\. To generate the list, the system scans the titles and descriptions of all OpsItems and returns OpsItems that use similar words\.

![\[Viewing similar OpsItems.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_scenario_5.png)

**To add a related OpsItem from similar OpsItems**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose an OpsItem ID to open the details page\.

1. In the **Related OpsItem** section, choose **Add**\.

1. For **OpsItem ID**, specify an ID\.

1. Choose **Add**\.

## Working with operational data<a name="OpsCenter-working-operational-data"></a>

Operational data is custom data that provides useful reference details about the OpsItem\. For example, you can specify log files, error strings, license keys, troubleshooting tips, or other relevant data\. You enter operational data as key\-value pairs\. The key has a maximum length of 128 characters\. The value has a maximum size of 20 KB\. You can enter multiple key\-value pairs of operational data\.

**Important**  
Operational data keys *can't* begin with the following: amazon, aws, amzn, ssm, /amazon, /aws, /amzn, /ssm\.

![\[Viewing operational data for an OpsItem.\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_scenario_7.png)

You can choose to make the data searchable by other users in the account or you can restrict search access\. Searchable data means that all users with access to the OpsItem Overview page \(as provided by the [DescribeOpsItems](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeOpsItems.html) API action\) can view and search on the specified data\. Operational data that is not searchable is only viewable by users who have access to the OpsItem \(as provided by the [GetOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetOpsItem.html) API action\)\.

**To add operational data to an OpsItem**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **OpsCenter**\.

1. Choose an OpsItem ID to open the details page\.

1. Expand either **Operational data**\.

1. If no operational data exists for the OpsItem, then choose **Add**\. If operational data already exists for the OpsItem, choose **Manage**\.

1. For **Key**, specify a word or words to help users understand the purpose of the data\. The key can't begin with the following: amazon, aws, amzn, ssm, /amazon, /aws, /amzn, /ssm\.

1. For **Value**, specify the data\.

1. Choose **Save**\.

After you create operational data, you can edit the key and the value, remove the operational data, or add additional key\-value pairs by choosing **Manage**\. 

**Note**  
You can filter OpsItems by using the **Operational data** operator on the OpsItems page\. In the Search box, choose **Operational data**, and then enter a key\-value pair in JSON\. You must enter the key\-value pair by using the following format: `{"key":"key_name","value":"a_value"}`

## Reducing duplicate OpsItems<a name="OpsCenter-working-deduplication"></a>

OpsCenter uses a combination of built\-in logic and configurable deduplication strings to help avoid creating duplicate OpsItems\. Deduplication built\-in logic is applied anytime the [CreateOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateOpsItem.html) API action is called\. When creating the OpsItem, Systems Manager creates and stores a hash based on the deduplication string and the resource that trigged the OpsItem\. When a request is made to create a new OpsItem, the system checks the deduplication string of the new request\. If a matching hash exists for this deduplication string, then Systems Manager doesn't create a new OpsItem\. 

Note the following information about OpsCenter and deduplication: 
+ Deduplication strings are not case sensitive\. If the system finds a matching deduplication string in an OpsItem, regardless of casing, the new OpsItem isn't created\.
+ If the system finds a matching deduplication string in an OpsItem, and that OpsItem has a status of `Open`, then the new OpsItem isn't created\. If a matching deduplication string is found in an OpsItem that has a status of `Resolved`, then the system creates a new OpsItem\.
+ If the system finds a matching deduplication string in an OpsItem, but the resources are different, then the system creates the new OpsItem\.

### Configuring deduplication strings<a name="OpsCenter-working-deduplication-configuring"></a>

OpsCenter includes the following options for configuring deduplication strings\.
+ **Edit preconfigured deduplication strings**: Each of the OpsItem default EventBridge rules includes a preconfigured deduplication string\. You can edit these deduplication strings in EventBridge\.
+ **Manually specify deduplication strings**: You can enter a deduplication string by using either the **Deduplication string** field in the console or the `OperationalData` parameter when you create a new OpsItem by using either the AWS CLI or AWS Tools for Windows PowerShell\. 

After the system creates an OpsItem, it populates the **Deduplication string** field, if a string was specified\. Here's an example\.

![\[Viewing an OpsItem deduplication entry in the AWS Management console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_dedup_1.png)

After you create an OpsItem, you *can't* edit or change the deduplication strings in that OpsItem\.

This sections includes the following procedures for configuring deduplication strings\.
+ [Editing a deduplication string in an OpsCenter default EventBridge rule](#OpsCenter-working-deduplication-editing-cwe)
+ [Specifying a deduplication string by using the AWS CLI](#OpsCenter-working-deduplication-configuring-manual-cli)

**Note**  
For information about entering deduplication strings when you manually create an OpsItem in the console, see [Creating OpsItems manually](OpsCenter-manually-create-OpsItems.md)\.

#### Editing a deduplication string in an OpsCenter default EventBridge rule<a name="OpsCenter-working-deduplication-editing-cwe"></a>

Use the following procedure to specify a deduplication string for an EventBridge rule that targets OpsCenter\.

**To edit a deduplication string in an OpsItem default EventBridge rule**

1. Sign in to the AWS Management Console and open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**\.

1. Choose a rule, and then choose **Edit**\.

1. In the **Select targets** section, expand **Configure input**\. In the lower **Input transformer** field, locate the `"operationalData": { "/aws/dedup"` JSON entry and the deduplication strings that you want to edit\.

   The deduplication string entry in EventBridge rules uses the following JSON format\.

   ```
   "operationalData": { "/aws/dedup": {"type": "SearchableString","value": "{\"dedupString\":\"Words the system should use to check for duplicate OpsItems\"}"}
   ```

   Here is an example\.

   ```
   "operationalData": { "/aws/dedup": {"type": "SearchableString","value": "{\"dedupString\":\"SSMOpsCenter-EBS-volume-performance-issue\"}"}
   ```

1. Edit the deduplications strings, and then choose **Update** to finish updating the rule\.

#### Specifying a deduplication string by using the AWS CLI<a name="OpsCenter-working-deduplication-configuring-manual-cli"></a>

You can specify a deduplication string when you manually create a new OpsItem by using the AWS CLI\. You enter the deduplication string by using the `OperationalData` parameter\. The parameter syntax uses JSON, as shown here\.

```
--operational-data '{"/aws/dedup":{"Value":"{\"dedupString\": \"Words the system should use to check for duplicate OpsItems\"}","Type":"SearchableString"}}'
```

Here is an example command that specifies a deduplication string of `disk full`\.

------
#### [ Linux ]

```
aws ssm create-ops-item \
    --title "EC2 instance disk full" \
    --description "Log clean up may have failed which caused the disk to be full" \
    --priority 1 \
    --source ec2 \
    --operational-data '{"/aws/dedup":{"Value":"{\"dedupString\": \"disk full\"}","Type":"SearchableString"}}' \
    --tags "Key=EC2,Value=ProductionServers" \
    --notifications Arn="arn:aws:sns:us-west-1:12345678:TestUser"
```

------
#### [ Windows ]

```
aws ssm create-ops-item ^
    --title "EC2 instance disk full" ^
    --description "Log clean up may have failed which caused the disk to be full" ^
    --priority 1 ^
    --source EC2 ^
    --operational-data={\"/aws/dedup\":{\"Value\":\"{\\"""dedupString\\""":\\"""disk full\\"""}\",\"Type\":\"SearchableString\"}} ^
    --tags "Key=EC2,Value=ProductionServers" --notifications Arn="arn:aws:sns:us-west-1:12345678:TestUser"
```

------