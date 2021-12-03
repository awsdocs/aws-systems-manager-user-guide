# Step 4: Update the AWS IoT Greengrass token exchange role and install SSM Agent on your edge devices<a name="systems-manager-edge-devices-install-SSM-agent"></a>

The final step for setting up and configuring your AWS IoT Greengrass core devices for Systems Manager requires you to update the AWS IoT Greengrass AWS Identity and Access Management \(IAM\) device service role, also called the *token exchange role*, and deploy AWS Systems Manager Agent \(SSM Agent\) to your AWS IoT Greengrass devices\. Both processes are described in detail in the *AWS IoT Greengrass Version 2 Developer Guide*\. For more information, see [Install the AWS Systems Manager Agent](https://docs.aws.amazon.com/greengrass/v2/developerguide/install-systems-manager-agent.html)\.

After you deploy SSM Agent to your devices, AWS IoT Greengrass automatically registers your devices with Systems Manager\. No additional registration is necessary\. You can begin using Systems Manager capabilities to access, manage, and configure your AWS IoT Greengrass devices\.

**Note**  
Your edge devices must be able to communicate with the Systems Manager service in the cloud\. Systems Manager doesn't support disconnected edge devices\.