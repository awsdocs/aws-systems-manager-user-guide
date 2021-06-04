# `aws:executeStateMachine` – Run an AWS Step Functions state machine<a name="automation-action-executeStateMachine"></a>

Runs an AWS Step Functions state machine\.

**Input**

This action supports most parameters for the Step Functions [StartExecution](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html) API action\.

**Required AWS Identity and Access Management \(IAM\) permissions**
+ `states:DescribeExecution`
+ `states:StartExecution`
+ `states:StopExecution`

**Note**  
Currently, this action is only supported in the following AWS Regions:  
us\-west\-2
us\-east\-1
us\-east\-2
eu\-west\-1
eu\-west\-2
eu\-central\-1
ap\-northeast\-1
ap\-southeast\-2

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
The Amazon Resource Name \(ARN\) of the Step Functions state machine\.  
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