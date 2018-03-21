# Create a Systems Manager Parameter \(Console\)<a name="param-create-console"></a>

You can use the Amazon EC2 console or AWS Systems Manager console to create a Systems Manager parameter\.

**Note**  
Parameters are only available in the Region where they were created\.

Depending on the service you are using, AWS Systems Manager or Amazon EC2 Systems Manager, use one of the following procedures:

**To create a parameter \(AWS Systems Manager\)**

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Parameter Store**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Parameter Store**\.

1. Choose **Create parameter**\.

1. For **Name**, type a hierachy and a parameter name\. For example, type `/Test/helloWorld`\.

1. In the **Description** box, type a description that identifies this parameter as a test parameter\.

1. For **Type**, choose **String**, **StringList**, or **SecureString**\.
**Note**  
If you choose **SecureString,** the **KMS Key ID** field appears\. If you don't provide a KMS CMK ID, a KMS CMK ARN, an alias name, or an alias ARN, then the system uses alias/aws/ssm \(Default\), which is the default KMS CMK for Systems Manager\. If you don't want to use this key, then you can choose a custom key\. For more information, see [Use Secure String Parameters](sysman-paramstore-about.md#sysman-paramstore-securestring)\.

1. In the **Value** box, type a value\. For example, type `MyFirstParameter`\. If you chose **Secure String**, the value is masked as you type\.

1. Choose **Create parameter**\. 

1. In the parameters list, choose the name of the parameter you just created\. Verify the details on the **Overview** tab\. If you created a `SecureString` parameter, choose **Show** to view the unencrypted value\.

**To create a parameter \(Amazon EC2 console\)**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Shared Resources** in the navigation pane, and then choose **Parameter Store**\. 

1. Choose **Create Parameter**\.

1. For **Name**, type a hierachy and a parameter name\. For example, type `/Test/helloWorld`\.

1. In the **Description** box, type a description that identifies this parameter as a test parameter\.

1. For **Type**, choose **String**, **String List**, or **Secure String**\.
**Note**  
If you choose **SecureString,** the **KMS Key ID** field appears\. If you don't provide a KMS CMK ID, a KMS CMK ARN, an alias name, or an alias ARN, then the system uses alias/aws/ssm \(Default\), which is the default KMS CMK for Systems Manager\. If you don't want to use this key, then you can choose a custom key\. For more information, see [Use Secure String Parameters](sysman-paramstore-about.md#sysman-paramstore-securestring)\.

1. In the **Value** box, type a value\. For example, type `MyFirstParameter`\. If you chose **Secure String**, the value is masked as you type\.

1. Choose **Create Parameter**\. After the system creates the parameter, choose **Close**\. 

1. In the parameters list, choose the parameter you just created\. Verify the details on the **Description** tab\. If you created a `SecureString` parameter, choose **Show** to view the unencrypted value\.