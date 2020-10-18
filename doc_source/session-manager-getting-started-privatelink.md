# Step 6: \(Optional\) Use AWS PrivateLink to set up a VPC endpoint for Session Manager<a name="session-manager-getting-started-privatelink"></a>

You can further improve the security posture of your managed instances by configuring AWS Systems Manager to use an interface virtual private cloud \(VPC\) endpoint\. Interface endpoints are powered by AWS PrivateLink, a technology that enables you to privately access Amazon EC2 and Systems Manager APIs by using private IP addresses\. 

AWS PrivateLink restricts all network traffic between your managed instances, Systems Manager, and Amazon EC2 to the Amazon network\. \(Managed instances don't have access to the internet\.\) Also, you don't need an internet gateway, a NAT device, or a virtual private gateway\. 

AWS PrivateLink with Systems Manager, requires the managed instances allow HTTPS (port 443) outbound traffic to the following endpoints:
   + **com\.amazonaws\.*region*\.ssm**: The endpoint for the Systems Manager service\.
   + **com\.amazonaws\.*region*\.ec2messages**: Systems Manager uses this endpoint to make calls from SSM Agent to the Systems Manager service\.
   + **com\.amazonaws\.*region*\.ssmmessages**: This endpoint is required only if you are connecting to your instances through a secure data channel using Session Manager\. For more information, see [AWS Systems Manager Session Manager](session-manager.md) and [Reference: ec2messages, ssmmessages, and other API calls](systems-manager-setting-up-messageAPIs.md)\.

For more information, see [\(Optional\) Create a Virtual Private Cloud endpoint](setup-create-vpc.md)\.