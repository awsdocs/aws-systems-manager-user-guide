# Step 8: Retrieving the Configuration<a name="appconfig-retrieving-the-configuration"></a>

You must configure a client to retrieve configuration updates by integrating with the [GetConfiguration](http://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetConfiguration.html) API action\. You can integrate using the AWS SDK\. The following AWS CLI command demonstrates how to retrieve a configuration\. This call includes the IDs of the AppConfig application, the environment, the configuration profile, and a unique client ID\. The configuration content is saved to the output filename\. 

```
aws appconfig get-configuration \
--application application_ID \
--environment environment_ID \
--configuration configuration_profile_ID \
--clientid client_ID \
output_filename
```

The system responds with information in the following format\.

```
{
   "ConfigurationVersion":"configuration version",
   "ContentType":"content type"
}
```

**Important**  
Configure your clients that call the `GetConfiguration` API action to apply the configuration response to your application and keep a record of the `ConfigurationVersion`\. Subsequent calls to `GetConfiguration` should send the `ConfigurationVersion` value as the `ClientConfigurationVersion` parameter\. AppConfig uses this value to determine if new configuration data is needed by the client\.

Sending `ConfigurationVersion` during subsequent polling for configuration updates is similar to the concept of [HTTP ETags](https://en.wikipedia.org/wiki/HTTP_ETag)\.

```
aws appconfig get-configuration \
--application application_ID \
--environment environment_ID \
--configuration configuration_profile_ID \
--client-configuration-version previous_configuration_version_value \
--clientid client_ID \
output_filename
```

**Note**  
We recommend tuning the polling frequency of your `GetConfiguration` API calls based on your budget, the expected frequency of your configuration deployments, and the number of targets for a configuration\.