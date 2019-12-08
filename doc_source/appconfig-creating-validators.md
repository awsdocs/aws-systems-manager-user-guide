# Step 2: \(Optional\) Create Validators<a name="appconfig-creating-validators"></a>

One of the AWS AppConfig resources you set up before you deploy a configuration is called a *configuration profile*\. A configuration profile includes source information for accessing your configuration data in either a Systems Manager \(SSM\) document or a Parameter Store parameter\. You can also specify up to two validators for a configuration profile\. A validator ensures that your configuration data is syntactically and semantically correct\. AppConfig performs a check using the validators when you start a deployment\. If the validators detect an error, AppConfig stops the deployment before making changes to the configuration targets\.

You can create validators in either JSON Schema or as an AWS Lambda function\. Lambda function validators must be configured with the following event schema\. AppConfig uses this schema to invoke the Lambda function\. The content is a base64\-encoded string, and the URI is a string\. 

```
{
   "content":Base64EncodedByteString,
   "uri":"The uri of the configuration"
}
```

For more information, see [Granting Function Access to AWS Services](https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html#permissions-resource-serviceinvoke)\.