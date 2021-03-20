# Calling public parameters<a name="parameter-store-public-parameters"></a>

Some AWS services publish information about common artifacts as AWS Systems Manager *public* parameters\. For example, the Amazon Elastic Compute Cloud \(Amazon EC2\) service publishes information about Amazon Machine Images \(AMIs\) as public parameters\.

You can call this information from your scripts and code by using the [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html), [GetParameter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameter.html), and [GetParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html) API actions\.

**Related blog posts**
+ [Query for AWS Regions, Endpoints, and More Using AWS Systems Manager Parameter Store](http://aws.amazon.com/blogs/aws/new-query-for-aws-regions-endpoints-and-more-using-aws-systems-manager-parameter-store/)
+ [Query for the latest Amazon Linux AMI IDs using AWS Systems Manager Parameter Store](http://aws.amazon.com/blogs/compute/query-for-the-latest-amazon-linux-ami-ids-using-aws-systems-manager-parameter-store/)
+ [Query for the Latest Windows AMI Using AWS Systems Manager Parameter Store](http://aws.amazon.com/blogs/mt/query-for-the-latest-windows-ami-using-systems-manager-parameter-store/)

**Topics**
+ [Calling AMI public parameters](parameter-store-public-parameters-ami.md)
+ [Calling the ECS optimized AMI public parameter](parameter-store-public-parameters-ecs.md)
+ [Calling the EKS optimized AMI public parameter](parameter-store-public-parameters-eks.md)
+ [Calling public parameters for AWS services, Regions, endpoints, Availability Zones, local zones, and Wavelength Zones](parameter-store-public-parameters-global-infrastructure.md)