# Using Parameter Store parameters in AWS Lambda functions<a name="ps-integration-lambda-extensions"></a>

Parameter Store, a capability of AWS Systems Manager, provides secure, hierarchical storage for configuration data management and secrets management\. You can store data such as passwords, database strings, Amazon Machine Image \(AMI\) IDs, and license codes as parameter values\. 

To use parameters from Parameter Store in AWS Lambda functions without using an SDK, you can use the AWS Parameters and Secrets Lambda Extension\. This extension retrieves parameter values and caches them for future use\. Using the Lambda extension can reduce your costs by reducing the number of API calls to Parameter Store\. Using the extension can also improve latency because retrieving a cached parameter is faster than retrieving it from Parameter Store\. 

A Lambda extension is a companion process that augments the capabilities of a Lambda function\. An extension is like a client that runs in parallel to a Lambda invocation\. This parallel client can interface with your function at any point during its lifecycle\. For more information about Lambda extensions, see [Lambda Extensions API](https://docs.aws.amazon.com/lambda/latest/dg/runtimes-extensions-api.html) in the *AWS Lambda Developer Guide*\.

The AWS Parameters and Secrets Lambda Extension works for both Parameter Store and AWS Secrets Manager\. To learn how to use the Lambda extension with secrets from Secrets Manager, see [Use AWS Secrets Manager secrets in AWS Lambda functions](https://docs.aws.amazon.com/secretsmanager/latest/userguide/retrieving-secrets_lambda.html) in the *AWS Secrets Manager User Guide*\. 

**Related info**

[Using the AWS Parameter and Secrets Lambda extension to cache parameters and secrets](http://aws.amazon.com/blogs/compute/using-the-aws-parameter-and-secrets-lambda-extension-to-cache-parameters-and-secrets/) \(AWS Compute Blog\)

## How the extension works<a name="ps-integration-lambda-extensions-how-it-works"></a>

To use parameters in a Lambda function *without* the Lambda extension, you must configure your Lambda function to receive configuration updates by integrating with the `GetParameter` API action for Parameter Store\.

When you use the AWS Parameters and Secrets Lambda Extension, the extension retrieves the parameter value from Parameter Store and stores it in the local cache\. Then, the cached value is used for further invocations until it expires\. Cached values expire after they pass their time\-to\-live \(TTL\)\. You can configure the TTL value using the `SSM_PARAMETER_STORE_TTL` [environment variable](#ps-integration-lambda-extensions-config), as explained later in this topic\.

If the configured cache TTL has not expired, the cached parameter value is used\. If the time has expired, the cached value is invalidated and the parameter value is retrieved from Parameter Store\.

Also, the system detects parameter values that are used frequently and maintains them in the cache while clearing those that are expired or unused\.

### Implementation details<a name="lambda-extension-details"></a>

Use the following details to help you configure the AWS Parameters and Secrets Lambda Extension\.

Authentication  
To authorize and authenticate Parameter Store requests, the extension uses the same credentials as those used to run the Lambda function itself\. Therefore, the AWS Identity and Access Management \(IAM\) role used to run the function must have the following permissions to interact with Parameter Store:  
+ `ssm:GetParameter` – Required to retrieve parameters from Parameter Store
+ `kms:Decrypt` – Required if you are retrieving `SecureString` parameters from Parameter Store
For more information, see [AWS Lambda execution role](https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html) in the *AWS Lambda Developer Guide*\.

Instantiation  
Lambda instantiates separate instances corresponding to the concurrency level that your function requires\. Each instance is isolated and maintains its own local cache of your configuration data\. For more information about Lambda instances and concurrency, see [Managing Lambda reserved currency](https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html) in the *AWS Lambda Developer Guide*\.

No SDK dependence  
The AWS Parameters and Secrets Lambda Extension works independently of any AWS SDK language library\. An AWS SDK is not required to make GET requests to Parameter Store\.

Localhost port  
Use `localhost` in your GET requests\. The extension makes requests to localhost port 2773\. You do not need to specify an external or internal endpoint to use the extension\. You can configure the port by setting the [environment variable](#ps-integration-lambda-extensions-config) `PARAMETERS_SECRETS_EXTENSION_HTTP_PORT`\.   
For example, in Python, your GET URL might look something like the following example\.  

```
parameter_url = ('http://localhost:' + port + '/systemsmanager/parameters/get/?name=' + ssm_parameter_path)
```

Changes to a parameter value before TTL expires  
The extension doesn't detect changes to the parameter value and doesn't perform an auto\-refresh before the TTL expires\. If you change a parameter value, operations that use the cached parameter value might fail until the cache is next refreshed\. If you expect frequent changes to a parameter value, we recommend setting a shorter TTL value\.

Header requirement  
To retrieve parameters from the extension cache, the header of your GET request must include an `X-Aws-Parameters-Secrets-Token` reference\. Set the token to `AWS_SESSION_TOKEN`, which is provided by Lambda for all running functions\. Using this header indicates that the caller is within the Lambda environment\.

Example  
The following example in Python demonstrates a basic request to retrieve the value of a cached parameter\.  

```
import urllib.request
import os
import json

aws_session_token = os.environ.get('AWS_SESSION_TOKEN')

def lambda_handler(event, context):
    # Retrieve /my/parameter from Parameter Store using extension cache
    req = urllib.request.Request('http://localhost:2773/systemsmanager/parameters/get?name=%2Fmy%2Fparameter')
    req.add_header('X-Aws-Parameters-Secrets-Token', aws_session_token)
    config = urllib.request.urlopen(req).read()

    return json.loads(config)
```

ARM support  
Currently, the extension supports the ARM architecture in the following AWS Regions only:  
+ US East \(Ohio\) Region \(us\-east\-2\)
+ US East \(N\. Virginia\) Region \(us\-east\-1\)
+ Asia Pacific \(Mumbai\) Region \(ap\-south\-1\)
+ Asia Pacific \(Singapore\) Region \(ap\-southeast\-1\)
+ Asia Pacific \(Sydney\) Region \(ap\-southeast\-2\)
+ Asia Pacific \(Tokyo\) Region \(ap\-northeast\-1\)
+ Europe \(Frankfurt\) Region \(eu\-central\-1\)
+ Europe \(Ireland\) Region \(eu\-west\-1\)
+ Europe \(London\) Region \(eu\-west\-2\)
For complete lists of extension ARNs, see [AWS Parameters and Secrets Lambda Extension ARNs](#ps-integration-lambda-extensions-add)\.

Logging  
Lambda logs execution information about the extension along with the function by using Amazon CloudWatch Logs\. By default, the extension logs a minimal amount of information to CloudWatch\. To log more details, set the [environment variable](#ps-integration-lambda-extensions-config) `PARAMETERS_SECRETS_EXTENSION_LOG_LEVEL` to `DEBUG`\.

### Adding the extension to a Lambda function<a name="add-extension"></a>

To use the AWS Parameters and Secrets Lambda Extension, you add the extension to your Lambda function as a layer\.

Use one of the following methods to add the extension to your function\.

AWS Management Console \(Add layer option\)  

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Choose your function\. In the **Layers** area, choose **Add a layer**\.

1. In the **Choose a layer** area, choose the **AWS layers** option\.

1. For **AWS layers**, choose **AWS\-Parameters\-and\-Secrets\-Lambda\-Extension**, choose a version, and then choose **Add**\.

AWS Management Console \(Specify ARN option\)  

1. Open the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.

1. Choose your function\. In the **Layers** area, choose **Add a layer**\.

1. In the **Choose a layer** area, choose the **Specify an ARN** option\.

1. For **Specify an ARN**, enter the [extension ARN for your AWS Region and architecture](#ps-integration-lambda-extensions-add), and then choose **Add**\.

AWS Command Line Interface  
Run the following command in the AWS CLI\. Replace each *example resource placeholder* with your own information\.  

```
aws lambda update-function-configuration \
    --function-name function-name \
    --layers layer-ARN
```

**Related information**

[Using layers with your Lambda function](https://docs.aws.amazon.com/lambda/latest/dg/invocation-layers.html)

[Configuring extensions \(\.zip file archive\)](https://docs.aws.amazon.com/lambda/latest/dg/using-extensions.html#using-extensions-config)

## AWS Parameters and Secrets Lambda Extension environment variables<a name="ps-integration-lambda-extensions-config"></a>

You can configure the extension by changing the following environment variables\. To see the current settings, set `PARAMETERS_SECRETS_EXTENSION_LOG_LEVEL` to `DEBUG`\. For more information, see [Using AWS Lambda environment variables](https://docs.aws.amazon.com/lambda/latest/dg/configuration-envvars.html) in the *AWS Lambda Developer Guide*\. 

**Note**  
AWS Lambda records operation details about the Lambda extension and Lambda function in Amazon CloudWatch Logs\. 


****  

| Environment variable | Details | Required | Valid values | Default value | 
| --- | --- | --- | --- | --- | 
|  `SSM_PARAMETER_STORE_TIMEOUT_MILLIS`  |  Timeout, in milliseconds, for requests to Parameter Store\.   A value of 0 \(zero\) indicates no timeout\.  | No | All whole numbers | 0 \(zero\) | 
|  `SECRETS_MANAGER_TIMEOUT_MILLIS`  |  Timeout, in milliseconds, for requests to Secrets Manager\.   A value of 0 \(zero\) indicates no timeout\.  | No | All whole numbers |  0 \(zero\)  | 
|  `SSM_PARAMETER_STORE_TTL`  |  Maximum valid lifetime, in seconds, of a parameter in the cache before it is invalidated\. A value of 0 \(zero\) indicates that the cache should be bypassed\. This variable is ignored if the value for `PARAMETERS_SECRETS_EXTENSION_CACHE_SIZE` is 0 \(zero\)\.  | No | 0 \(zero\) to 300 s \(Five minutes\) | 300 s \(Five minutes\) | 
|  `SECRETS_MANAGER_TTL`  |  Maximum valid lifetime, in seconds, of a secret in the cache before it is invalidated\. A value of 0 \(zero\) indicates that the cache is bypassed\. This variable is ignored if the value for `PARAMETERS_SECRETS_EXTENSION_CACHE_SIZE` is 0 \(zero\)\.   | No | 0 \(zero\) to 300 s \(Five minutes\) | 300 s \(5 minutes\) | 
| PARAMETERS\_SECRETS\_EXTENSION\_CACHE\_ENABLED |  Determines whether the cache for the extension is enabled\. Value values: `TRUE \| FALSE`  | No | TRUE \| FALSE | TRUE | 
| PARAMETERS\_SECRETS\_EXTENSION\_CACHE\_SIZE |  The maximum size of the cache in terms of number of items\. A value of 0 \(zero\) indicates that the cache is bypassed\. This variable is ignored if both cache TTL values are 0 \(zero\)\.  | No | 0 \(zero\) to 1000 |  1000  | 
| PARAMETERS\_SECRETS\_EXTENSION\_HTTP\_PORT | The port for the local HTTP server\. | No | 1 \- 65535 |  2773  | 
| PARAMETERS\_SECRETS\_EXTENSION\_MAX\_CONNECTIONS |  Maximum number of connections for the HTTP clients that the extension uses to make requests to Parameter Store or Secrets Manager\. This is a per\-client configuration for the number of connections that both the Secrets Manager client and Parameter Store client make to the backend services\.  | No | Minimum of 1; No maximum limit\. |  3  | 
| PARAMETERS\_SECRETS\_EXTENSION\_LOG\_LEVEL |  The level of detail reported in logs for the extension\. We recommend using `DEBUG` for the most detail about your cache configuration as you set up and test the extension\.  Logs for Lambda operations are automatically pushed to an associated CloudWatch Logs log group\.  | No |  `DEBUG \| WARN \| ERROR \| NONE \| INFO`  | INFO | 

## Sample commands for using the AWS Systems Manager Parameter Store and AWS Secrets Manager Extension<a name="ps-integration-lambda-extensions-sample-commands"></a>

The examples in this section demonstrate API actions for use with the AWS Systems Manager Parameter Store and AWS Secrets Manager extension\.

### Sample commands for Parameter Store<a name="sample-commands-ps"></a>

The Lambda extension uses read\-only access to the **GetParameter** API action\.

To call this action, make an HTTP GET call similar to the following\.

```
GET http://localhost:port/systemsmanager/parameters/get?name=parameter-path&version=version&label=label&withDecryption={true|false}
```

In this example, *parameter\-path* represents the full parameter name, or the parameter path if the parameter is part of a hierarchy\. *version* and *label* are the selectors available for use with the `GetParameter` action\. This command format provides access to parameters in the standard parameter tier\. 

**Note**  
When using GET calls, parameter values must be encoded for HTTP to preserve special characters\. For example, instead of formatting a hierarchical path like `/a/b/c`, encode characters that could be interpreted as part of the URL, such as `%2Fa%2Fb%2Fc`\.

```
GET http://localhost:port/systemsmanager/parameters/get/?name=MyParameter&version=5
```

To call a parameter in a hierarchy, make an HTTP GET call similar to the following\.

```
GET http://localhost:port/systemsmanager/parameters/get?name=%2Fa%2Fb%2F&label=release
```

To call a public \(global\) parameter, make an HTTP GET call similar to the following\.

```
GET http://localhost:port/systemsmanager/parameters/get/?name=%2Faws%2Fservice%20list%2F…
```

To make an HTTP GET call to a Secrets Manager secret by using Parameter Store references, make an HTTP GET call similar to the following\.

```
GET http://localhost:port/systemsmanager/parameters/get?name=%2Faws%2Freference%2Fsecretsmanager%2F…
```

To make a call using the Amazon Resource Name \(ARN\) for a parameter, make an HTTP GET call similar to the following\.

```
GET http://localhost:port/systemsmanager/parameters/get?name=arn:aws:ssm:us-east-1:123456789012:parameter/MyParameter
```

To make a call that accesses a `SecureString` parameter with decryption, make an HTTP GET call similar to the following\.

```
GET http://localhost:port/systemsmanager/parameters/get?name=MyParameter&withDecryption=true
```

You can specify that parameters aren't decrypted by omitting `withDecryption` or explicitly setting it to `false`\. You can also specify either a version or a label, but not both\. If you do, only the first of these that is placed after question mark \(`?`\) in the URL is used\.

## AWS Parameters and Secrets Lambda Extension ARNs<a name="ps-integration-lambda-extensions-add"></a>

The following tables provide extension ARNs for supported architectures and Regions\.

**Topics**
+ [Extension ARNs for the x86\_64 and x86 architectures](#intel)
+ [Extension ARNs for ARM64 architecture](#arm64)

### Extension ARNs for the x86\_64 and x86 architectures<a name="intel"></a>


****  

| Region | ARN | 
| --- | --- | 
|  US East \(Ohio\)  |  `arn:aws:lambda:us-east-2:590474943231:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
|  US East \(N\. Virginia\)  |  `arn:aws:lambda:us-east-1:177933569100:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
|  US West \(N\. California\)  |  `arn:aws:lambda:us-west-1:997803712105:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
|  US West \(Oregon\)  |  `arn:aws:lambda:us-west-2:345057560386:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
|  Africa \(Cape Town\)  |  `arn:aws:lambda:af-south-1:317013901791:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
|  Asia Pacific \(Hong Kong\)  |  `arn:aws:lambda:ap-east-1:768336418462:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
| Asia Pacific \(Hyderabad\) Region |  `arn:aws:lambda:ap-south-2:070087711984:layer:AWS-Parameters-and-Secrets-Lambda-Extension:1`  | 
|  Asia Pacific \(Jakarta\)  |  `arn:aws:lambda:ap-southeast-3:490737872127:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
|  Asia Pacific \(Mumbai\)  |  `arn:aws:lambda:ap-south-1:176022468876:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
| Asia Pacific \(Osaka\) |  `arn:aws:lambda:ap-northeast-3:576959938190:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
|  Asia Pacific \(Seoul\)  |  `arn:aws:lambda:ap-northeast-2:738900069198:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
|  Asia Pacific \(Singapore\)  |  `arn:aws:lambda:ap-southeast-1:044395824272:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
|  Asia Pacific \(Sydney\)  |  `arn:aws:lambda:ap-southeast-2:665172237481:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
|  Asia Pacific \(Tokyo\)  |  `arn:aws:lambda:ap-northeast-1:133490724326:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
|  Canada \(Central\)  |  `arn:aws:lambda:ca-central-1:200266452380:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
| China \(Beijing\) |  `arn:aws-cn:lambda:cn-north-1:287114880934:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
| China \(Ningxia\) |  `arn:aws-cn:lambda:cn-northwest-1:287310001119:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
|  Europe \(Frankfurt\)  |  `arn:aws:lambda:eu-central-1:187925254637:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
|  Europe \(Ireland\)  |  `arn:aws:lambda:eu-west-1:015030872274:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
|  Europe \(London\)  |  `arn:aws:lambda:eu-west-2:133256977650:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
|  Europe \(Milan\)  |  `arn:aws:lambda:eu-south-1:325218067255:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
|  Europe \(Paris\)  |  `arn:aws:lambda:eu-west-3:780235371811:layer:AWS-Parameters-and-Secrets-Lambda-Extension:3`  | 
| Europe \(Spain\) Region |  `arn:aws:lambda:eu-south-2:524103009944:layer:AWS-Parameters-and-Secrets-Lambda-Extension:1`  | 
|  Europe \(Stockholm\)  |  `arn:aws:lambda:eu-north-1:427196147048:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
| Europe \(Zurich\) Region |  `arn:aws:lambda:eu-central-2:772501565639:layer:AWS-Parameters-and-Secrets-Lambda-Extension:1`  | 
|  Middle East \(Bahrain\)  |  `arn:aws:lambda:me-south-1:832021897121:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
| Middle East \(UAE\) | arn:aws:lambda:me\-central\-1:858974508948:layer:AWS\-Parameters\-and\-Secrets\-Lambda\-Extension:4 | 
|  South America \(São Paulo\)  |  `arn:aws:lambda:sa-east-1:933737806257:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
| AWS GovCloud \(US\-East\) |  `arn:aws-us-gov:lambda:us-gov-east-1:129776340158:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 
| AWS GovCloud \(US\-West\) |  `arn:aws-us-gov:lambda:us-gov-west-1:127562683043:layer:AWS-Parameters-and-Secrets-Lambda-Extension:4`  | 

### Extension ARNs for ARM64 architecture<a name="arm64"></a>


****  

| Region | ARN | 
| --- | --- | 
|  US East \(Ohio\)  |  `arn:aws:lambda:us-east-2:590474943231:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:4`  | 
|  US East \(N\. Virginia\)  |  `arn:aws:lambda:us-east-1:177933569100:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:4`  | 
|  US West \(N\. California\) Region  |  `arn:aws:lambda:us-west-1:997803712105:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:1`  | 
|  US West \(Oregon\)  |  `arn:aws:lambda:us-west-2:345057560386:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:4`  | 
|  Africa \(Cape Town\) Region  |  `arn:aws:lambda:af-south-1:317013901791:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:1`  | 
|  Asia Pacific \(Hong Kong\) Region  |  `arn:aws:lambda:ap-east-1:768336418462:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:1`  | 
|  Asia Pacific \(Jakarta\) Region  |  `arn:aws:lambda:ap-southeast-3:490737872127:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:1`  | 
|  Asia Pacific \(Mumbai\)  |  `arn:aws:lambda:ap-south-1:176022468876:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:4`  | 
|  Asia Pacific \(Seoul\) Region  |  `arn:aws:lambda:ap-northeast-2:738900069198:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:1`  | 
|  Asia Pacific \(Singapore\)  |  `arn:aws:lambda:ap-southeast-1:044395824272:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:4`  | 
|  Asia Pacific \(Sydney\)  |  `arn:aws:lambda:ap-southeast-2:665172237481:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:4`  | 
|  Asia Pacific \(Tokyo\)  |  `arn:aws:lambda:ap-northeast-1:133490724326:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:4`  | 
|  Canada \(Central\) Region  |  `arn:aws:lambda:ca-central-1:200266452380:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:1`  | 
|  Europe \(Frankfurt\)  |  `arn:aws:lambda:eu-central-1:187925254637:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:4`  | 
|  Europe \(Ireland\)  |  `arn:aws:lambda:eu-west-1:015030872274:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:4`  | 
|  Europe \(London\)  |  `arn:aws:lambda:eu-west-2:133256977650:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:4`  | 
|  Europe \(Milan\) Region  |  `arn:aws:lambda:eu-south-1:325218067255:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:1`  | 
|  Europe \(Paris\) Region  |  `arn:aws:lambda:eu-west-3:780235371811:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:1`  | 
|  Europe \(Stockholm\) Region  |  `arn:aws:lambda:eu-north-1:427196147048:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:1`  | 
|  Middle East \(Bahrain\) Region  |  `arn:aws:lambda:me-south-1:832021897121:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:1`  | 
|  South America \(São Paulo\) Region  |  `arn:aws:lambda:sa-east-1:933737806257:layer:AWS-Parameters-and-Secrets-Lambda-Extension-Arm64:1`  | 