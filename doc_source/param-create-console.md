# Create a Systems Manager parameter \(console\)<a name="param-create-console"></a>

You can use the AWS Systems Manager console to create a Systems Manager parameter\.

**Note**  
Parameters are only available in the AWS Region where they were created\.

**To create a parameter**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose **Create parameter**\.

1. For **Name**, type a hierarchy and a parameter name\. For example, type `/Test/helloWorld`\.

1. In the **Description** box, type a description that identifies this parameter as a test parameter\.

1. For **Parameter tier** choose either **Standard** or **Advanced**\. For more information about advanced parameters, see [Managing parameter tiers](parameter-store-advanced-parameters.md)\.

1. For **Type**, choose **String**, **StringList**, or **SecureString**\.
   + If you choose **String**, the **Data type** field appears\. If you are creating a parameter to hold the resource ID for an Amazon Machine Image \(AMI\), select `aws:ec2:image`\. Otherwise, leave the default `text` selected\.
   + If you choose **SecureString,** the **KMS Key ID** field appears\. If you don't provide a KMS customer master key \(CMK\) ID, a CMK ARN, an alias name, or an alias ARN, then the system uses `alias/aws/ssm`, which is the AWS managed CMK for Systems Manager\. If you don't want to use this key, then you can use a customer managed CMK\. For more information about `SecureString` parameters, see [SecureString parameters](sysman-paramstore-securestring.md)\. For more information about AWS managed and customer managed CMKs, see [AWS Key Management Service Concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html) in the *AWS Key Management Service Developer Guide*\. For more information about Parameter Store and KMS encryption, see [How AWS Systems Manager Parameter Store Uses AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/services-parameter-store.html)\.
   + When creating a `SecureString` parameter in the console by using the `key-id` parameter with either a customer managed CMK alias name or an alias ARN, you must specify the prefix `alias/` before the alias\. Here is an ARN example:

     ```
     arn:aws:kms:us-east-2:123456789012:alias/MyAliasName
     ```

     Here is an alias name example:

     ```
     alias/MyAliasName
     ```

1. In the **Value** box, type a value\. For example, type **This is my first parameter** or **ami\-0dbf5ea29aEXAMPLE**\.
**Note**  
Parameters can't be referenced or nested in the values of other parameters\. You can't include `{{}}` or `{{ssm:parameter-name}}` in a parameter value\.  
If you chose **SecureString**, the value of the parameter is masked by default \("\*\*\*\*\*\*"\) when you view it later on the parameter **Overview** tab\. Choose **Show** to display the parameter value\.  

![\[A SecureString parameter's value is masked on the Overview tab\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/ps-overview-show-secstring.png)

1. \(Optional\) In the **Tags** area, apply one or more tag key\-value pairs to the parameter\.

   Tags are optional metadata that you assign to a resource\. Tags enable you to categorize a resource in different ways, such as by purpose, owner, or environment\. For example, you might want to tag a Systems Manager parameter to identify the type of resource to which it applies, the environment, or the type of configuration data referenced by the parameter\. In this case, you could specify the following key\-value pairs:
   + `Key=Resource,Value=S3bucket`
   + `Key=OS,Value=Windows`
   + `Key=ParameterType,Value=LicenseKey`

1. Choose **Create parameter**\. 

1. In the parameters list, choose the name of the parameter you just created\. Verify the details on the **Overview** tab\. If you created a `SecureString`g parameter, choose **Show** to view the unencrypted value\.

**Note**  
You can’t change an advanced parameter to a standard parameter\. If you no longer need an advanced parameter, or if you no longer want to incur charges for an advanced parameter, you must delete it and recreate it as a new standard parameter\.