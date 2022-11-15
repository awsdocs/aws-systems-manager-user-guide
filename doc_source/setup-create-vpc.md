# Step 5: Create VPC endpoints<a name="setup-create-vpc"></a>

You can improve the security posture of your managed instances \(including managed instances in your hybrid environment\) by configuring AWS Systems Manager to use an interface VPC endpoint in Amazon Virtual Private Cloud \(Amazon VPC\)\. By using an interface VPC endpoint \(interface endpoint\), you can connect to services powered by AWS PrivateLink\. AWS PrivateLink is a technology that allows you to privately access Amazon Elastic Compute Cloud \(Amazon EC2\) and Systems Manager APIs by using private IP addresses\. 

AWS PrivateLink restricts all network traffic between your managed instances, Systems Manager, and Amazon EC2 to the Amazon network\. This means that your managed instances don't have access to the Internet\. If you use AWS PrivateLink, you don't need an internet gateway, a NAT device, or a virtual private gateway\. 

You aren't required to configure AWS PrivateLink, but it's recommended\. For more information about AWS PrivateLink and VPC endpoints, see [AWS PrivateLink and VPC endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/endpoint-services-overview.html)\.

**Note**  
The alternative to using a VPC endpoint is to allow outbound internet access on your managed instances\. In this case, the managed instances must also allow HTTPS \(port 443\) outbound traffic to the following endpoints:  
`ssm.region.amazonaws.com`
`ssmmessages.region.amazonaws.com`
`ec2messages.region.amazonaws.com`
SSM Agent initiates all connections to the Systems Manager service in the cloud\. For this reason, you don't need to configure your firewall to allow inbound traffic to your instances for Systems Manager\.  
For more information about calls to these endpoints, see [Reference: ec2messages, ssmmessages, and other API operations](systems-manager-setting-up-messageAPIs.md)\.

**About Amazon VPC**  
You can use Amazon Virtual Private Cloud \(Amazon VPC\) to define a virtual network in your own logically isolated area within the AWS Cloud, known as a *virtual private cloud \(VPC\)*\. You can launch your AWS resources, such as instances, into your VPC\. Your VPC closely resembles a traditional network that you might operate in your own data center, with the benefits of using the scalable infrastructure of AWS\. You can configure your VPC; you can select its IP address range, create subnets, and configure route tables, network gateways, and security settings\. You can connect instances in your VPC to the internet\. You can connect your VPC to your own corporate data center, making the AWS Cloud an extension of your data center\. To protect the resources in each subnet, you can use multiple layers of security, including security groups and network access control lists\. For more information, see the [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/)\.

**Topics**
+ [VPC endpoint restrictions and limitations](#vpc-requirements-and-limitations)
+ [Creating VPC endpoints for Systems Manager](#sysman-setting-up-vpc-create)
+ [Create an interface VPC endpoint policy](#sysman-endpoint-policies)

## VPC endpoint restrictions and limitations<a name="vpc-requirements-and-limitations"></a>

Before you configure VPC endpoints for Systems Manager, be aware of the following restrictions and limitations\.

**`aws:domainJoin` plugin**  
If you choose to create VPC endpoints, then be aware that requests to join a Windows Server instance to a domain from SSM documents that use the `aws:domainJoin` plugin will fail unless you allow traffic from your instance to the public AWS Directory Service endpoints\. This plugin requires the AWS Directory Service, and AWS Directory Service doesn't have PrivateLink endpoint support\. Support for joining a Windows Server instance to a domain from other methods of joining instances to a domain depends only on Active Directory requirements \(for example, ensuring that domain controllers are reachable and discoverable by using DNS and other related requirements\)\. You can use [Amazon EC2 User Data scripts](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-windows-user-data.html) to join an instance to a domain\.

**Cross\-Region requests**  
VPC endpoints don't support cross\-Region requests—ensure that you create your endpoint in the same AWS Region as your bucket\. You can find the location of your bucket by using the Amazon S3 console, or by using the [get\-bucket\-location](https://docs.aws.amazon.com/cli/latest/reference/s3api/get-bucket-location.html) command\. Use a Region\-specific Amazon S3 endpoint to access your bucket; for example, `DOC-EXAMPLE-BUCKET.s3-us-west-2.amazonaws.com`\. For more information about Region\-specific endpoints for Amazon S3, see [Amazon S3 endpoints](https://docs.aws.amazon.com/general/latest/gr/s3.html#s3_region) in the *Amazon Web Services General Reference*\. If you use the AWS CLI to make requests to Amazon S3, set your default region to the same region as your bucket, or use the `--region` parameter in your requests\.

**VPC peering connections**  
VPC interface endpoints can be accessed through both *intra\-Region* and *inter\-Region* VPC peering connections\. For more information about VPC peering connection requests for VPC interface endpoints, see [VPC peering connections \(Quotas\)](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html#vpc-limits-peering) in the *Amazon Virtual Private Cloud User Guide*\. 

VPC gateway endpoint connections can't be extended out of a VPC\. Resources on the other side of a VPC peering connection in your VPC can't use the gateway endpoint to communicate with resources in the gateway endpoint service\. For more information about VPC peering connection requests for VPC gateway endpoints, see [VPC endpoints \(Quotas\)](https://docs.aws.amazon.com/vpc/latest/userguide/amazon-vpc-limits.html#vpc-limits-endpoints) in the *Amazon Virtual Private Cloud User Guide*

**Incoming connections**  
The security group attached to the VPC endpoint must allow incoming connections on port 443 from the private subnet of the managed instance\. If incoming connections aren't allowed, then the managed instance can't connect to the SSM and EC2 endpoints\.

**Amazon S3 buckets**  
Your VPC endpoint policy must allow access to at least the following Amazon S3 buckets:
+ The S3 buckets used by Patch Manager for patch baseline operations in your AWS Region\. These buckets contain the code that is retrieved and run on instances by the patch baseline service\. Each AWS Region has its own patch baseline operations buckets from which the code is retrieved when a patch baseline document is run\. If the code can't be downloaded, the patch baseline command will fail\. 
**Note**  
If you use an on\-premises firewall and plan to use Patch Manager, that firewall must also allow access to the appropriate patch baseline endpoint\.

  To provide access to the buckets in your AWS Region, include the following permission in your endpoint policy\.

  ```
  arn:aws:s3:::patch-baseline-snapshot-region/*
  arn:aws:s3:::aws-ssm-region/*
  ```

  *region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

  See the following example\.

  ```
  arn:aws:s3:::patch-baseline-snapshot-us-east-2/*
  arn:aws:s3:::aws-ssm-us-east-2/*
  ```
**Note**  
In the Middle East \(Bahrain\) Region \(me\-south\-1\) *only*, these buckets use different naming conventions\. For this AWS Region *only*, use the following two buckets instead:  
`patch-baseline-snapshot-me-south-1-uduvl7q8`
`aws-patch-manager-me-south-1-a53fc9dce`
+ The S3 buckets listed in [SSM Agent communications with AWS managed S3 buckets](ssm-agent-minimum-s3-permissions.md)\.

**Amazon CloudWatch Logs**  
If you don't allow your instances to access the internet, create a VPC endpoint for CloudWatch Logs to use features that send logs to CloudWatch Logs\. For more information about creating an endpoint for CloudWatch Logs, see [Creating a VPC endpoint for CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch-logs-and-interface-VPC.html#create-VPC-endpoint-for-CloudWatchLogs) in the *Amazon CloudWatch Logs User Guide*\.

**DNS in hybrid environment**  
For information about configuring DNS to work with AWS PrivateLink endpoints in hybrid environments, see [Private DNS for interface endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-interface.html#vpce-private-dns) in the *Amazon VPC User Guide*\. If you want to use your own DNS, you can use Route 53 Resolver\. For more information, see [Resolving DNS queries between VPCs and your network](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver.html) in the *Amazon Route 53 Developer Guide*\. 

## Creating VPC endpoints for Systems Manager<a name="sysman-setting-up-vpc-create"></a>

Use the following information to create VPC interface and gateway endpoints for AWS Systems Manager\. This topic links to procedures in the *Amazon VPC User Guide*\. 

**To create VPC endpoints for Systems Manager**

In the first step of this procedure, you create three required and one optional *interface* endpoints for Systems Manager\. The first three endpoints are required for Systems Manager to work in a VPC\. The fourth, `com.amazonaws.region.ssmmessages`, is required only if you're using Session Manager capabilities\.

In the second step, you create the required *gateway* endpoint for Systems Manager to access Amazon S3\.
**Note**  
*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

1. Follow the steps in [Create an interface endpoint](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-interface.html#create-interface-endpoint) to create the following interface endpoints:
   + **`com.amazonaws.region.ssm`** – The endpoint for the Systems Manager service\.
   + **`com.amazonaws.region.ec2messages`** – Systems Manager uses this endpoint to make calls from SSM Agent to the Systems Manager service\.
   + **`com.amazonaws.region.ec2`** – If you're using Systems Manager to create VSS\-enabled snapshots, you need to ensure that you have an endpoint to the EC2 service\. Without the EC2 endpoint defined, a call to enumerate attached Amazon EBS volumes fails, which causes the Systems Manager command to fail\.
   + **`com.amazonaws.region.ssmmessages`** – This endpoint is required only if you're connecting to your instances through a secure data channel using Session Manager\. For more information, see [AWS Systems Manager Session Manager](session-manager.md) and [Reference: ec2messages, ssmmessages, and other API operations](systems-manager-setting-up-messageAPIs.md)\.
   + **`com.amazonaws.region.kms`** – This endpoint is optional\. However, it can be created if you want to use AWS Key Management Service \(AWS KMS\) encryption for Session Manager or Parameter Store parameters\.
   + **`com.amazonaws.region.logs`** – This endpoint is optional\. However, it can be created if you want to use Amazon CloudWatch Logs \(CloudWatch Logs\) for Session Manager, Run Command, or SSM Agent logs\.

1. Follow the steps in [Create a gateway endpoint](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-gateway.html#create-gateway-endpoint) to create the following gateway endpoint for Amazon S3\. 
   + **`com.amazonaws.region.s3`** – Systems Manager uses this endpoint to update SSM Agent and toperform patching operations\. Systems Manager also uses this endpoint for tasks like uploading output logs you choose to store in S3 buckets, retrieving scripts or other files you store in buckets, and so on\. If the security group associated with your instances restricts outbound traffic, you must add a rule to allow traffic to the prefix list for Amazon S3\. For more information, see [Modify your security group](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-gateway.html#vpc-endpoints-security) in the *AWS PrivateLink Guide*\.

## Create an interface VPC endpoint policy<a name="sysman-endpoint-policies"></a>

You can create policies for VPC interface endpoints for AWS Systems Manager in which you can specify:
+ The principal that can perform actions
+ The actions that can be performed
+ The resources that can have actions performed on them

For more information, see [Control access to services with VPC endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\.

Example: Allow users to only get and list command invocations

```
{
   "Statement":[
      {
         "Sid":"SSMCommandsReadOnly",
         "Principal":"*",
         "Action":[
            "ssm:ListCommands",
            "ssm:ListCommandInvocations",
            "ssm:GetCommandInvocation"
         ],
         "Effect":"Allow",
         "Resource":"*"
      }
   ]
}
```