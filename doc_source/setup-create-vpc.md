# Step 6: \(Optional\) Create a Virtual Private Cloud endpoint<a name="setup-create-vpc"></a>

You can improve the security posture of your managed instances \(including managed instances in your hybrid environment\) by configuring AWS Systems Manager to use an interface VPC endpoint in Amazon Virtual Private Cloud \(Amazon VPC\)\. An interface VPC endpoint \(interface endpoint\) enables you to connect to services powered by AWS PrivateLink, a technology that enables you to privately access Amazon EC2 and Systems Manager APIs by using private IP addresses\. PrivateLink restricts all network traffic between your managed instances, Systems Manager, and Amazon EC2 to the Amazon network\. \(Managed instances don't have access to the Internet\.\) Also, you don't need an Internet gateway, a NAT device, or a virtual private gateway\. 

You are not required to configure PrivateLink, but it's recommended\. For more information about PrivateLink and VPC endpoints, see [Accessing AWS Services Through PrivateLink](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html#what-is-privatelink)\.

**Note**  
The alternative to using a VPC endpoint is to enable outbound internet access on your managed instances\. In this case, the managed instances must also allow HTTPS \(port 443\) outbound traffic to the following endpoints:  
`ssm.region.amazonaws.com`
`ssmmessages.region.amazonaws.com`
`ec2messages.region.amazonaws.com`
For more information about calls to these endpoints, see [Reference: ec2messages, ssmmessages, and other API calls](systems-manager-setting-up-messageAPIs.md)\.

**About Amazon VPC**  
Amazon Virtual Private Cloud \(Amazon VPC\) enables you to define a virtual network in your own logically isolated area within the AWS cloud, known as a *virtual private cloud \(VPC\)*\. You can launch your AWS resources, such as instances, into your VPC\. Your VPC closely resembles a traditional network that you might operate in your own data center, with the benefits of using AWS's scalable infrastructure\. You can configure your VPC; you can select its IP address range, create subnets, and configure route tables, network gateways, and security settings\. You can connect instances in your VPC to the internet\. You can connect your VPC to your own corporate data center, making the AWS cloud an extension of your data center\. To protect the resources in each subnet, you can use multiple layers of security, including security groups and network access control lists\. For more information, see the [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/)\.

**Topics**
+ [VPC endpoint restrictions and limitations](#vpc-requirements-and-limitations)
+ [Creating VPC endpoints for Systems Manager](#sysman-setting-up-vpc-create)

## VPC endpoint restrictions and limitations<a name="vpc-requirements-and-limitations"></a>

Before you configure VPC endpoints for Systems Manager, be aware of the following restrictions and limitations\.

**aws:domainJoin plugin**  
If you choose to create VPC endpoints, then be aware that requests to join a Windows Server instance to a domain from SSM documents that use the `aws:domainJoin` plugin will fail\. This plugin requires the AWS Directory Service, and AWS Directory Service does not have PrivateLink endpoint support\. Support for joining a Windows Server instance to a domain from other domain join methods depend only on Active Directory requirements \(for example, ensuring that domain controllers are reachable and discoverable by using DNS and other related requirements\)\. You can use [Amazon EC2 User Data scripts](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-windows-user-data.html) to join an instance to a domain\.

**Cross\-region requests**  
VPC endpoints currently do not support cross\-region requests—ensure that you create your endpoint in the same region as your bucket\. You can find the location of your bucket by using the Amazon S3 console, or by using the [get\-bucket\-location](https://docs.aws.amazon.com/cli/latest/reference/s3api/get-bucket-location.html) command\. Use a Region\-specific Amazon S3 endpoint to access your bucket; for example, `DOC-EXAMPLE-BUCKET.s3-us-west-2.amazonaws.com`\. For more information about Region\-specific endpoints for Amazon S3, see [Amazon S3 Service Endpoints](https://docs.aws.amazon.com/general/latest/gr/s3.html#s3_region) in the *Amazon Web Services General Reference*\. If you use the AWS CLI to make requests to Amazon S3, set your default region to the same region as your bucket, or use the `--region` parameter in your requests\.

**VPC Peering connections**  
VPC interface endpoints can be accessed through both *intra\-Region* and *inter\-Region* VPC peering connections\. For more information about VPC peering connection requests for VPC interface endpoints, see [Interface Endpoints Properties and Limitations](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-interface-limitations) in the *Amazon Virtual Private Cloud User Guide*\. 

VPC Gateway Endpoint connections cannot be extended out of a VPC\. Resources on the other side of a VPC peering connection in your VPC cannot use the gateway endpoint to communicate with resources in the gateway endpoint service\. For more information about VPC peering connection requests for VPC gateway endpoints, see [Gateway Endpoint Limitations](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-gateway.html#vpc-endpoints-limitations) in the *Amazon Virtual Private Cloud User Guide*

**Incoming connections**  
The security group attached to the VPC endpoint must allow incoming connections on port 443 from the private subnet of the managed instance\. If incoming connections are not allowed, then the managed instance cannot connect to the SSM and EC2 endpoints\.

**Amazon S3 buckets**  
Your VPC endpoint policy must allow at least access to the following Amazon S3 buckets:
+ The S3 buckets used by Patch Manager for patch baseline operations in your AWS Region\. These buckets contain the code that is retrieved and run on instances by the patch baseline service\.Each AWS Region has its own patch baseline operations buckets from which the code is retrieved when a patch baseline document is run\. If the code can't be downloaded, the patch baseline command will fail\. 
**Note**  
If you use an on\-premises firewall and plan to use Patch Manager, that firewall must also allow access to the patch baseline endpoint indicated below\.

  To provide access to the buckets in your AWS Region, include the following permission in your endpoint policy:
**Note**  
In the Middle East \(Bahrain\) Region \(me\-south\-1\) only, these buckets use different naming conventions\. For this AWS Region only, use the following two buckets instead\.  
`patch-baseline-snapshot-me-south-1-uduvl7q8`
`aws-patch-manager-me-south-1-a53fc9dce`

  ```
  arn:aws:s3:::patch-baseline-snapshot-region/*
  arn:aws:s3:::aws-ssm-region/*
  ```

  *region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

  For example:

  ```
  arn:aws:s3:::patch-baseline-snapshot-us-east-2/*
  arn:aws:s3:::aws-ssm-us-east-2/*
  ```
+ The S3 buckets listed in [About minimum S3 Bucket permissions for SSM Agent](ssm-agent-minimum-s3-permissions.md)\.

**Amazon CloudWatch Logs**  
If you do not allow your instances to access the internet, you must create a VPC endpoint for CloudWatch Logs to use features that send logs to CloudWatch Logs\. For more information about creating an endpoint for CloudWatch Logs, see [Creating a VPC endpoint for CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch-logs-and-interface-VPC.html#create-VPC-endpoint-for-CloudWatchLogs) in the *Amazon CloudWatch Logs User Guide*\.

**DNS in hybrid environment**  
For information about configuring DNS to work with PrivateLink endpoints in hybrid environments, see [Private DNS](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-private-dns)\. If you want to use your own DNS, you can use Route 53 Resolver\. For more information, see [Resolving DNS Queries Between VPCs and Your Network](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver.html) in the *Amazon Route 53 Developer Guide*\. 

## Creating VPC endpoints for Systems Manager<a name="sysman-setting-up-vpc-create"></a>

Use the following information to create VPC interface and gateway endpoints for Systems Manager\. This topic links to procedures in the *Amazon VPC User Guide*\. 

**To create VPC endpoints for Systems Manager**

In the first step below, you create three required and one optional *interface* endpoints for Systems Manager\. The first three endpoints are required for Systems Manager to work in a VPC\. The fourth, `com.amazonaws.region.ssmmessages`, is required only if you are using Session Manager capabilities\.

In the second step, you create the required *gateway* endpoint for Systems Manager to access Amazon S3\.
**Note**  
*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

1. Follow the steps in [Creating an Interface Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) to create the following interface endpoints:
   + **com\.amazonaws\.*region*\.ssm**: The endpoint for the Systems Manager service\.
   + **com\.amazonaws\.*region*\.ec2messages**: Systems Manager uses this endpoint to make calls from SSM Agent to the Systems Manager service\.
   + **com\.amazonaws\.*region*\.ec2**: If you're using Systems Manager to create VSS\-enabled snapshots, you need to ensure that you have an endpoint to the EC2 service\. Without the EC2 endpoint defined, a call to enumerate attached EBS volumes fails, which causes the Systems Manager command to fail\.
   + **com\.amazonaws\.*region*\.ssmmessages**: This endpoint is required only if you are connecting to your instances through a secure data channel using Session Manager\. For more information, see [AWS Systems Manager Session Manager](session-manager.md) and [Reference: ec2messages, ssmmessages, and other API calls](systems-manager-setting-up-messageAPIs.md)\.

1. Follow the steps in [Creating a Gateway Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-gateway.html#create-gateway-endpoint) to create the following gateway endpoint for Amazon S3\. 
   + **com\.amazonaws\.*region*\.s3**: Systems Manager uses this endpoint to update SSM Agent and for tasks like uploading output logs you choose to store in S3 buckets, retrieving scripts or other files you store in buckets, and so on\.

Continue to [Step 7: \(Optional\) Create Systems Manager service roles](setup-service-role.md)\.