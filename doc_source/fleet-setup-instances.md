# Step 2: Verify your instances are managed by Systems Manager<a name="fleet-setup-instances"></a>

For instances to be monitored and managed using Fleet Manager, a capability of AWS Systems Manager, they must be Systems Manager managed instances\. For information about configuring managed instances, see [AWS Systems Manager Managed Instances](managed_instances.md)\.

You can use Quick Setup, a capability of AWS Systems Manager, to help you quickly configure your Amazon Elastic Compute Cloud \(Amazon EC2\) instances as managed instances in an individual account, or across multiple organizational units \(OUs\) and AWS Regions by integrating with AWS Organizations\. For more information about using Quick Setup to configure managed instances, see [Quick Setup Host Management](quick-setup-host-management.md)\.

**Note**  
For servers or virtual machines that aren't running on AWS, use a hybrid activation to configure the server or VM as a managed instance\. For information about hybrid activations, see [AWS Systems Manager Hybrid Activations](activations.md)\.