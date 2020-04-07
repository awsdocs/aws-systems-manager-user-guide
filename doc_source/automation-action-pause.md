# aws:pause – Pause an automation execution<a name="automation-action-pause"></a>

This action pauses the Automation execution\. Once paused, the execution status is *Waiting*\. To continue the Automation execution, use the [SendAutomationSignal](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_SendAutomationSignal.html) API action with the Resume signal type\. 

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