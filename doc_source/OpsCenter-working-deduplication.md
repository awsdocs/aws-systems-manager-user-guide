# Managing duplicate OpsItems<a name="OpsCenter-working-deduplication"></a>

OpsCenter can receive multiple duplicate OpsItems for a single source from multiple AWS services\. OpsCenter uses a combination of built\-in logic and configurable deduplication strings to avoid creating duplicate OpsItems\. AWS Systems Manager applies deduplication built\-in logic when the [CreateOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateOpsItem.html) API operation is called\. 

AWS Systems Manager uses the following deduplication logic:

1. When creating the OpsItem, Systems Manager creates and stores a hash based on the deduplication string and the resource that initiated the OpsItem\. 

1. When another request is made to create an OpsItem, the system checks the deduplication string of the new request\.

1. If a matching hash exists for this deduplication string, Systems Manager checks the status of the existing OpsItem\. If the status of an existing OpsItem is open or in progress, the OpsItem is not created\. If the existing OpsItem is resolved, Systems Manager creates a new OpsItem\.

After you create an OpsItem, you *can't* edit or change the deduplication strings in that OpsItem\.

To manage duplicate OpsItems, you can do the following:
+ Edit the deduplication string for an Amazon EventBridge rule that targets OpsCenter\. For more information, see [Editing a deduplication string in a default EventBridge rule](#OpsCenter-working-deduplication-editing-cwe)\. 
+ Specify a deduplication string when you manually create an OpsItem\. For more information, see [Specifying a deduplication string using AWS CLI](#OpsCenter-working-deduplication-configuring-manual-cli)\.
+ Review and resolve duplicate OpsItems using operational insights\. You can use runbooks to resolve duplicate OpsItems\.

  To help you resolve duplicate OpsItems and reduce the number of OpsItems created by a source, Systems Manager provides automation runbooks\. For information, see [Resolving duplicate OpsItems based on insights](OpsCenter-working-operational-insights.md#OpsCenter-working-operational-insights-resolve)\.

## Editing a deduplication string in a default EventBridge rule<a name="OpsCenter-working-deduplication-editing-cwe"></a>

Use the following procedure to specify a deduplication string for an EventBridge rule that targets OpsCenter\.

**To edit a deduplication string for an EventBridge rule**

1. Sign in to the AWS Management Console and open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**\.

1. Choose a rule, and then choose **Edit**\.

1. Go to the **Select target\(s\)** page\.

1. In the **Additional settings** section, choose **Configure input transformer**\.

1. In the **Template** box, locate the `"operationalData": { "/aws/dedup"` JSON entry and the deduplication strings that you want to edit\.

   The deduplication string entry in EventBridge rules uses the following JSON format\.

   ```
   "operationalData": { "/aws/dedup": {"type": "SearchableString","value": "{\"dedupString\":\"Words the system should use to check for duplicate OpsItems\"}"}}
   ```

   Here is an example\.

   ```
   "operationalData": { "/aws/dedup": {"type": "SearchableString","value": "{\"dedupString\":\"SSMOpsCenter-EBS-volume-performance-issue\"}"}}
   ```

1. Edit the deduplication strings, and then choose **Confirm**\.

1. Choose **Next**\.

1. Choose **Next**\.

1. Choose **Update rule**\.

## Specifying a deduplication string using AWS CLI<a name="OpsCenter-working-deduplication-configuring-manual-cli"></a>

You can specify a deduplication string when you manually create a new OpsItem by using either the AWS Systems Manager console or the AWS CLI\. For information about entering deduplication strings when you manually create an OpsItem in the console, see [Create OpsItems manually](OpsCenter-manually-create-OpsItems.md)\. If you're using the AWS CLI, you can enter the deduplication string for the `OperationalData` parameter\. The parameter syntax uses JSON, as shown in the following example\.

```
--operational-data '{"/aws/dedup":{"Value":"{\"dedupString\": \"Words the system should use to check for duplicate OpsItems\"}","Type":"SearchableString"}}'
```

Here is an example command that specifies a deduplication string of `disk full`\.

------
#### [ Linux & macOS ]

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