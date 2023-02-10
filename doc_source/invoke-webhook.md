# `aws:invokeWebhook` – Invoke an Automation webhook integration<a name="invoke-webhook"></a>

Invokes the specified Automation webhook integration\. For information about creating Automation integrations, see [Creating webhook integrations for Automation](creating-webhook-integrations.md)\.

**Note**  
To use the `aws:invokeWebhook` action, your user or service role must allow the following actions:  
ssm:GetParameter
kms:Decrypt
Permission for the AWS Key Management Service \(AWS KMS\) `Decrypt` operation is only required if you use a customer managed key to encrypt the parameter for your integration\.

**Input**  
Provide the information for the Automation integration you want to invoke\.

------
#### [ YAML ]

```
action: "aws:invokeWebhook"
inputs: 
 IntegrationName: "exampleIntegration"
 Body: "Request body"
```

------
#### [ JSON ]

```
{
    "action": "aws:invokeWebhook",
    "inputs": {
        "IntegrationName": "exampleIntegration",
        "Body": "Request body"
    }
}
```

------

IntegrationName  
The name of the Automation integration\. For example, `exampleIntegration`\. The integration you specify must already exist\.  
Type: String  
Required: Yes

Body  
The payload you want to send when your webhook integration is invoked\.  
Type: String  
Required: NoOutput

Response  
The text received from the webhook provider response\.

ResponseCode  
The HTTP status code received from the webhook provider response\.