# SSM Agent technical reference<a name="ssm-agent-technical-details"></a>

Use the information in this topic to help you implement AWS Systems Manager Agent \(SSM Agent\) and understand how the agent works\.

**Topics**
+ [SSM Agent credentials precedence](#credentials-precedence)
+ [About the local ssm\-user account](#ssm-user-account)
+ [SSM Agent and the Instance Metadata Service \(IMDS\)](#imds)
+ [Keeping SSM Agent up\-to\-date](#updating)
+ [SSM Agent rolling updates by AWS Regions](#rolling-updates)
+ [Installing SSM Agent on VMs and on\-premises instances](#agent-hybrid-installations)
+ [Validating on\-premises servers, edge devices, and virtual machines using a hardware fingerprint](#fingerprint-validation)
+ [AMIs with SSM Agent preinstalled](#ami-preinstalled-agent)
+ [SSM Agent on GitHub](#github)

## SSM Agent credentials precedence<a name="credentials-precedence"></a>

This topic describes important information about how SSM Agent is granted permission to perform actions on your resources\. The content is primarily focused on SSM Agent running Amazon Elastic Compute Cloud \(Amazon EC2\) instances and servers or VMs in your hybrid environment\. For edge devices, you must configure your devices to use AWS IoT Greengrass Core software, configure an AWS Identity and Access Management \(IAM\) service role, and deploy SSM Agent to your devices by using AWS IoT Greengrass\. For more information, see [Setting up AWS Systems Manager for edge devices](systems-manager-setting-up-edge-devices.md)\.

When SSM Agent is installed on an instance, it requires permissions in order to communicate with the Systems Manager service\. On Amazon Elastic Compute Cloud \(Amazon EC2\) instances, these permissions are provided in an instance profile that is attached to the instance\. On a hybrid instance, SSM Agent normally gets the needed permissions from the shared credentials file, located at `/root/.aws/credentials` \(Linux and macOS\) or `%USERPROFILE%\.aws\credentials` \(Windows Server\)\. The needed permissions are added to this file during the hybrid activation process\.

In rare cases, however, an instance might end up with permissions added to more than one of the locations where SSM Agent checks for permissions to run its tasks\. 

As one example, you might configure an instance to be managed by Systems Manager\. For an EC2 instance, that configuration includes attaching an instance profile\. For an on\-premises server or virtual machine \(VM\), that means credentials are provided through the hybrid activation process\. But, then you decide to also use that instance for developer or end\-user tasks and install the AWS Command Line Interface \(AWS CLI\) on it\. This installation results in additional permissions being added to a credentials file on the instance\.

When you run a Systems Manager command on the instance, SSM Agent might try to use credentials different from the ones you expect it to use, such as from a credentials file instead of an instance profile\. This is because SSM Agent looks for credentials in the order prescribed for the *default credential provider chain*\.

**Note**  
On Linux and macOS, SSM Agent runs as the root user\. Therefore, the environment variables and credentials file that SSM Agent looks for in this process are those of the root user only \(`/root/.aws/credentials`\)\. SSM Agent doesn't look at the environment variables or credentials file of any other user accounts on the instance during the search for credentials\.

The default provider chain looks for credentials in the following order:

1. Environment variables, if configured \(`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`\)\.

1. Shared credentials file \(`$HOME/.aws/credentials` for Linux and macOS or `%USERPROFILE%\.aws\credentials` for Windows Server\) with permissions provided by, for example, a hybrid activation or an AWS CLI installation\.

1. An AWS Identity and Access Management \(IAM\) role for tasks if an application is present that uses an Amazon Elastic Container Service \(Amazon ECS\) task definition or RunTask API operation\.

1. An instance profile attached to an Amazon EC2 instance\.

For related information, see the following topics:
+ Instance profiles for EC2 instances – [Create an IAM instance profile for Systems Manager](setup-instance-profile.md) and [Attach an IAM instance profile to an EC2 instance](setup-launch-managed-instance.md) 
+ Hybrid activations – [Create a managed\-instance activation for a hybrid environment](sysman-managed-instance-activation.md)
+ AWS CLI credentials – [Configuration and credential file settings](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) in the *AWS Command Line Interface User Guide*
+ Default credential provider chain – [Specifying Credentials](https://docs.aws.amazon.com/sdk-for-go/latest/developer-guide/configuring-sdk.html#specifying-credentials) in the *AWS SDK for Go Developer Guide*
**Note**  
This topic in the *AWS SDK for Go Developer Guide* describes the default provider chain in terms of the SDK for Go; however, the same principles apply to evaluating credentials for SSM Agent\.

## About the local ssm\-user account<a name="ssm-user-account"></a>

Starting with version 2\.3\.50\.0 of SSM Agent, the agent creates a local user account called `ssm-user` and adds it to the `/etc/sudoers.d` directory \(Linux and macOS\) or to the Administrators group \(Windows Server\)\. On agent versions before 2\.3\.612\.0, the account is created the first time SSM Agent starts or restarts after installation\. On version 2\.3\.612\.0 and later, the `ssm-user` account is created the first time a session is started on an instance\. This `ssm-user` is the default OS user when a session starts in Session Manager, a capability of AWS Systems Manager\. You can change the permissions by moving `ssm-user` to a less\-privileged group or by changing the `sudoers` file\. The `ssm-user` account isn't removed from the system when SSM Agent is uninstalled\.

On Windows Server, SSM Agent handles setting a new password for the `ssm-user` account when each session starts\. No passwords are set for `ssm-user` on Linux managed instances\.

Starting with SSM Agent version 2\.3\.612\.0, the `ssm-user` account isn't created automatically on Windows Server machines that are being used as domain controllers\. To use Session Manager on a Windows Server domain controller, create the `ssm-user` account manually if it isn't already present, and assign Domain Administrator permissions to the user\.

**Important**  
In order for the ssm\-user account to be created, the instance profile attached to the instance must provide the necessary permissions\. For information, see [Verify or create an IAM role with Session Manager permissions](session-manager-getting-started-instance-profile.md)\.

## SSM Agent and the Instance Metadata Service \(IMDS\)<a name="imds"></a>

Systems Manager relies on EC2 instance metadata to function correctly\. Systems Manager can access instance metadata using either version 1 or version 2 of the Instance Metadata Service \(IMDSv1 and IMDSv2\)\. Your instance must be able to access IPv4 address of the instance metadata service: 169\.254\.169\.254\. For more information, see [Instance metadata and user data](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html) in the *Amazon EC2 User Guide for Linux Instances*\.

## Keeping SSM Agent up\-to\-date<a name="updating"></a>

An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on a managed node, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your machines\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. Subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub to get notifications about SSM Agent updates\.

**Note**  
An updated version of SSM Agent is released whenever new capabilities are added to Systems Manager or updates are made to existing capabilities\. If an older version of the agent is running on a managed node, some SSM Agent processes can fail\. For that reason, we recommend that you automate the process of keeping SSM Agent up\-to\-date on your machines\. For information, see [Automating updates to SSM Agent](ssm-agent-automatic-updates.md)\. Subscribe to the [SSM Agent Release Notes](https://github.com/aws/amazon-ssm-agent/blob/mainline/RELEASENOTES.md) page on GitHub to get notifications about SSM Agent updates\.  
Amazon Machine Images \(AMIs\) that include SSM Agent by default can take up to two weeks to be updated with the newest version of SSM Agent\. We recommend that you configure even more frequent automated updates to SSM Agent\.

## SSM Agent rolling updates by AWS Regions<a name="rolling-updates"></a>

After an SSM Agent update is made available in its GitHub repository, it can take up to two weeks until the updated version is rolled out to all AWS Regions at different times\. For this reason, you might receive the "Unsupported on current platform" or "updating amazon\-ssm\-agent to an older version, please turn on allow downgrade to proceed" error when trying to deploy a new version of SSM Agent in a Region\.

To determine the version of SSM Agent available to you, you can run a `curl` command\.

To view the version of the agent available in the global download bucket, run the following command\.

```
curl https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/VERSION
```

To view the version of the agent available in a specific Region, run the following command, substituting *region* with the Region you're working in, such as `us-east-2` for the US East \(Ohio\) Region\.

```
curl https://s3.region.amazonaws.com/amazon-ssm-region/latest/VERSION
```

You can also open the `VERSION` file directly in your browser without a `curl` command\.

## Installing SSM Agent on VMs and on\-premises instances<a name="agent-hybrid-installations"></a>

For information about installing SSM Agent on on\-premises servers, edge devices, and virtual machines \(VMs\) in a hybrid environment, see [Install SSM Agent for a hybrid environment \(Linux\)](sysman-install-managed-linux.md) and [Install SSM Agent for a hybrid environment \(Windows\)](sysman-install-managed-win.md)\.

## Validating on\-premises servers, edge devices, and virtual machines using a hardware fingerprint<a name="fingerprint-validation"></a>

When running on\-premises servers, edge devices, and virtual machines \(VMs\) in a hybrid environment, SSM Agent gathers a number of system attributes \(referred to as the *hardware hash*\) and uses these attributes to compute a *fingerprint*\. The fingerprint is an opaque string that the agent passes to certain Systems Manager APIs\. This unique fingerprint associates the caller with a particular on\-premises managed node\. The agent stores the fingerprint and hardware hash on the local disk in a location called the *Vault*\.

The agent computes the hardware hash and fingerprint when the on\-premises server, edge device, or VM is registered for use with Systems Manager\. Then, the fingerprint is passed back to the Systems Manager service when the agent sends a `RegisterManagedInstance` command\. 

Later, when sending a `RequestManagedInstanceRoleToken` command, the agent checks the fingerprint and hardware hash in the Vault to make sure that the current machine attributes match with the stored hardware hash\. If the current machine attributes do match the hardware hash stored in the Vault, the agent passes in the fingerprint from the Vault to `RegisterManagedInstance`, resulting in a successful call\. 

If the current machine attributes don't match the stored hardware hash, SSM Agent computes a new fingerprint, stores the new hardware hash and fingerprint in the Vault, and passes the new fingerprint to `RequestManagedInstanceRoleToken`\.* This causes `RequestManagedInstanceRoleToken` to fail, and the agent won't be able to obtain a role token to connect to the Systems Manager service\.*

This failure is by design and is used as a verification step to prevent multiple on\-premises managed nodes from communicating with the Systems Manager service as the same managed node\.

When comparing the current machine attributes to the hardware hash stored in the Vault, the agent uses the following logic to determine whether the old and new hashes match:
+ If the SID \(system/machine ID\) is different, then no match\.
+ Otherwise, if the IP address is the same, then match\.
+ Otherwise, the percentage of machine attributes that match is computed and compared with the user\-configured similarity threshold to determine whether there is a match\. 

The similarity threshold is stored in the Vault, as part of the hardware hash\. 

The similarity threshold can be set after an instance is registered using a command like the following\.

On Linux instances:

```
sudo amazon-ssm-agent -fingerprint -similarityThreshold 1
```

On Windows Server instances using PowerShell:

```
cd "C:\Program Files\Amazon\SSM\" `
    .\amazon-ssm-agent.exe -fingerprint -similarityThreshold 1
```

**Important**  
If one of the components used to calculate the fingerprint changes, this can cause the agent to hibernate\. To help avoid this hibernation, set the similarity threshold to a low value, such as **1**\.

## AMIs with SSM Agent preinstalled<a name="ami-preinstalled-agent"></a>

In most cases, SSM Agent is preinstalled, by default, on the following Amazon Machine Images \(AMIs\):
+ Amazon Linux
+ Amazon Linux 2
+ Amazon Linux 2 ECS\-Optimized Base AMIs
+ macOS 10\.14\.x \(Mojave\), 10\.15\.x \(Catalina\), and 11\.x \(Big Sur\)
+ SUSE Linux Enterprise Server \(SLES\) 12 and 15
+ Ubuntu Server 16\.04, 18\.04, and 20\.04  
+ Windows Server 2008\-2012 R2 AMIs published in November 2016 or later
+ Windows Server 2016 and 2019

For information about verifying whether the agent is installed on an EC2 instance, see [Checking SSM Agent status and starting the agent](ssm-agent-status-and-restart.md)\.

You must manually install SSM Agent on EC2 instances created from other Linux AMIs\. You must also manually install SSM Agent on AWS IoT Greengrass core devices and on\-premises servers, edge devices, and VMs in your hybrid environment\. For more information, see [Setting up AWS Systems Manager for hybrid environments](systems-manager-managedinstances.md)\.

**Note**  
SSM Agent might be pre\-installed on Community AMIs that support other operating systems\. AWS doesn't support these Community AMIs\.

## SSM Agent on GitHub<a name="github"></a>

The source code for SSM Agent is available on [GitHub](https://github.com/aws/amazon-ssm-agent) so that you can adapt the agent to meet your needs\. We encourage you to submit [pull requests](https://github.com/aws/amazon-ssm-agent/blob/mainline/CONTRIBUTING.md) for changes that you would like to have included\. However, Amazon Web Services doesn't provide support for running modified copies of this software\.