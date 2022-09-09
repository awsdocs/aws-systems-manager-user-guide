# `aws:invokeLambdaFunction` – Invoke an AWS Lambda function<a name="automation-action-lamb"></a>

Invokes the specified AWS Lambda function\.

**Note**  
Each `aws:invokeLambdaFunction` action can run up to a maximum duration of 300 seconds \(5 minutes\)\. You can limit the timeout by specifying the `timeoutSeconds` parameter for an `aws:invokeLambdaFunction` step\.

**Input**  
This action supports most invoked parameters for the Lambda service\. For more information, see [Invoke](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html)\.

------
#### [ YAML ]

```
name: invokeMyLambdaFunction
action: aws:invokeLambdaFunction
maxAttempts: 3
timeoutSeconds: 120
onFailure: Abort
inputs:
  FunctionName: MyLambdaFunction
```

------
#### [ JSON ]

```
{
    "name": "invokeMyLambdaFunction",
    "action": "aws:invokeLambdaFunction",
    "maxAttempts": 3,
    "timeoutSeconds": 120,
    "onFailure": "Abort",
    "inputs": {
        "FunctionName": "MyLambdaFunction"
    }
}
```

------

FunctionName  
The name of the Lambda function\. This function must exist\.  
Type: String  
Required: Yes

Qualifier  
The function version or alias name\.  
Type: String  
Required: No

InvocationType  
The invocation type\. The default value is `RequestResponse`\.  
Type: String  
Valid values: `Event` \| `RequestResponse` \| `DryRun`  
Required: No

LogType  
If the default value is `Tail`, the invocation type must be `RequestResponse`\. Lambda returns the last 4 KB of log data produced by your Lambda function, base64\-encoded\.  
Type: String  
Valid values: `None` \| `Tail`  
Required: No

ClientContext  
The client\-specific information\.  
Required: No

InputPayload  
A YAML or JSON object that is passed to the first parameter of the handler\. You can use this input to pass data to the function\. This input provides more flexibility and support than the legacy `Payload` input\. If you define both `InputPayload` and `Payload` for the action, `InputPayload` takes precedence and the `Payload` value is not used\.  
Type: StringMap  
Required: No

Payload  
A JSON string that's passed to the first parameter of the handler\. This can be used to pass input data to the function\. We recommend using `InputPayload` input for added functionality\.  
Type: String  
Required: NoOutput

StatusCode  
The HTTP status code\.

FunctionError  
Indicates whether an error occurred while running the Lambda function\. If an error occurred, this field will show either `Handled` or `Unhandled`\. `Handled` errors are reported by the function\. `Unhandled` errors are detected and reported by Lambda\.

LogResult  
The base64\-encoded logs for the Lambda function invocation\. Logs are present only if the invocation type is `RequestResponse`, and the logs were requested\.

Payload  
The JSON representation of the object returned by the Lambda function\. Payload is present only if the invocation type is `RequestResponse`\. Up to 200KB is returned

The following is a portion from the `AWS-PatchInstanceWithRollback` runbook demonstrating how to reference outputs from the `aws:invokeLambdaFunction` action\.

------
#### [ YAML ]

```
- name: IdentifyRootVolume
  action: aws:invokeLambdaFunction
  inputs:
    FunctionName: "IdentifyRootVolumeLambda-{{automation:EXECUTION_ID}}"
    Payload: '{"InstanceId": "{{InstanceId}}"}'
- name: PrePatchSnapshot
  action: aws:executeAutomation
  inputs:
    DocumentName: "AWS-CreateSnapshot"
    RuntimeParameters:
      VolumeId: "{{IdentifyRootVolume.Payload}}"
      Description: "ApplyPatchBaseline restoration case contingency"
```

------
#### [ JSON ]

```
{
    "name": "IdentifyRootVolume",
    "action": "aws:invokeLambdaFunction",
    "inputs": {
      "FunctionName": "IdentifyRootVolumeLambda-{{automation:EXECUTION_ID}}",
      "Payload": "{\"InstanceId\": \"{{InstanceId}}\"}"
    }
  },
  {
    "name": "PrePatchSnapshot",
    "action": "aws:executeAutomation",
    "inputs": {
      "DocumentName": "AWS-CreateSnapshot",
      "RuntimeParameters": {
        "VolumeId": "{{IdentifyRootVolume.Payload}}",
        "Description": "ApplyPatchBaseline restoration case contingency"
      }
    }
  }
```

------