# AWSSupport\-TroubleshootDirectoryTrust<a name="automation-awssupport-troubleshootdirectorytrust"></a>

 **Description** 

The AWSSupport\-TroubleshootDirectoryTrust Automation document diagnoses trust creation issues between an AWS Managed Microsoft AD and a Microsoft Active Directory\. The automation ensures the directory type supports trusts, and then checks the associated security group rules, network access control lists \(network ACLs\), and route tables for potential connectivity issues\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSSupport-TroubleshootDirectoryTrust)

**Document Type**

Automation

**Owner**

Amazon

**Platforms**

Windows, Linux

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Optional\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\. If no role is specified, Systems Manager Automation uses the permissions of the user that runs this document\.
+ DirectoryId

  Type: String

  Allowed pattern: ^d\-\[a\-z0\-9\]\{10\}$

  Description: \(Required\) The ID of the AWS Managed Microsoft AD to troubleshoot\.
+ RemoteDomainCidrs

  Type: StringList

  Allowed pattern: ^\(\(\[0\-9\]\|\[1\-9\]\[0\-9\]\|1\[0\-9\]\{2\}\|2\[0\-4\]\[0\-9\]\|25\[0\-5\]\)\\\.\)\{3\}\(\[0\-9\]\|\[1\-9\]\[0\-9\]\|1\[0\-9\]\{2\}\|2\[0\-4\]\[0\-9\]\|25\[0\-5\]\)\(\\/\(3\[0\-2\]\|\[1\-2\]\[0\-9\]\|\[1\-9\]\)\)$

  Description: \(Required\) The CIDR\(s\) of the remote domain you are attempting to establish a trust relationship with\. You can add multiple CIDRs using comma\-separated values\. For example, 172\.31\.48\.0/20, 192\.168\.1\.10/32\.
+ RemoteDomainName

  Type: String

  Description: \(Required\) The fully qualified domain name of the remote domain you are establishing a trust relationship with\.
+ RequiredTrafficACL

  Type: String

  Description: \(Required\) The default port requirements for AWS Managed Microsoft AD\. In most cases, you should not modify the default value\.

  Default: \{"inbound":\{"tcp":\[\[53,53\],\[88,88\],\[135,135\],\[389,389\],\[445,445\],\[464,464\],\[636,636\],\[1024,65535\]\],"udp":\[\[53,53\],\[88,88\],\[123\.123\],\[138,138\],\[389,389\],\[445,445\],\[464,464\]\],"icmp":\[\[\-1,\-1\]\]\},"outbound":\{"\-1":\[\[0,65535\]\]\}\}
+ RequiredTrafficSG

  Type: String

  Description: \(Required\) The default port requirements for AWS Managed Microsoft AD\. In most cases, you should not modify the default value\.

  Default: \{"inbound":\{"tcp":\[\[53,53\],\[88,88\],\[135,135\],\[389,389\],\[445,445\],\[464,464\],\[636,636\],\[1024,65535\]\],"udp":\[\[53,53\],\[88,88\],\[123\.123\],\[138,138\],\[389,389\],\[445,445\],\[464,464\]\],"icmp":\[\[\-1,\-1\]\]\},"outbound":\{"\-1":\[\[0,65535\]\]\}\}
+ TrustId

  Type: String

  Description: \(Optional\) The ID of the trust relationship to troubleshoot\.

**Required IAM Permissions**

The `AutomationAssumeRole` requires the following actions to successfully run the Automation document\.
+ `ds:DescribeConditionalForwarders`
+ `ds:DescribeDirectories`
+ `ds:DescribeTrusts`
+ `ds:ListIpRoutes`
+ `ec2:DescribeNetworkAcls`
+ `ec2:DescribeSecurityGroups`
+ `ec2:DescribeSubnets`

**Document Steps**
+ aws:assertAwsResourceProperty \- Confirms the directory type is AWS Managed Microsoft AD\.
+ aws:executeAwsApi \- Gets information about the AWS Managed Microsoft AD\.
+ aws:branch \- Branches workflow if a value is provided for the `TrustId` input parameter\.
+ aws:executeAwsApi \- Gets information about the trust relationship\.
+ aws:executeAwsApi \- Gets the conditional forwarder DNS IP addresses for the `RemoteDomainName`\.
+ aws:executeAwsApi \- Gets information about IP routes that have been added to the AWS Managed Microsoft AD\.
+ aws:executeAwsApi \- Gets the CIDRs of the AWS Managed Microsoft AD subnets\.
+ aws:executeAwsApi \- Gets information about the security groups associated with the AWS Managed Microsoft AD\.
+ aws:executeAwsApi \- Gets information about the network ACLs associated with the AWS Managed Microsoft AD\.
+ aws:executeScript \- Confirms the `RemoteDomainCidrs` are valid values\. Confirms that the AWS Managed Microsoft AD has conditional forwarders for the `RemoteDomainCidrs`, and that the requisite IP routes have been added to the AWS Managed Microsoft AD if the `RemoteDomainCidrs` are non\-RFC 1918 IP addresses\.
+ aws:executeScript \- Evaluates security group rules\.
+ aws:executeScript \- Evaluates network ACLs\.

**Outputs**

evalDirectorySecurityGroup\.output \- Results from evaluating whether the security group rules associated with the AWS Managed Microsoft AD allow the requisite traffic for trust creation\.

evalAclEntries\.output \- Results from evaluating whether the network ACLs associated with the AWS Managed Microsoft AD allow the requisite traffic for trust creation\.

evaluateRemoteDomainCidr\.output \- Results from evaluating whether the `RemoteDomainCidrs` are valid values\. Confirms that the AWS Managed Microsoft AD has conditional forwarders for the `RemoteDomainCidrs`, and that the requisite IP routes have been added to the AWS Managed Microsoft AD if the `RemoteDomainCidrs` are non\-RFC 1918 IP addresses\.