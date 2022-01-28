# Step 2: Verify your instances and edge devices can be managed by Systems Manager<a name="fleet-setup-instances"></a>

For Amazon Elastic Compute Cloud \(Amazon EC2\) instances; AWS IoT Greengrass core devices; and on\-premises servers, edge devices, and virtual machines \(VMs\) to be monitored and managed using Fleet Manager, a capability of AWS Systems Manager, they must be Systems Manager* managed nodes*\. This means your nodes must meet certain prerequisites and be configured with the AWS Systems Manager Agent \(SSM Agent\)\. For more information, see [Setting up AWS Systems Manager](systems-manager-setting-up.md)\. 

You can use Quick Setup, a capability of AWS Systems Manager, to help you quickly configure your Amazon EC2 instances as managed instances in an individual account\. If your business or organization uses AWS Organizations, you can also configure instances across multiple organizational units \(OUs\) and AWS Regions \. For more information about using Quick Setup to configure managed instances, see [Quick Setup Host Management](quick-setup-host-management.md)\.

**Note**  
For servers or virtual machines that aren't running on AWS, use a hybrid activation to configure the server, edge device, or VM as a managed instance\. For information about hybrid activations, see [AWS Systems Manager Hybrid Activations](activations.md)\.