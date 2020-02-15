# Step 3: Create a Configuration and a Configuration Profile<a name="appconfig-creating-configuration-and-profile"></a>

A *configuration* is a collection of settings that influence the behavior of your application\. For example, you can create and deploy configurations that carefully introduce changes to your application or turn on new features that require a timely deployment, such as a product launch or announcement\. Here's a very simple example of an access list configuration\. 

```
{
    "AccessList": [
        {
            "user_name": "Mateo_Jackson"
        },
        {
            "user_name": "Jane_Doe"
        }
    ]
}
```

A *configuration profile* enables AWS AppConfig to access your configuration in its stored location\. You can store configurations in documents in the Systems Manager document store or in parameters in Parameter Store\. A configuration profile includes the following information\.
+ The URI location where the configuration is stored\. 
+ The AWS Identity and Access Management \(IAM\) role that provides access to the configuration file\.
+ A validator for the configuration data\. You can use either a JSON Schema or an AWS Lambda function to validate your configuration profile\. A configuration profile can have a maximum of two validators\.

For configurations stored in SSM documents, you can create the configuration by using the Systems Manager console at the time you create a configuration profile\. The process is described later in this topic\. 

For configurations stored in SSM parameters, you must create the parameter in Parameter Store first\. After you create the parameter, you can use the procedure in this topic to create the configuration profile\. For information about creating a parameter in Parameter Store, see [Creating Systems Manager Parameters](sysman-paramstore-su-create.md)\.

## About Validators<a name="appconfig-creating-configuration-and-profile-validators"></a>

When you create a configuration and configuration profile using the procedure in this topic, you can specify up to two validators\. A validator ensures that your configuration data is syntactically and semantically correct\. You can create validators in either JSON Schema or as an AWS Lambda function\.

**Important**  
Configuration data stored in SSM documents must validate against an associated JSON Schema before you can add the configuration to the system\. SSM parameters do not require a validation method, but we recommend that you create a validation check for new or updated SSM parameter configurations by using AWS Lambda\.

**JSON Schema Validators**

If you create a configuration in an SSM document by using the procedure in this topic, then you must specify or create a JSON Schema for that configuration\. A JSON Schema defines the allowable properties for each application configuration setting\. The JSON Schema functions like a set of rules to ensure that new or updated configuration settings conform to the best practices required by your application\. Here is an example\. 

```
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "title": "$id$",
      "description": "BasicFeatureToggle-1",
      "type": "object",
      "additionalProperties": false,
      "patternProperties": {
          "[^\\s]+$": {
              "type": "boolean"
          }
      },
      "minProperties": 1
    }
```

When you create the configuration by using the procedure in this topic, AppConfig verifies that the configuration conforms to the schema requirements\. If it doesn't, Systems Manager returns a validation error\.

**Note**  
AppConfig supports JSON Schema version 4\.X for inline schema\. If your application configuration requires a different version of JSON Schema, then you must create a Lambda validator\.

**AWS Lambda Validators**

Lambda function validators must be configured with the following event schema\. AppConfig uses this schema to invoke the Lambda function\. The content is a base64\-encoded string, and the URI is a string\. 

```
{
   "content":Base64EncodedByteString,
   "uri":"The uri of the configuration"
}
```

AppConfig verifies that the Lambda `X-Amz-Function-Error` header is set in the response\. Lambda sets this header if the function throws an exception\. For more information about `X-Amz-Function-Error`, see [Error Handling and Automatic Retries in AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/retries-on-errors.html) in the *AWS Lambda Developer Guide*\.

Here is a simple example of a Lambda response code for a successful validation\.

```
import json
def handler(event, context):
     #Add your validation logic here
     print("We passed!")
```

Here is a simple example of a Lambda response code for an unsuccessful validation\.

```
def handler(event, context):
     #Add your validation logic here
     raise Exception("Failure!")
```

AppConfig calls your validation Lambda when calling the `StartDeployment` and `ValidateConfigurationActivity` API actions\. You must provide appconfig\.amazonaws\.com permissions to invoke your Lambda\. For more information, see [Granting Function Access to AWS Services](https://docs.aws.amazon.com/lambda/latest/dg/access-control-resource-based.html#permissions-resource-serviceinvoke)\.

## About the Configuration Profile IAM Role<a name="appconfig-creating-configuration-and-profile-iam-role"></a>

You can create the IAM role that provides access to the configuration data by using AppConfig, as described in the following procedure\. Or you can create the IAM role yourself and choose it from a list\. If you create the role by using AppConfig, the system creates the role and specifies one of the following permissions policies, depending on which type of configuration source you choose\.

**Configuration source is an SSM document**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetDocument"
            ],
            "Resource": [
                "arn:aws:ssm:AWS-Region:account-number:document/document-name"
            ]
        }
    ]
}
```

**Configuration source is a Parameter Store parameter**

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetParameter"
            ],
            "Resource": [
                "Arn:aws:ssm:AWS-Region:account-number:parameter/parameter-name"
            ]
        }
    ]
    }
```

If you create the role by using AppConfig, the system also creates the following trust relationship for the role\. 

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "appconfig.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

## Creating a Configuration and a Configuration Profile<a name="appconfig-creating-configuration-and-profile-console"></a>

Use the following procedure to create an AppConfig configuration and a configuration profile by using the AWS Systems Manager console\.

**To create a configuration profile**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/appconfig/](https://console.aws.amazon.com/systems-manager/appconfig/)\.

1. On the **Applications** tab, choose the application you created in [Create an AppConfig Configuration](appconfig-creating-application.md) and then choose the **Configuration profiles** tab\.

1. Choose **Create configuration profile**\.

1. For **Name**, enter a name for the configuration profile\.

1. For **Description**, enter information about the configuration profile\.

1. In the **Service role** section, choose **New service role** to have AppConfig create the IAM role that provides access to the configuration data\. AppConfig automatically populates the **Role name** field based on the name you entered earlier\. Or, to choose a role that already exists in IAM, choose **Existing service role**\. Choose the role by using the **Role ARN** list\.

1. In the **Tags** section, enter a key and an optional value\. You can specify a maximum of 50 tags for a resource\. 

1. On the **Select configuration source** page, choose either **AWS Systems Manager Document** or **AWS Systems Manager Parameter**\.  
![\[Choose an AWS AppConfig configuration source\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/appconfig-profile-1.png)

1. If you selected **AWS Systems Manager Parameter**, then choose the name of the parameter from the list\. If you selected **AWS Systems Manager Document**, then complete the following steps\.

   1. In the **Document source** section, choose either **Saved document** or **New document**\. 

   1. If you choose **Saved document**, then choose the SSM document from the list\. If you choose **New document**, the **Details** and **Content** sections appear\.

   1. In the **Details** section, enter a name for the new application configuration\.

   1. For the **Application configuration schema** section, either choose the JSON schema using the list or choose **Create schema**\. If you choose **Create schema**, Systems Manager opens the **Create schema** page\. Enter the schema details in the **Content** section, and then choose **Create schema**\.  
![\[Enter details of an AWS AppConfig configuration\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/appconfig-profile-2.png)

   1. For **Application configuration schema version** either choose the version from the list or choose **Update schema** to edit the schema and create a new version\.

   1. In the **Content** section, choose either **YAML** or **JSON** and then enter the configuration data in the field\.  
![\[Enter configuration data in an AWS AppConfig configuration profile\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/appconfig-profile-3.png)

   1. Choose **Next**\.

1. On the **Add validators** page, choose either **JSON Schema** or **AWS Lambda**\. If you choose **JSON Schema**, enter the JSON Schema in the field\. If you choose **AWS Lambda**, choose the function Amazon Resource Name \(ARN\) and the version from the list\.   
![\[Add validators to an AWS AppConfig configuration\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/appconfig-profile-4.png)
**Important**  
Configuration data stored in SSM documents must validate against an associated JSON Schema before you can add the configuration to the system\. SSM parameters do not require a validation method, but we recommend that you create a validation check for new or updated SSM parameter configurations by using AWS Lambda\.

1. Choose **Create configuration profile**\.

AppConfig creates the configuration profile\. Proceed to [Step 4: Create a Deployment Strategy](appconfig-creating-deployment-strategy.md)\.