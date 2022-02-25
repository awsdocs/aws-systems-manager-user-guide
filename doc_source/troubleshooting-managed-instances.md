# Troubleshooting managed node availability<a name="troubleshooting-managed-instances"></a>

For several AWS Systems Manager capabilities like Run Command, Distributor, and Session Manager, you can choose to manually select the managed nodes on which you want to run an operation\. In cases like these, after you specify that you want to choose nodes manually, the system displays a list of managed nodes where you can run the operation\.

This topic provides information to help you diagnose why a managed node *that you have confirmed is running* isn't included in your lists of managed nodes in Systems Manager\. 

In order for a node to be managed by Systems Manager and available in lists of managed nodes, it must meet three requirements:
+ SSM Agent must be installed and running on the node with a supported operating system\.
**Note**  
Some Amazon Machine Images \(AMIs\) are configured to launch managed nodes with [SSM Agent](ssm-agent.md) preinstalled\. \(You can also configure a custom AMI to preinstall SSM Agent\.\)   
SSM Agent is preinstalled, by default, on the following Amazon Machine Images \(AMIs\):  
Amazon Linux
Amazon Linux 2
Amazon Linux 2 ECS\-Optimized Base AMIs
macOS 10\.14\.x \(Mojave\), 10\.15\.x \(Catalina\), and 11\.x \(Big Sur\)
SUSE Linux Enterprise Server \(SLES\) 12 and 15
Ubuntu Server 16\.04, 18\.04, and 20\.04  
Windows Server 2008\-2012 R2 AMIs published in November 2016 or later
Windows Server 2016 and 2019
+ For Amazon Elastic Compute Cloud \(Amazon EC2\) instances, you must attach an AWS Identity and Access Management \(IAM\) instance profile to the instance\. The instance profile enables the instance to communicate with the Systems Manager service\. If you don't assign an instance profile to the instance, you register it using a hybrid activation, which is not a common scenario\.
+ SSM Agent must be able to connect to a Systems Manager endpoint in order to register itself with the service\. Thereafter, the managed node must be available to the service, which is confirmed by the service sending a signal every five minutes to check the instance's health\. 

After you verify that a managed node is running, you can use the following command to check whether SSM Agentsuccessfully registered with the Systems Manager service\. This command doesn't return results until a successful registration has taken place\.

------
#### [ Linux & macOS ]

```
aws ssm describe-instance-associations-status \
    --instance-id instance-id
```

------
#### [ Windows ]

```
aws ssm describe-instance-associations-status ^
    --instance-id instance-id
```

------
#### [ PowerShell ]

```
Get-SSMInstanceAssociationsStatus `
    -InstanceId instance-id
```

------

If registration was successful and the managed node is now available for Systems Manager operations, the command returns results similar to the following\.

```
{
    "InstanceAssociationStatusInfos": [
        {
            "AssociationId": "fa262de1-6150-4a90-8f53-d7eb5EXAMPLE",
            "Name": "AWS-GatherSoftwareInventory",
            "DocumentVersion": "1",
            "AssociationVersion": "1",
            "InstanceId": "i-02573cafcfEXAMPLE",
            "Status": "Pending",
            "DetailedStatus": "Associated"
        },
        {
            "AssociationId": "f9ec7a0f-6104-4273-8975-82e34EXAMPLE",
            "Name": "AWS-RunPatchBaseline",
            "DocumentVersion": "1",
            "AssociationVersion": "1",
            "InstanceId": "i-02573cafcfEXAMPLE",
            "Status": "Queued",
            "AssociationName": "SystemAssociationForScanningPatches"
        }
    ]
}
```

If registration hasn't completed yet or was unsuccessful, the command returns results similar to the following:

```
{
    "InstanceAssociationStatusInfos": []
}
```

If the command doesn't return results after 5 minutes or so, use the following information to help you troubleshoot problems with your managed nodes\.

**Topics**
+ [Solution 1: Verify that SSM Agent is installed and running on the managed node](#instances-missing-solution-1)
+ [Solution 2: Verify that an IAM instance profile has been specified for the instance \(EC2 instances only\)](#instances-missing-solution-2)
+ [Solution 3: Verify service endpoint connectivity](#instances-missing-solution-3)
+ [Solution 4: Verify target operating system support](#instances-missing-solution-4)
+ [Solution 5: Verify you're working in the same AWS Region as the Amazon EC2 instance](#instances-missing-solution-5)
+ [Solution 6: Verify the proxy configuration you applied to the SSM Agent on your managed node](#instances-missing-solution-6)
+ [Solution 7: Install a TLS certificate on managed instances](#hybrid-tls-certificate)
+ [Troubleshooting Amazon EC2 managed instance availability using `ssm-cli`](ssm-cli.md)

## Solution 1: Verify that SSM Agent is installed and running on the managed node<a name="instances-missing-solution-1"></a>

Make sure the latest version of SSM Agent is installed and running on the managed node\.

To determine whether SSM Agent is installed and running on a managed node, see [Checking SSM Agent status and starting the agent](ssm-agent-status-and-restart.md)\.

To install or reinstall SSM Agent on a managed node, see the following topics:
+ [Installing and configuring SSM Agent on EC2 instances for Linux](sysman-install-ssm-agent.md)
+ [Install SSM Agent for a hybrid environment \(Linux\)](sysman-install-managed-linux.md)
+ [Installing and configuring SSM Agent on EC2 instances for Windows Server](sysman-install-ssm-win.md)
+ [Install SSM Agent for a hybrid environment \(Windows\) ](sysman-install-managed-win.md)

## Solution 2: Verify that an IAM instance profile has been specified for the instance \(EC2 instances only\)<a name="instances-missing-solution-2"></a>

For Amazon Elastic Compute Cloud \(Amazon EC2\) instances, verify that the instance is configured with an AWS Identity and Access Management \(IAM\) instance profile that allows the instance to communicate with the Systems Manager API\. Also verify that your user account has an IAM user trust policy that allows your account to communicate with the Systems Manager API\.

**Note**  
On\-premises servers, edge devices, and virtual machines \(VMs\) use an IAM service role instead of an instance profile\. For more information, see [Create an IAM service role for a hybrid environment](sysman-service-role.md)\.



**To determine whether an instance profile with the necessary permissions is attached to an EC2 instance**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, choose **Instances**\.

1. Choose the instance to check for an instance profile\.

1. On the **Description** tab in the bottom pane, locate **IAM role** and choose the name of the role\.

1. On the role **Summary** page for the instance profile, on the **Permissions** tab, ensure that `AmazonSSMManagedInstanceCore` is listed under **Permissions policies**\.

   If a custom policy is used instead, ensure that it provides the same permissions as `AmazonSSMManagedInstanceCore`\.

   [Open `AmazonSSMManagedInstanceCore` in the console](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore$jsonEditor)

   For information about other policies that can be attached to an instance profile for Systems Manager, see [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)\.

**To create and attach an instance profile with the necessary permissions to a new EC2 instance**
+ Complete the tasks in the following topics:
  + [Create an IAM instance profile for Systems Manager](setup-instance-profile.md)
  + [Launch an instance that uses the Systems Manager instance profile \(console\)](setup-launch-managed-instance.md#setup-launch-managed-instance-new)

**To attach an existing instance profile with the necessary permissions to an existing Amazon EC2 instance**
+ Complete the task in the following topic:
  + [Attach the Systems Manager instance profile to an existing instance \(console\)](setup-launch-managed-instance.md#setup-launch-managed-instance-existing)

## Solution 3: Verify service endpoint connectivity<a name="instances-missing-solution-3"></a>

Verify that the instance has connectivity to the Systems Manager service endpoints\. This connectivity is provided by creating and configuring VPC endpoints for Systems Manager, or by allowing HTTPS \(port 443\) outbound traffic to the service endpoints\.

For Amazon EC2 instances, the Systems Manager service endpoint for the AWS Region the instance is used to register the instance if your virtual private cloud \(VPC\) configuration allows outbound traffic\. However, if the VPC configuration the instance was launched in does not allow outbound traffic and you can't change this configuration to allow connectivity to the public service endpoints, you must configure interface endpoints for your VPC instead\.

For more information, see [\(Optional\) Create a Virtual Private Cloud endpoint](setup-create-vpc.md)\.

## Solution 4: Verify target operating system support<a name="instances-missing-solution-4"></a>

Verify that the operation you have chosen can be run on the type of managed node you expect to see listed\. Some Systems Manager operations can target only Windows instances or only Linux instances\. For example, the Systems Manager \(SSM\) documents `AWS-InstallPowerShellModule` and `AWS-ConfigureCloudWatch` can be run only on Windows instances\. In the **Run a command** page, if you choose either of these documents and select **Choose instances manually**, only your Windows instances are listed and available for selection\.

## Solution 5: Verify you're working in the same AWS Region as the Amazon EC2 instance<a name="instances-missing-solution-5"></a>

Amazon EC2 instances are created and available in specific AWS Regions, such as the US East \(Ohio\) Region \(us\-east\-2\) or Europe \(Ireland\) Region \(eu\-west\-1\)\. Ensure that you're working in the same AWS Region as the Amazon EC2 instance that you want to work with\. For more information, see [Choosing a Region](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/getting-started.html#select-region) in *Getting Started with the AWS Management Console*\.

## Solution 6: Verify the proxy configuration you applied to the SSM Agent on your managed node<a name="instances-missing-solution-6"></a>

Verify that the proxy configuration you applied to the SSM Agent on your managed node is correct\. If the proxy configuration is incorrect, the node can't connect to the required service endpoints, or Systems Manager might identify the operating system of the managed node incorrectly\. For more information, see [Configure SSM Agent to use a proxy \(Linux\)](sysman-proxy-with-ssm-agent.md) and [Configure SSM Agent to use a proxy for Windows Server instances](sysman-install-ssm-proxy.md)\.

## Solution 7: Install a TLS certificate on managed instances<a name="hybrid-tls-certificate"></a>

A Transport Layer Security \(TLS\) certificate must be installed on each managed instance you use with AWS Systems Manager\. AWS services use these certificates to encrypt calls to other AWS services\.

A TLS certificate is already installed by default on each Amazon EC2 instance created from any Amazon Machine Image \(AMI\)\. Most modern operating systems include the required TLS certificate from Amazon Trust Services CAs in their trust store\.

To verify whether the required certificate is installed on your instance run the following command based on the operating system of your instance\. Be sure to replace the *region* portion of the URL with the AWS Region where your managed instance is located\.

------
#### [ Linux & macOS ]

```
curl -L https://ssm.region.amazonaws.com
```

------
#### [ Windows ]

```
Invoke-WebRequest -Uri https://ssm.region.amazonaws.com
```

------

The command should return an `UnknownOperationException` error\. If you receive an SSL/TLS error message instead then the required certificate might not be installed\.

If you find the required Amazon Trust Services CA certificates aren't installed on your base operating systems, on instances created from AMIs that aren't supplied by Amazon, or on your own on\-premises servers and VMs, you must install and allow a certificate from [Amazon Trust Services](https://www.amazontrust.com/repository/), or use AWS Certificate Manager \(ACM\) to create and manage certificates for a supported integrated service\.

Each of your managed instances must have one of the following Transport Layer Security \(TLS\) certificates installed\.
+ Amazon Root CA 1
+ Starfield Services Root Certificate Authority \- G2
+ Starfield Class 2 Certificate Authority

For information about using ACM, see the *[AWS Certificate Manager User Guide](https://docs.aws.amazon.com/acm/latest/userguide/)*\.

If certificates in your computing environment are managed by a Group Policy Object \(GPO\), then you might need to configure Group Policy to include one of these certificates\.

For more information about the Amazon Root and Starfield certificates, see the blog post [How to Prepare for AWS’s Move to Its Own Certificate Authority](http://aws.amazon.com/blogs/security/how-to-prepare-for-aws-move-to-its-own-certificate-authority/)\.