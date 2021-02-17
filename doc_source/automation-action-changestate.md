# aws:changeInstanceState – Change or assert instance state<a name="automation-action-changestate"></a>

Changes or asserts the state of the instance\.

This action can be used in assert mode \(do not run the API to change the state but verify the instance is in the desired state\.\) To use assert mode, set the `CheckStateOnly` parameter to true\. This mode is useful when running the Sysprep command on Windows, which is an asynchronous command that can run in the background for a long time\. You can ensure that the instance is stopped before you create an AMI\.

**Note**  
The default timeout value for this action is 3600 seconds \(one hour\)\. You can limit or extend the timeout by specifying the `timeoutSeconds` parameter for an `aws:changeInstanceState` step\.

**Input**

------
#### [ YAML ]

```
name: stopMyInstance
action: aws:changeInstanceState
maxAttempts: 3
timeoutSeconds: 3600
onFailure: Abort
inputs:
  InstanceIds:
  - i-1234567890abcdef0
  CheckStateOnly: true
  DesiredState: stopped
```

------
#### [ JSON ]

```
{
    "name":"stopMyInstance",
    "action": "aws:changeInstanceState",
    "maxAttempts": 3,
    "timeoutSeconds": 3600,
    "onFailure": "Abort",
    "inputs": {
        "InstanceIds": ["i-1234567890abcdef0"],
        "CheckStateOnly": true,
        "DesiredState": "stopped"
    }
}
```

------

InstanceIds  
The IDs of the instances\.  
Type: StringList  
Required: Yes

CheckStateOnly  
If false, sets the instance state to the desired state\. If true, asserts the desired state using polling\.  
Default: `false`  
Type: Boolean  
Required: No

DesiredState  
The desired state\. When set to `running`, this action waits for the Amazon EC2 state to be `Running`, the Instance Status to be `OK`, and the System Status to be `OK` before completing\.  
Type: String  
Valid values: `running` \| `stopped` \| `terminated`  
Required: Yes

Force  
If set, forces the instances to stop\. The instances do not have an opportunity to flush file system caches or file system metadata\. If you use this option, you must perform file system check and repair procedures\. This option is not recommended for EC2 instances for Windows Server\.  
Type: Boolean  
Required: No

AdditionalInfo  
Reserved\.  
Type: String  
Required: No

**Output**  
None