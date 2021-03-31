# AWSSupport\-ConnectivityTroubleshooter<a name="automation-awssupport-connectivitytroubleshooter"></a>

 **Description** 

The AWSSupport\-ConnectivityTroubleshooter runbook diagnoses connectivity issues between the following:
+ AWS resources within an Amazon Virtual Private Cloud \(Amazon VPC\)
+ AWS resources in different Amazon VPCs within the same AWS Region that are connected using VPC peering
+ AWS resources in an Amazon VPC and an internet resource using an internet gateway
+ AWS resources in an Amazon VPC and an internet resource using a network address translation \(NAT\) gateway

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-ConnectivityTroubleshooter)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ DestinationIP

  Type: String

  Description: \(Required\) The IPv4 address of the resource you want to connect to\.
+ DestinationPort

  Type: String

  Default: True

  Description: \(Required\) The port number you want to connect to on the destination resource\.
+ DestinationVpc

  Type: String

  Default: All

  Description: \(Optional\) The ID of the Amazon VPC you want to test connectivity to\.
+ SourceIP

  Type: String

  Description: \(Required\) The private IPv4 address of the AWS resource in your Amazon VPC you want to test connectivity from\.
+ SourcePortRange

  Type: String

  Description: \(Optional\) The port range used by the AWS resource in your Amazon VPC you want to test connectivity from\.
+ SourceVpc

  Type: String

  Default: All

  Description: \(Optional\) The ID of the Amazon VPC you want to test connectivity from\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ec2:DescribeNatGateways`
+ `ec2:DescribeNetworkAcls`
+ `ec2:DescribeNetworkInterfaces`
+ `ec2:DescribeRouteTables`
+ `ec2:DescribeSecurityGroups`
+ `ec2:DescribeVpcPeeringConnections`

**Document Steps**
+ aws:executeScript \- Gathers details about the AWS resource you specify in the `SourceIP` parameter\.
+ aws:executeScript \- Determines the destination of network traffic from the AWS resource using the routes gathered from the previous step\.
+ aws:branch \- Branches based on the destination of the network traffic\.
+ aws:executeAwsApi \- Gathers details about the destination resource\.
+ aws:executeScript \- Confirms that the ID returned for the destination Amazon VPC matches the value specified, if any, in the `DestinationVpc` parameter\.
+ aws:executeAwsApi \- Gathers the security group rules for the source and destination resources\.
+ aws:executeScript \- Confirms whether the security group rules allow the needed traffic between the source and destination resources\.
+ aws:executeAwsApi \- Gathers the network access control lists \(NACLs\) associated with the subnets for the source and destination resources\.
+ aws:executeScript \- Confirms whether the NACLs allow the needed traffic between the source and destination resources\.
+ aws:executeScript \- Confirms whether the source has a public IP address associated with the resource, if the route destination is an internet gateway\.
+ aws:executeAwsApi \- Gathers the security group rules for the source resource\.
+ aws:executeScript \- Confirms whether the security group rules allow the needed traffic from the source to the destination resource\.
+ aws:executeAwsApi \- Gathers the NACLs associated with the subnet for the source resource\.
+ aws:executeScript \- Confirms whether the NACLs allow the needed traffic from the source resource\.
+ aws:executeAwsApi \- Gathers details about the NAT gateway\.
+ aws:executeAwsApi \- Gathers the NACLs associated with the subnet for the NAT gateway\.
+ aws:executeScript \- Confirms whether the NACLs allow the needed traffic from the subnet for the NAT gateway\.
+ aws:executeScript \- Gathers the routes associated with the subnet for the NAT gateway\.
+ aws:executeScript \- Confirms whether the NAT gateway has a route to an internet gateway\.
+ aws:executeAwsApi \- Gathers details about the VPC peering connection\.
+ aws:executeScript \- Confirms both VPCs are in the same Region and that the ID returned for the destination VPC matches the value specified, if any, in the `DestinationVpc` parameter\.
+ aws:executeAwsApi \- Returns the subnet of the destination resource\.
+ aws:executeScript \- Gathers the routes associated with the subnet for the peered VPC\.
+ aws:executeScript \- Confirms whether the peered VPC has a route to the peering connection\.
+ aws:executeScript \- Confirms whether traffic is allowed from the source resource if the destination is not supported by the automation\.