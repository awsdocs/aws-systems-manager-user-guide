# aws:invokeLambdaFunction – Invoke an AWS Lambda function<a name="automation-action-lamb"></a>

Invokes the specified Lambda function\.

**Note**  
Each `aws:invokeLambdaFunction` action can run up to a maximum duration of 300 seconds \(five minutes\)\. You can limit the timeout by specifying the `timeoutSeconds` parameter for an `aws:invokeLambdaFunction` step\.

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
The invocation type\. The default is `RequestResponse`\.  
Type: String  
Valid values: `Event` \| `RequestResponse` \| `DryRun`  
Required: No

LogType  
If `Tail`, the invocation type must be `RequestResponse`\. AWS Lambda returns the last 4 KB of log data produced by your Lambda function, base64\-encoded\.  
Type: String  
Valid values: `None` \| `Tail`  
Required: No

ClientContext  
The client\-specific information\.  
Required: No

Payload  
The JSON input for your Lambda function\.  
Required: NoOutput

StatusCode  
The function execution status code\.

FunctionError  
Indicates whether an error occurred while running the Lambda function\. If an error occurred, this field will show either `Handled` or `Unhandled`\. `Handled` errors are reported by the function\. `Unhandled` errors are detected and reported by AWS Lambda\.

LogResult  
The base64\-encoded logs for the Lambda function invocation\. Logs are present only if the invocation type is `RequestResponse`, and the logs were requested\.

Payload  
The JSON representation of the object returned by the Lambda function\. Payload is present only if the invocation type is `RequestResponse`\.