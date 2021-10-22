# Troubleshooting Amazon EC2 managed instance availability<a name="troubleshooting-managed-instances"></a>

For several AWS Systems Manager operations, you can choose to manually select the instances on which you want to run the operation\. Examples include running an Run Command command, specifying maintenance window targets, installing Distributor packages, and connecting to the instance using Session Manager, a capability of AWS Systems Manager\. In cases like these, after you specify that you want to choose instances manually, a list is displayed of managed instances you can choose to run the operation on\.

This topic provides information to help you diagnose why an Amazon Elastic Compute Cloud \(Amazon EC2\) instance *that you have confirmed is running* isn't included in your lists of managed instances in Systems Manager\. 

In order for an EC2 instance to be managed by Systems Manager and available in lists of managed instances, it must meet three primary requirements:
+ SSM Agent must be installed and running on an instance with a supported operating system\.
**Note**  
Some Amazon Machine Images \(AMIs\) are configured to launch instances with [SSM Agent](ssm-agent.md) preinstalled\. \(You can also configure a custom AMI to preinstall SSM Agent\.\)   
SSM Agent is preinstalled, by default, on the following Amazon Machine Images \(AMIs\):  
Amazon Linux
Amazon Linux 2
Amazon Linux 2 ECS\-Optimized Base AMIs
macOS 10\.14\.x \(Mojave\), 10\.15\.x \(Catalina\), and 11\.x \(Big Sur\)
SUSE Linux Enterprise Server \(SLES\) 12 and 15
Ubuntu Server 16\.04, 18\.04, and 20\.04  
Windows Server 2008\-2012 R2 AMIs published in November 2016 or later
Windows Server 2016 and 2019
SSM Agent isn't installed on all AMIs based on Amazon Linux or Amazon Linux 2\.
+ An AWS Identity and Access Management \(IAM\) instance profile that supplies the required permissions for the instance to communicate with the Systems Manager service must be attached to the instance\.
+ SSM Agent must be able to connect to a Systems Manager endpoint in order to register itself with the service\. Thereafter, the instance must be available to the service, which is confirmed by the service sending a signal every five minutes to check the instance's health\. 

After you verify that an EC2 instance is running, you can use the following command to check whether SSM Agent on one of these instance types has successfully registered itself with the Systems Manager service\. This command doesn't return results until a successful registration has taken place\.

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

If registration was successful and the instance is now available for Systems Manager operations, the command returns results similar to the following\.

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

If the command doesn't return results after 5 minutes or so, use the following information to help you troubleshoot problems with your managed instances\.

**Topics**
+ [Solution 1: Verify that SSM Agent is installed and running on the instance](#instances-missing-solution-1)
+ [Solution 2: Verify that an IAM instance role has been specified for the instance](#instances-missing-solution-2)
+ [Solution 3: Verify service endpoint connectivity](#instances-missing-solution-3)
+ [Solution 4: Verify target operating system support](#instances-missing-solution-4)
+ [Solution 5: Verify you're working in the same AWS Region as the EC2 instance](#instances-missing-solution-5)
+ [Solution 6: Verify the proxy configuration you applied to the SSM Agent on your instance](#instances-missing-solution-6)

## Solution 1: Verify that SSM Agent is installed and running on the instance<a name="instances-missing-solution-1"></a>

Make sure the latest version of SSM Agent is installed and running on the instance\.

To determine whether SSM Agent is installed and running on an EC2 instance, see [Checking SSM Agent status and starting the agent](ssm-agent-status-and-restart.md)\.

To install or reinstall SSM Agent on an EC2 instance, see the following topics:
+ [Installing and configuring SSM Agent on EC2 instances for Linux](sysman-install-ssm-agent.md)
+ [Installing and configuring SSM Agent on EC2 instances for Windows Server](sysman-install-ssm-win.md)

## Solution 2: Verify that an IAM instance role has been specified for the instance<a name="instances-missing-solution-2"></a>

Verify that the instance is configured with an AWS Identity and Access Management \(IAM\) role that allows the instance to communicate with the Systems Manager API\. Also verify that your user account has an IAM user trust policy that allows your account to communicate with the Systems Manager API\.

**To determine whether an instance profile with the necessary permissions is attached to the instance**

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

**To attach an existing instance profile with the necessary permissions to an existing EC2 instance**
+ Complete the task in the following topic:
  + [Attach the Systems Manager instance profile to an existing instance \(console\)](setup-launch-managed-instance.md#setup-launch-managed-instance-existing)

## Solution 3: Verify service endpoint connectivity<a name="instances-missing-solution-3"></a>

Verify that the instance has connectivity to the Systems Manager service endpoints\. This connectivity is provided by creating and configuring VPC endpoints for Systems Manager, or by allowing HTTPS \(port 443\) outbound traffic to the service endpoints\. 

Often, when you create an EC2 instance, the Systems Manager service endpoint for the AWS Region the instance is used to register the instance\. However, if you are using a Virtual Private Cloud \(VPC\) and the EC2 instance has been created in a private subnet, service endpoints aren't provided automatically\. Configure interface endpoints for your VPC instead\.

For more information, see [\(Optional\) Create a Virtual Private Cloud endpoint](setup-create-vpc.md)\.

## Solution 4: Verify target operating system support<a name="instances-missing-solution-4"></a>

Verify that the operation you have chosen can be run on the type of instance you expect to see listed\. Some Systems Manager operations can target only Windows instances or only Linux instances\. For example, the Systems Manager \(SSM\) documents `AWS-InstallPowerShellModule` and `AWS-ConfigureCloudWatch` can be run only on Windows instances\. In the **Run a command** page, if you choose either of these documents and select **Choose instances manually**, only your Windows instances are listed and available for selection\.

## Solution 5: Verify you're working in the same AWS Region as the EC2 instance<a name="instances-missing-solution-5"></a>

EC2 instances are created and available in specific AWS Regions, such as the US East \(Ohio\) Region \(us\-east\-2\) or Europe \(Ireland\) Region \(eu\-west\-1\)\. Ensure that you're working in the same AWS Region as the EC2 instance that you want to work with\. For more information, see [Choosing a Region](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/getting-started.html#select-region) in *Getting Started with the AWS Management Console*\.

## Solution 6: Verify the proxy configuration you applied to the SSM Agent on your instance<a name="instances-missing-solution-6"></a>

Verify that the proxy configuration you applied to the SSM Agent on your EC2 instance is correct\. If the proxy configuration is incorrect, the instance can't connect to the required service endpoints, or Systems Manager might identify the operating system of the instance incorrectly\. For more information, see [Configure SSM Agent to use a proxy \(Linux\)](sysman-proxy-with-ssm-agent.md) and [Configure SSM Agent to use a proxy for Windows Server instances](sysman-install-ssm-proxy.md)\.