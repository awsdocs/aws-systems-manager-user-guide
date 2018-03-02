# Setting Up VPC Endpoints for Systems Manager<a name="sysman-setting-up-vpc"></a>

You can improve the security posture of your managed instances \(including managed instances in your hybrid environment\) by configuring AWS Systems Manager to use an interface VPC endpoint\. Interface endpoints are powered by PrivateLink, a technology that enables you to privately access Amazon EC2 and Systems Manager APIs by using private IP addresses\. PrivateLink restricts all network traffic between your managed instances, Systems Manager, and EC2 to the Amazon network \(managed instances don't have access to the Internet\)\. Also, you don't need an Internet gateway, a NAT device, or a virtual private gateway\. 

You are not required to configure PrivateLink, but it's recommended\. For more information about PrivateLink and VPC endpoints, see [Accessing AWS Services Through PrivateLink](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_Introduction.html#what-is-privatelink)\.

**Before You Begin**  
Before you configure VPC endpoints for Systems Manager, be aware of the following restrictions and limitations\.

+ VPC endpoints do not support Active Directory directory service, Amazon CloudWatch Events, or Amazon CloudWatch Logs\. If you configure your managed instances to use a VPC endpoint, you won't be able to use these services\.

+ VPC endpoints currently do not support cross\-region requests—ensure that you create your endpoint in the same region as your bucket\. You can find the location of your bucket by using the Amazon S3 console, or by using the [get\-bucket\-location](http://docs.aws.amazon.com/cli/latest/reference/s3api/get-bucket-location.html) command\. Use a region\-specific Amazon S3 endpoint to access your bucket; for example, `mybucket.s3-us-west-2.amazonaws.com`\. For more information about region\-specific endpoints for Amazon S3, see [Amazon Simple Storage Service \(S3\)](http://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region) in *Amazon Web Services General Reference*\. If you use the AWS CLI to make requests to Amazon S3, set your default region to the same region as your bucket, or use the `--region` parameter in your requests\.

+ VPC endpoints only support Amazon\-provided DNS through Route 53\. If you want to use your own DNS, you can use conditional DNS forwarding\. For more information, see [DHCP Options Sets](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_DHCP_Options.html) in the Amazon VPC User Guide\.

## Creating VPC EndPoints for Systems Manager<a name="sysman-setting-up-vpc-create"></a>

Use the following procedure to create three separate VPC endpoints for Systems Manager\. All three endpoints are required for Systems Manager to work in a VPC\. This procedure links to related procedures in the Amazon VPC User Guide\. 

**To create VPC endpoints for Systems Manager**

1. Use [Creating an Interface Endpoint](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-interface.html#create-interface-endpoint) to create the following endpoints:

   + **com\.amazonaws\.*region*\.ssm**: The endpoint for the Systems Manager service\.

   + **com\.amazonaws\.*region*\.ec2messages**: Systems Manager uses this endpoint to make calls from SSM Agent to the Systems Manager service\.

   *region* represents the region identifier for an AWS region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager table of regions and endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) in the *AWS General Reference*\.

1. Use [Creating a Gateway Endpoint](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpce-gateway.html#create-gateway-endpoint) to create an endpoint for Amazon S3\. Systems Manager uses this endpoint to upload Amazon S3 output logs, and to update SSM Agent\.