# Sharing SSM documents<a name="ssm-sharing"></a>

You can share SSM documents privately or publicly with accounts in the same AWS Region\. To privately share a document, you modify the document permissions and allow specific individuals to access it according to their Amazon Web Services \(AWS\) ID\. To publicly share an SSM document, you modify the document permissions and specify `All`\. 

**Warning**  
Use shared SSM documents only from trusted sources\. When using any shared document, carefully review the contents of the document before using it so that you understand how it will change the configuration of your instance\. For more information about shared document best practices, see [Best practices for shared SSM documents](ssm-before-you-share.md)\. 

**Limitations**  
As you begin working with SSM documents, be aware of the following limitations\.
+ Only the owner can share a document\.
+ You must stop sharing a document before you can delete it\. For more information, see [Modify permissions for a shared SSM document](ssm-share-modify.md)\.
+ You can share a document with a maximum of 1000 AWS accounts\. You can request an increase to this limit in the [AWS Support Center](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase)\. For **Limit type**, choose *EC2 Systems Manager* and describe your reason for the request\.
+ You can publicly share a maximum of five SSM documents\. You can request an increase to this limit in the [AWS Support Center](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase)\. For **Limit type**, choose *EC2 Systems Manager* and describe your reason for the request\.
+ Documents can be shared with other accounts in the same AWS Region only\. Cross\-Region sharing is not supported\.

For more information about Systems Manager service quotas, see [AWS Systems Manager Service Quotas](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_ssm)\.

**Topics**
+ [Best practices for shared SSM documents](ssm-before-you-share.md)
+ [Share an SSM document](ssm-how-to-share.md)
+ [Modify permissions for a shared SSM document](ssm-share-modify.md)
+ [Using shared SSM documents](ssm-using-shared.md)