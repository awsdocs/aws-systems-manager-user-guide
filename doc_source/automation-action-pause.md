# `aws:pause` – Pause an automation<a name="automation-action-pause"></a>

This action pauses the automation\. Once paused, the automation status is *Waiting*\. To continue the automation, use the [SendAutomationSignal](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendAutomationSignal.html) API operation with the `Resume` signal type\. We recommend using the `aws:sleep` or `aws:approve` action for more granular control of your workflows\.

**Input**  
The input is as follows\.

------
#### [ YAML ]

```
name: pauseThis
action: aws:pause
inputs: {}
```

------
#### [ JSON ]

```
{
    "name": "pauseThis",
    "action": "aws:pause",
    "inputs": {}
}
```

------Output

None  
