# Step 6: \(Optional\) Use AWS PrivateLink to set up a VPC endpoint for Session Manager<a name="session-manager-getting-started-privatelink"></a>

You can further improve the security posture of your managed instances by configuring AWS Systems Manager to use an interface virtual private cloud \(VPC\) endpoint\. Interface endpoints are powered by AWS PrivateLink, a technology that enables you to privately access Amazon Elastic Compute Cloud \(Amazon EC2\) and Systems Manager APIs by using private IP addresses\. 

AWS PrivateLink restricts all network traffic between your managed instances, Systems Manager, and Amazon EC2 to the Amazon network\. \(Managed instances don't have access to the internet\.\) Also, you don't need an internet gateway, a NAT device, or a virtual private gateway\. 

For information about creating a VPC, see [\(Optional\) Create a Virtual Private Cloud endpoint](setup-create-vpc.md)\.

The alternative to using a VPC endpoint is to enable outbound internet access on your managed instances\. In this case, the managed instances must also allow HTTPS \(port 443\) outbound traffic to the following endpoints:
+ `ec2messages.region.amazonaws.com`
+ `ssm.region.amazonaws.com`
+ `ssmmessages.region.amazonaws.com`

Systems Manager uses the last of these endpoints, `ssmmessages.region.amazonaws.com`, to make calls from SSM Agent to the Session Manager service in the cloud\.

For more information about required endpoints for Systems Manager, see [Reference: ec2messages, ssmmessages, and other API calls](systems-manager-setting-up-messageAPIs.md)\.