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
+ `ssm:UpdateInstanceInformation`

These are special operations used AWS Systems Manager\.

**ec2messages API operations**  
`ec2messages:*` API operations are made to the Amazon Message Delivery Service endpoint\. Systems Manager uses this endpoint for API operations from Systems Manager Agent \(SSM Agent\) to the Systems Manager service in the cloud\. This endpoint is required to send and receive commands\. For more information, see [Actions, resources, and condition keys for Amazon Message Delivery Service](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonmessagedeliveryservice.html)\.

**ssmmessages API operations**  
Systems Manager uses the `ssmmessages` endpoint for API operations from SSM Agent to Session Manager, a capability of AWS Systems Manager, in the cloud\. This endpoint is required to create and delete session channels with the Session Manager service in the cloud\. For more information, see [Actions, resources, and condition keys for Amazon Session Manager Message Gateway Service](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonsessionmanagermessagegatewayservice.html)\.

**Instance\-related API operations**  
`UpdateInstanceInformation`: SSM Agent calls the Systems Manager service in the cloud every 5 minutes to provide heartbeat information\. This call is necessary to maintain a heartbeat with the agent so that the service knows the agent is functioning as expected\. 

`ListInstanceAssociations`: The agent runs this API operation to see if a new State Manager association is available\. This API operation is required for State Manager, a capability of AWS Systems Manager, to function\.

`DescribeInstanceProperties` and `DescribeDocumentParameters`: Systems Manager runs these API operations to render specific nodes in the Amazon EC2 console\. Results of the `DescribeInstanceProperties` operation are displayed in the Fleet Manager node\. Results of the `DescribeDocumentParameters` operation are displayed in the **Documents** node\.

`ssm:RegisterManagedInstance`: SSM Agent runs this API operation to register an on\-premises server or virtual machine \(VM\) with Systems Manager as a managed instance using an activation code and ID, or to register AWS IoT Greengrass Version 2 credentials\. 