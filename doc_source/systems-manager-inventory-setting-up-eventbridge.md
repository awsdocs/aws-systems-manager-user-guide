# About EventBridge monitoring of Inventory events<a name="systems-manager-inventory-setting-up-eventbridge"></a>

You can configure a rule in Amazon EventBridge to create an event in response to AWS Systems Manager Inventory resource state changes\. EventBridge supports events for the following Inventory state changes\. All events are sent on a best effort basis\.

**Custom inventory type deleted for a specific instance**: If a rule is configured to monitor for this event, EventBridge creates an event when a custom inventory type on a specific instance is deleted\. EventBridge sends one event per instance per custom inventory type\. Here is a sample event pattern\.

```
{
    "timestampMillis": 1610042981103,
    "source": "SSM",
    "account": "123456789012",
    "type": "INVENTORY_RESOURCE_STATE_CHANGE",
    "startTime": "Jan 7, 2021 6:09:41 PM",
    "resources": [
        {
            "arn": "arn:aws:ssm:us-east-1:123456789012:managed-instance/i-12345678"
        }
    ],
    "body": {
        "action-status": "succeeded",
        "action": "delete",
        "resource-type": "managed-instance",
        "resource-id": "i-12345678",
        "action-reason": "",
        "type-name": "Custom:MyCustomInventoryType"
    }
}
```

**Custom inventory type deleted event for all instances**: If a rule is configured to monitor for this event, EventBridge creates an event when a custom inventory type for all instances is deleted\. Here is a sample event pattern\.

```
{
    "timestampMillis": 1610042904712,
    "source": "SSM",
    "account": "123456789012",
    "type": "INVENTORY_RESOURCE_STATE_CHANGE",
    "startTime": "Jan 7, 2021 6:08:24 PM",
    "resources": [
        
    ],
    "body": {
        "action-status": "succeeded",
        "action": "delete-summary",
        "resource-type": "managed-instance",
        "resource-id": "",
        "action-reason": "The delete for type name Custom:SomeCustomInventoryType was completed. The deletion summary is: {\"totalCount\":1,\"remainingCount\":0,\"summaryItems\":[{\"version\":\"1.1\",\"count\":1,\"remainingCount\":0}]}",
        "type-name": "Custom:MyCustomInventoryType"
    }
}
```

**[PutInventory](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutInventory.html) call with old schema version event**: If a rule is configured to monitor for this event, EventBridge creates an event when a PutInventory call is made that uses a schema version that is lower than the current schema\. This event applies to all inventory types\. Here is a sample event pattern\.

```
{
    "timestampMillis": 1610042629548,
    "source": "SSM",
    "account": "123456789012",
    "type": "INVENTORY_RESOURCE_STATE_CHANGE",
    "startTime": "Jan 7, 2021 6:03:49 PM",
    "resources": [
        {
            "arn": "arn:aws:ssm:us-east-1:123456789012:managed-instance/i-12345678"
        }
    ],
    "body": {
        "action-status": "failed",
        "action": "put",
        "resource-type": "managed-instance",
        "resource-id": "i-01f017c1b2efbe2bc",
        "action-reason": "The inventory item with type name Custom:MyCustomInventoryType was sent with a disabled schema verison 1.0. You must send a version greater than 1.0",
        "type-name": "Custom:MyCustomInventoryType"
    }
}
```

For information about how to configure EventBridge to monitor for these events, see [Configuring EventBridge for Systems Manager events](monitoring-systems-manager-events.md)\.