# Sharing Systems Manager Documents<a name="ssm-sharing"></a>

You can share Systems Manager documents privately or publicly\. To privately share a document, you modify the document permissions and allow specific individuals to access it according to their Amazon Web Services \(AWS\) ID\. To publicly share a Systems Manager document, you modify the document permissions and specify `All`\. 

**Warning**  
Use shared Systems Manager documents only from trusted sources\. When using any shared document, carefully review the contents of the document before using it so that you understand how it will change the configuration of your instance\. For more information about shared document best practices, see [Guidelines for Sharing and Using Shared Systems Manager Documents](ssm-before-you-share.md)\. 

**Limitations**  
As you begin working with Systems Manager documents, be aware of the following limitations\.
+ Only the owner can share a document\.
+ You must stop sharing a document before you can delete it\. For more information, see [Modify Permissions for a Shared Document](ssm-share-modify.md)\.
+ You can share a document with a maximum of 1000 AWS accounts\. To increase this limit, go to [AWS Support Center](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-ec2-instances) and submit a limit increase request form\.
+ You can publicly share a maximum of five Systems Manager documents\. To increase this limit, go to [AWS Support Center](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-ec2-instances) and submit a limit increase request form\.

For more information about Systems Manager limits, see [AWS Systems Manager Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_ssm)\.

**Topics**
+ [Guidelines for Sharing and Using Shared Systems Manager Documents](ssm-before-you-share.md)
+ [Share a Systems Manager Document](ssm-how-to-share.md)
+ [Modify Permissions for a Shared Document](ssm-share-modify.md)
+ [Use a Shared Systems Manager Document](ssm-using-shared.md)