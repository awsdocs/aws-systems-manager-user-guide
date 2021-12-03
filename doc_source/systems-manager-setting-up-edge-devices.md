# Setting up AWS Systems Manager for edge devices<a name="systems-manager-setting-up-edge-devices"></a>

This section describes the setup tasks that account and system administrators perform to enable configuration and management of AWS IoT Greengrass core devices\. After you complete these tasks, users who have been granted permissions by the AWS account administrator can use AWS Systems Manager to configure and manage their organization's AWS IoT Greengrass core devices\. 

**Note**  
SSM Agent for AWS IoT Greengrass isn't supported on macOS and Windows 10\. You can't use Systems Manager capabilities to manage and configure edge devices that use these operating systems\.
Systems Manager also supports edge devices that aren't configured as AWS IoT Greengrass core devices\. To use Systems Manager to manage AWS IoT Core devices and non\-AWS edge devices, you must configure them as on\-premises machines in a hybrid environment\. For more information, see [Setting up AWS Systems Manager for hybrid environments](systems-manager-managedinstances.md)\.
To use Session Manager and Microsoft application patching with your edge devices, you must enable the advanced\-instances tier\. For more information, see [Turning on the advanced\-instances tier](systems-manager-managedinstances-advanced.md)\.

**Before you begin**  
Verify that your edge devices meet the following requirements\.
+ Your edge devices must meet the requirements to be configured as AWS IoT Greengrass core devices\. For more information, see [Setting up AWS IoT Greengrass core devices](https://docs.aws.amazon.com/greengrass/v2/developerguide/setting-up.html) in the *AWS IoT Greengrass Version 2 Developer Guide*\.
+ Your edge devices must be compatible with AWS Systems Manager Agent \(SSM Agent\)\. For more information, see [Supported operating systems](prereqs-operating-systems.md)\.
+ Your edge devices must be able to communicate with the Systems Manager service in the cloud\. Systems Manager doesn't support disconnected edge devices\.

**About setting up edge devices**  
Setting up AWS IoT Greengrass devices for Systems Manager involves the following processes\.


****  

| Step | Details | 
| --- | --- | 
|  [Step 1: Complete general Systems Manager setup steps](systems-manager-edge-devices-setup-general.md)  |  Complete all of the general requirements for setting up and configuring Systems Manager\. If you completed these steps already, see Step 2\.  | 
|  [Step 2: Create an IAM service role for edge devices](systems-manager-setting-up-edge-devices-service-role.md)  |  Create an AWS Identity and Access Management \(IAM\) service role that enables your AWS IoT Greengrass devices to communicate with Systems Manager\. If you previously configured on\-premises servers and virtual machines in a hybrid environment for Systems Manager then you might have completed this step already\.  | 
|  [Step 3: Set up AWS IoT Greengrass](systems-manager-edge-devices-set-up-greengrass.md)  |  You must set up your edge devices as AWS IoT Greengrass core devices\. The setup process involves verifying supported operating systems and system requirements, as well as installing and configuring the AWS IoT Greengrass Core software on your devices\. For more information, see [Setting up AWS IoT Greengrass core devices](https://docs.aws.amazon.com/greengrass/v2/developerguide/setting-up.html) in the *AWS IoT Greengrass Version 2 Developer Guide*\.  | 
|  [Step 4: Update the AWS IoT Greengrass token exchange role and install SSM Agent on your edge devices](systems-manager-edge-devices-install-SSM-agent.md)  |  The final step for setting up and configuring your AWS IoT Greengrass core devices for Systems Manager requires you to update the AWS IoT Greengrass IAM service role, also called the *token exchange role*, and deploy AWS Systems Manager Agent \(SSM Agent\) to your AWS IoT Greengrass devices\. Both processes are described in detail in the *AWS IoT Greengrass Version 2 Developer Guide*\. For more information, see [Install AWS Systems Manager SSM Agent](https://docs.aws.amazon.com/greengrass/v2/developerguide/install-systems-manager-agent.html)\.AWS Systems Manager Agent \(SSM Agent\) makes it possible for Systems Manager to update, manage, and configure your edge devices\. To deploy SSM Agent to your AWS IoT Greengrass devices, use Greengrass to deploy the `aws.greengrass.SystemsManagerAgent` component to your devices\. After you deploy SSM Agent to your devices, AWS IoT Greengrass automatically registers your devices with Systems Manager\. No additional registration is necessary\. You can begin using Systems Manager capabilities to access, manage, and configure your AWS IoT Greengrass devices\.  | 

**Topics**
+ [Step 1: Complete general Systems Manager setup steps](systems-manager-edge-devices-setup-general.md)
+ [Step 2: Create an IAM service role for edge devices](systems-manager-setting-up-edge-devices-service-role.md)
+ [Step 3: Set up AWS IoT Greengrass](systems-manager-edge-devices-set-up-greengrass.md)
+ [Step 4: Update the AWS IoT Greengrass token exchange role and install SSM Agent on your edge devices](systems-manager-edge-devices-install-SSM-agent.md)