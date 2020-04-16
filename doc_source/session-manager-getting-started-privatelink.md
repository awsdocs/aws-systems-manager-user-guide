# Step 6: \(Optional\) use PrivateLink to set up a VPC endpoint for Session Manager<a name="session-manager-getting-started-privatelink"></a>

You can further improve the security posture of your managed instances by configuring AWS Systems Manager to use an interface VPC endpoint\. Interface endpoints are powered by AWS PrivateLink, a technology that enables you to privately access Amazon EC2 and Systems Manager APIs by using private IP addresses\. 

PrivateLink restricts all network traffic between your managed instances, Systems Manager, and Amazon EC2 to the Amazon network\. \(Managed instances don't have access to the internet\.\) Also, you don't need an internet gateway, a NAT device, or a virtual private gateway\. 

In addition to the three endpoints required to use PrivateLink with Systems Manager, you can create a fourth, **com\.amazonaws\.*region*\.ssmmessages**, for use with Session Manager\.

For more information, see [\(Optional\) Create a Virtual Private Cloud endpoint](setup-create-vpc.md)\.