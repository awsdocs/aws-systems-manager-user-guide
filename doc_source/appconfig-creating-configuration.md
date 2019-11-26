# Step 1: Create a Configuration<a name="appconfig-creating-configuration"></a>

A configuration is a collection of settings that influence the behavior of your application\. For example, a whitelist configuration that enables beta testers to access new applications or features might contain the following settings\.

```
{
   "whitelist" : [
      { "name" : "Jane_Doe",
        "email": "Jane_Doe@example.com",
        "location" : "iad21"
      },
      { "name" : "Mateo_Jackson",
        "email": "Mateo_Jackson@example.com",
        "location" : "iad21"
      }
   ]
}
```

An A/B testing configuration that enables customers to use different versions of a new feature might contain the following settings\.

```
{
   "testingVariable" : {
     "feature1" : 40,
     "feature2" : 70
   },
   "restrictions" : {
     "feature1" : {
       "group" : "beta_tester",
       "location" : "iad21"
     }
   }
 }
```

You can create configurations as Systems Manager \(SSM\) documents in JSON or as Systems Manager Parameter Store parameters\. For information about creating a parameter in Parameter Store, see [Creating Systems Manager Parameters](sysman-paramstore-su-create.md)\. For information about creating an SSM document, see [Creating Systems Manager Documents](create-ssm-doc.md)\.

For an SSM document configuration, start by creating a JSON Schema that uses the `ApplicationConfigurationSchema` document type\. A JSON schema defines the allowable properties for each application configuration setting\. The JSON Schema functions like a set of rules to ensure that new or updated configuration settings conform to the best practices required by your application\. 

Here are some example JSON Schemas that could be saved as SSM documents that use the `ApplicationConfigurationSchema` document type\.

**Example 1: Feature toggle example in JSON Schema**

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

After you create the JSON Schema, you create the application as an SSM document that uses the `ApplicationConfiguration` document type\. When you save or update an SSM document that uses the `ApplicationConfiguration` document type, you specify the name of the corresponding `ApplicationConfigurationSchema` SSM document\. If the configuration data doesn't conform to the schema requirements, Systems Manager returns a validation error\. The system only creates a configuration that uses the `ApplicationConfiguration` document type, when it validates against the corresponding JSON Schema\.

Currently you can't create `ApplicationConfiguration` and `ApplicationConfigurationSchema` SSM documents in the console\. You must create them by using the AWS CLI, as shown in the following procedure\.

**To create an AppConfig configuration in an SSM document**

1. Enter your JSON Schema content into a file and save the file as **schema\_file\_name*\.json*\.

1. Enter your configuration content into a file and save the file as **configuration\_file\_name*\.json*\.

1. Install and configure the AWS CLI, if you have not already\.

   For information, see [Install or Upgrade the AWS CLI](getting-started-cli.md)\.

1. To create the JSON Schema document and add it to the Systems Manager document store, run the following command from AWS CLI\.

   ```
   aws ssm create-document  \
   --document-type ApplicationConfigurationSchema  \
   --name a_schema_name  \
   --content file://schema_file_name.json
   ```

1. To create the configuration document and add it to the Systems Manager document store, run the following command from AWS CLI\.

   ```
   aws ssm create-document  \
   --document-type ApplicationConfiguration  \
   --name a_configuration_name  \
   --content file://configuration_file_name.json  \
   --requires Name=a_schema_name
   ```

If the command to create the configuration document fails, verify that the configuration data conforms to the requirements in the JSON Schema document\.