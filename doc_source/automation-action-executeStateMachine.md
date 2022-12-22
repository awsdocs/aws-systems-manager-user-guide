# `aws:executeStateMachine` – Run an AWS Step Functions state machine<a name="automation-action-executeStateMachine"></a>

Runs an AWS Step Functions state machine\.

**Input**

This action supports most parameters for the Step Functions [StartExecution](https://docs.aws.amazon.com/step-functions/latest/apireference/API_StartExecution.html) API operation\.

**Required AWS Identity and Access Management \(IAM\) permissions**
+ `states:DescribeExecution`
+ `states:StartExecution`
+ `states:StopExecution`

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

**Outputs**  
The following outputs are predefined for this action\.

executionArn  
The ARN of the execution\.  
Type: String

input  
The string that contains the JSON input data of the execution\. Length constraints apply to the payload size, and are expressed as bytes in UTF\-8 encoding\.\.  
Type: String

name  
The name of the execution\.  
Type: String

output  
The JSON output data of the execution\. Length constraints apply to the payload size, and are expressed as bytes in UTF\-8 encoding\.  
Type: String

startDate  
The date the execution is started\.  
Type: String

stateMachineArn  
The ARN of the executed stated machine\.  
Type: String

status  
The current status of the execution\.  
Type: String

stopDate  
If the execution has already ended, the date the execution stopped\.  
Type: String