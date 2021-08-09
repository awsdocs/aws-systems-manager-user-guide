# Working with deduplication strings<a name="OpsCenter-working-deduplication-strings"></a>

OpsCenter uses a combination of built\-in logic and configurable deduplication strings to help avoid creating duplicate OpsItems\. Deduplication built\-in logic is applied anytime the [CreateOpsItem](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_CreateOpsItem.html) API operation is called\. When creating the OpsItem, Systems Manager creates and stores a hash based on the deduplication string and the resource that initiated the OpsItem\. When a request is made to create a new OpsItem, the system checks the deduplication string of the new request\. If a matching hash exists for this deduplication string, then Systems Manager doesn't create a new OpsItem\. 

Note the following information about OpsCenter and deduplication: 
+ Deduplication strings aren't case sensitive\. If the system finds a matching hash based on a deduplication string in an incoming OpsItem, regardless of the deduplication string casing, the new OpsItem isn't created\.
+ If the system finds a matching deduplication string in an OpsItem, and that OpsItem has a status of `Open/InProgress`, then the new OpsItem isn't created\. If a matching deduplication string is found in an OpsItem that has a status of `Resolved`, then the system creates a new OpsItem\.
+ If the system finds a matching deduplication string in an OpsItem, but the resources are different, then the system creates the new OpsItem\.
+ If no deduplication string is specified for an incoming OpsItem, then the OpsItem is always created\.

## Configuring deduplication strings<a name="OpsCenter-working-deduplication-configuring"></a>

OpsCenter includes the following options for configuring deduplication strings\.
+ **Edit preconfigured deduplication strings**: Each of the OpsItem default EventBridge rules includes a preconfigured deduplication string\. You can edit these deduplication strings in EventBridge\.
+ **Manually specify deduplication strings**: You can enter a deduplication string by using either the **Deduplication string** field in the console or the `OperationalData` parameter when you create a new OpsItem by using either the AWS Command Line Interface \(AWS CLI\) or AWS Tools for Windows PowerShell\. 

After the system creates an OpsItem, it populates the **Deduplication string** field, if a string was specified\. Here's an example\.

![\[Viewing an OpsItem deduplication entry in the AWS AWS Management Console\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/OpsItems_working_dedup_1.png)

After you create an OpsItem, you *can't* edit or change the deduplication strings in that OpsItem\.

This sections includes the following procedures for configuring deduplication strings\.
+ [Editing a deduplication string in an OpsCenter default EventBridge rule](#OpsCenter-working-deduplication-editing-cwe)
+ [Specifying a deduplication string by using the AWS CLI](#OpsCenter-working-deduplication-configuring-manual-cli)

**Note**  
For information about entering deduplication strings when you manually create an OpsItem in the console, see [Creating OpsItems manually](OpsCenter-manually-create-OpsItems.md)\.

### Editing a deduplication string in an OpsCenter default EventBridge rule<a name="OpsCenter-working-deduplication-editing-cwe"></a>

Use the following procedure to specify a deduplication string for an EventBridge rule that targets OpsCenter\.

**To edit a deduplication string in an OpsItem default EventBridge rule**

1. Sign in to the AWS Management Console and open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**\.

1. Choose a rule, and then choose **Edit**\.

1. In the **Select targets** section, expand **Configure input**\. In the lower **Input transformer** field, locate the `"operationalData": { "/aws/dedup"` JSON entry and the deduplication strings that you want to edit\.

   The deduplication string entry in EventBridge rules uses the following JSON format\.

   ```
   "operationalData": { "/aws/dedup": {"type": "SearchableString","value": "{\"dedupString\":\"Words the system should use to check for duplicate OpsItems\"}"}}
   ```

   Here is an example\.

   ```
   "operationalData": { "/aws/dedup": {"type": "SearchableString","value": "{\"dedupString\":\"SSMOpsCenter-EBS-volume-performance-issue\"}"}}
   ```

1. Edit the deduplications strings, and then choose **Update** to finish updating the rule\.

### Specifying a deduplication string by using the AWS CLI<a name="OpsCenter-working-deduplication-configuring-manual-cli"></a>

You can specify a deduplication string when you manually create a new OpsItem by using the AWS CLI\. You enter the deduplication string by using the `OperationalData` parameter\. The parameter syntax uses JSON, as shown here\.

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