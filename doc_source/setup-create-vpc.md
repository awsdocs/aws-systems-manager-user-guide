# Step 6: \(Optional\) Create a Virtual Private Cloud Endpoint<a name="setup-create-vpc"></a>

You can improve the security posture of your managed instances \(including managed instances in your hybrid environment\) by configuring AWS Systems Manager to use an interface VPC endpoint\. Interface endpoints are powered by AWS PrivateLink, a technology that enables you to privately access Amazon EC2 and Systems Manager APIs by using private IP addresses\. PrivateLink restricts all network traffic between your managed instances, Systems Manager, and Amazon EC2 to the Amazon network\. \(Managed instances don't have access to the Internet\.\) Also, you don't need an Internet gateway, a NAT device, or a virtual private gateway\. 

You are not required to configure PrivateLink, but it's recommended\. For more information about PrivateLink and VPC endpoints, see [Accessing AWS Services Through PrivateLink](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html#what-is-privatelink)\.

**Note**  
The alternative to using a VPC is to enable outbound internet access on your managed instances\.

**Topics**
+ [VPC Endpoint Restrictions and Limitations](#vpc-requirements-and-limitations)
+ [Creating VPC EndPoints for Systems Manager](#sysman-setting-up-vpc-create)

## VPC Endpoint Restrictions and Limitations<a name="vpc-requirements-and-limitations"></a>

Before you configure VPC endpoints for Systems Manager, be aware of the following restrictions and limitations\.

**aws:domainJoin plugin**  
If you choose to create VPC endpoints, then be aware that requests to join a Windows instance to a domain from SSM documents that use the aws:domainJoin plugin will fail\. This plugin requires the AWS Directory Service, and AWS Directory Service does not have PrivateLink endpoint support\. Support for joining a Windows instance to a domain from other domain join methods depend only on Active Directory requirements \(for example, ensuring that domain controllers are reachable and discoverable by using DNS and other related requirements\)\. You can use [Amazon EC2 User Data scripts](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-windows-user-data.html) to join an instance to a domain\.

**Cross\-region requests**  
VPC endpoints currently do not support cross\-region requests—ensure that you create your endpoint in the same region as your bucket\. You can find the location of your bucket by using the Amazon S3 console, or by using the [get\-bucket\-location](https://docs.aws.amazon.com/cli/latest/reference/s3api/get-bucket-location.html) command\. Use a region\-specific Amazon S3 endpoint to access your bucket; for example, `mybucket.s3-us-west-2.amazonaws.com`\. For more information about region\-specific endpoints for Amazon S3, see [Amazon Simple Storage Service \(S3\)](https://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region) in *Amazon Web Services General Reference*\. If you use the AWS CLI to make requests to Amazon S3, set your default region to the same region as your bucket, or use the `--region` parameter in your requests\.

**Incoming connections**  
The security group attached to the VPC endpoint must allow incoming connections on port 443 from the private subnet of the managed instance\. If incoming connections are not allowed, then the managed instance cannot connect to the SSM and EC2 endpoints\.

**Amazon S3 buckets**  
Your VPC endpoint policy must allow at least access to the following Amazon S3 buckets:
+ The S3 buckets used by Patch Manager for patch baseline operations in your AWS Region\. These buckets contain the code that is retrieved and run on instances by the patch baseline service\. Each AWS Region has its own patch baseline operations buckets for the code to be retrieved when a patch baseline document is run\. If the code can't be downloaded, the patch baseline command will fail\. 

  To provide access to the buckets in your AWS Region, include the following permission in your endpoint policy:

  ```
  arn:aws:s3:::patch-baseline-snapshot-region/*
  arn:aws:s3:::aws-ssm-region/*
  ```

  *region* represents the Region identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager Table of Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) topic in the *AWS General Reference*\.

  For example:

  ```
  arn:aws:s3:::patch-baseline-snapshot-us-east-2/*
  arn:aws:s3:::aws-ssm-us-east-2/*
  ```
+ The S3 buckets listed in [About Minimum S3 Bucket Permissions for SSM Agent](ssm-agent-minimum-s3-permissions.md)\.

**DNS in hybrid environment**  
For information about configuring DNS to work with PrivateLink endpoints in hybrid environments, see [Private DNS](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-private-dns)\. If you want to use your own DNS, you can use Route 53 Resolver\. For more information, see [Resolving DNS Queries Between VPCs and Your Network](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver.html) in the *Amazon Route 53 Developer Guide*\. 

## Creating VPC EndPoints for Systems Manager<a name="sysman-setting-up-vpc-create"></a>

Use the following procedure to create three required and one optional separate VPC endpoints for Systems Manager\. All three endpoints are required for Systems Manager to work in a VPC\. The fourth is required only if you are using Session Manager capabilities\. This procedure links to related procedures in the Amazon VPC User Guide\. 

**To create VPC endpoints for Systems Manager**

1. Follow the steps in [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) to create the following endpoints:
   + **com\.amazonaws\.*region*\.ssm**: The endpoint for the Systems Manager service\.
   + **com\.amazonaws\.*region*\.ec2messages**: Systems Manager uses this endpoint to make calls from SSM Agent to the Systems Manager service\.
   + **com\.amazonaws\.*region*\.ec2**: If you're using Systems Manager to create VSS\-enabled snapshots, you need to ensure that you have an endpoint to the EC2 service\. Without the EC2 endpoint defined, a call to enumerate attached EBS volumes fails, which causes the Systems Manager command to fail\. For more information about using Systems Manager to create VSS\-enabled snapshots, see [Using Run Command to Take VSS\-Enabled Snapshots of EBS Volumes](integration-vss.md)\.
   + **com\.amazonaws\.*region*\.ssmmessages**: This endpoint is required only if you are connecting to your instances through a secure data channel using Session Manager\. For more information, see [AWS Systems Manager Session Manager](session-manager.md)\.

   *region* represents the Region identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in the [AWS Systems Manager Table of Regions and Endpoints](https://docs.aws.amazon.com/general/latest/gr/rande.html#ssm_region) topic in the *AWS General Reference*\.

1. Follow the steps in [Creating a Gateway Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-gateway.html#create-gateway-endpoint) to create an endpoint for Amazon S3\. Systems Manager uses this endpoint to upload Amazon S3 output logs, and to update SSM Agent\.

Continue to [Step 7: \(Optional\) Create Systems Manager Service Roles](setup-service-role.md)\.