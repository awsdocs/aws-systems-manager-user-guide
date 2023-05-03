# Reference: ec2messages, ssmmessages, and other API operations<a name="systems-manager-setting-up-messageAPIs"></a>

If you monitor API operations, you might see calls to the following:
+ `ec2messages:AcknowledgeMessage`
+ `ec2messages:DeleteMessage`
+ `ec2messages:FailMessage`
+ `ec2messages:GetEndpoint`
+ `ec2messages:GetMessages`
+ `ec2messages:SendReply`
+ `ssmmessages:CreateControlChannel`
+ `ssmmessages:CreateDataChannel`
+ `ssmmessages:OpenControlChannel`
+ `ssmmessages:OpenDataChannel`
+ `ssm:DescribeInstanceProperties`
+ `ssm:DescribeDocumentParameters`
+ `ssm:ListInstanceAssociations`
+ `ssm:RegisterManagedInstance`
+ `ssm:UpdateInstanceAssociationStatus`
+ `ssm:UpdateInstanceInformation`
+ `ssm:GetManifest`
+ `ssm:PutConfigurePackageResult`

These are special operations used AWS Systems Manager\.

**ec2messages API operations**  
`ec2messages:*` API operations are made to the Amazon Message Delivery Service endpoint\. Systems Manager uses this endpoint for API operations from Systems Manager Agent \(SSM Agent\) to the Systems Manager service in the cloud\. This endpoint is required to send and receive commands\. For more information, see [Actions, resources, and condition keys for Amazon Message Delivery Service](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonmessagedeliveryservice.html)\.

**ssmmessages API operations**  
Systems Manager uses the `ssmmessages` endpoint for API operations from SSM Agent to Session Manager, a capability of AWS Systems Manager, in the cloud\. This endpoint is required to create and delete session channels with the Session Manager service in the cloud\. Additionally, if connectivity is allowed, SSM Agent receives `Command` documents through Message Gateway Service\. If connectivity is not allowed, SSM Agent receives `Command` documents through Message Delivery Service\. For more information, see [Actions, resources, and condition keys for Amazon Session Manager Message Gateway Service](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonsessionmanagermessagegatewayservice.html)\.

**Instance\-related API operations**  
`UpdateInstanceInformation`: SSM Agent calls the Systems Manager service in the cloud every 5 minutes to provide heartbeat information\. This call is necessary to maintain a heartbeat with the agent so that the service knows the agent is functioning as expected\. 

`UpdateInstanceAssociationStatus`: The agent runs this API operation to update an association\. This API operation is required for State Manager, a capability of AWS Systems Manager, to function\.

`ListInstanceAssociations`: The agent runs this API operation to see if a new State Manager association is available\. This API operation is required for State Manager to function\.

`DescribeInstanceProperties` and `DescribeDocumentParameters`: Systems Manager runs these API operations to render specific nodes in the Amazon EC2 console\. Results of the `DescribeInstanceProperties` operation are displayed in the Fleet Manager node\. Results of the `DescribeDocumentParameters` operation are displayed in the **Documents** node\.

`RegisterManagedInstance`: SSM Agent runs this API operation to register an on\-premises server or virtual machine \(VM\) with Systems Manager as a managed instance using an activation code and ID, or to register AWS IoT Greengrass Version 2 credentials\. This operation is also called by Amazon EC2 instances running SSM Agent version 3\.1\.x or later\.

**Distributor\-related API operations**  
SSM Agent runs `GetManifest` to determine system requirements for installing or updating a specified version of an [AWS Systems Manager Distributor](distributor.md) package\. This is a legacy API operation and not available in AWS Regions launched after 2017\. 

SSM Agent runs `PutConfigurePackageResult` to publish installation error and latency metrics for public Distributor packages to the package owner’s account\.