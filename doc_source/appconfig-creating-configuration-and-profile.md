# Step 3: Create a configuration and a configuration profile<a name="appconfig-creating-configuration-and-profile"></a>

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

A *configuration profile* enables AWS AppConfig to access your configuration in its stored location\. You can store configurations in one of the following locations:
+ Objects in Amazon S3
+ Documents in the Systems Manager document store
+ Parameters in Parameter Store

A configuration profile includes the following information\.
+ The URI location where the configuration is stored\. 
+ The AWS Identity and Access Management \(IAM\) role that provides access to the configuration\.
+ A validator for the configuration data\. You can use either a JSON Schema or an AWS Lambda function to validate your configuration profile\. A configuration profile can have a maximum of two validators\.

For configurations stored in SSM documents, you can create the configuration by using the Systems Manager console at the time you create a configuration profile\. The process is described later in this topic\. 

For configurations stored in SSM parameters or in Amazon S3 objects, you must create the parameter or object first and then add it to Parameter Store or Amazon S3\. After you create the parameter or object, you can use the procedure in this topic to create the configuration profile\. For information about creating a parameter in Parameter Store, see [Creating Systems Manager parameters](sysman-paramstore-su-create.md)\.

**Topics**
+ [About configurations stored in Amazon S3](appconfig-creating-configuration-and-profile-S3-source.md)
+ [About validators](appconfig-creating-configuration-and-profile-validators.md)
+ [About the configuration profile IAM role](appconfig-creating-configuration-and-profile-iam-role.md)
+ [Creating a configuration and a configuration profile](appconfig-creating-configuration-and-profile-console.md)