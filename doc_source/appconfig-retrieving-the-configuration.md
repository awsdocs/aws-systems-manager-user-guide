# Step 6: Receiving the configuration<a name="appconfig-retrieving-the-configuration"></a>

You must configure a client to receive configuration updates by integrating with the [GetConfiguration](http://docs.aws.amazon.com/appconfig/2019-10-09/APIReference/API_GetConfiguration.html) API action\. You can integrate using the AWS SDK\. The following AWS CLI command demonstrates how to receive a configuration\. This call includes the IDs of the AppConfig application, the environment, the configuration profile, and a unique client ID\. The configuration content is saved to the output filename\. 

**Note**  
The `client-id` parameter in the following command is a unique, user\-specified ID to identify the client for the configuration\. This ID enables AppConfig to deploy the configuration in intervals, as defined in the deployment strategy\. 

```
aws appconfig get-configuration \
--application application_name_or_ID \
--environment environment_name_or_ID \
--configuration configuration_profile_name_or_ID \
--client-id client_ID \
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
AWS AppConfig uses the value of the `ClientConfigurationVersion` parameter to identify the configuration version on your clients\. If you don’t send `ClientConfigurationVersion` with each call to `GetConfiguration`, your clients receive the current configuration\. You are charged each time your clients receive a configuration\.  
To avoid excess charges, we recommend that you include the `ClientConfigurationVersion` value with every call to `GetConfiguration`\. This value must be saved on your client\. Subsequent calls to `GetConfiguration` must pass this value by using the `ClientConfigurationVersion` parameter, as shown here\. 

Sending `ConfigurationVersion` during subsequent polling for configuration updates is similar to the concept of [HTTP ETags](https://en.wikipedia.org/wiki/HTTP_ETag)\.

```
aws appconfig get-configuration \
--application application_name_or_ID \
--environment environment_name_or_ID \
--configuration configuration_profile_name_or_ID \
--client-configuration-version previous_configuration_version_value \
--client-id client_ID \
output_filename
```

**Note**  
We recommend tuning the polling frequency of your `GetConfiguration` API calls based on your budget, the expected frequency of your configuration deployments, and the number of targets for a configuration\.