# Creating a configuration and a configuration profile<a name="appconfig-creating-configuration-and-profile-console"></a>

Use the following procedure to create an AppConfig configuration and a configuration profile by using the AWS Systems Manager console\.

**To create a configuration profile**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/appconfig/](https://console.aws.amazon.com/systems-manager/appconfig/)\.

1. On the **Applications** tab, choose the application you created in [Create an AppConfig Configuration](appconfig-creating-application.md) and then choose the **Configuration profiles** tab\.

1. Choose **Create configuration profile**\.

1. For **Name**, enter a name for the configuration profile\.

1. For **Description**, enter information about the configuration profile\.

1. In the **Service role** section, choose **New service role** to have AppConfig create the IAM role that provides access to the configuration data\. AppConfig automatically populates the **Role name** field based on the name you entered earlier\. Or, to choose a role that already exists in IAM, choose **Existing service role**\. Choose the role by using the **Role ARN** list\.

1. In the **Tags** section, enter a key and an optional value\. You can specify a maximum of 50 tags for a resource\. 

1. On the **Select configuration source** page, choose an option\.  
![\[Choose an AWS AppConfig configuration source\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/appconfig-profile-1.png)

1. If you selected **Amazon S3 object**, then enter the object URI\. If you selected **AWS Systems Manager parameter**, then choose the name of the parameter from the list\. If you selected **AWS Systems Manager document**, then complete the following steps\. 

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

AppConfig creates the configuration profile\. Proceed to [Step 4: Create a deployment strategy](appconfig-creating-deployment-strategy.md)\.