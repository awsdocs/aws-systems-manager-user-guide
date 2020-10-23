# aws:executeStateMachine – Run an AWS Step Functions state machine<a name="automation-action-executeStateMachine"></a>

Run an AWS Step Functions state machine\.

**Input**

This action supports most parameters for the Step Functions [StartExecution](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html) API action\.

------
#### [ YAML ]

```
name: executeTheStateMachine
action: aws:executeStateMachine
inputs:
  stateMachineArn: StateMachine_ARN
  input: '{"parameters":"values"}'
  name: name
```

------
#### [ JSON ]

```
{
    "name": "executeTheStateMachine",
    "action": "aws:executeStateMachine",
    "inputs": {
        "stateMachineArn": "StateMachine_ARN",
        "input": "{\"parameters\":\"values\"}",
        "name": "name"
    }
}
```

------

stateMachineArn  
The ARN of the Step Functions state machine\.  
Type: String  
Required: Yes

name  
The name of the execution\.  
Type: String  
Required: No

input  
A string that contains the JSON input data for the execution\.  
Type: String  
Required: No