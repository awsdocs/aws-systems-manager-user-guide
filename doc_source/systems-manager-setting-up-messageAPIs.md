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
+ `ssm:UpdateInstanceInformation`
+ `ssm:ListInstanceAssociations`
+ `ssm:DescribeInstanceProperties`
+ `ssm:DescribeDocumentParameters`

These are special operations used AWS Systems Manager\.

<<<<<<< HEAD
**ec2messages API operations**  
`ec2messages:*` API operations are made to the Amazon Message Delivery Service endpoint\. Systems Manager uses this endpoint for API operations from Systems Manager Agent \(SSM Agent\) to the Systems Manager service in the cloud\. This endpoint is required to send and receive commands\. For more information, see [Actions, resources, and condition keys for Amazon Message Delivery Service](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonmessagedeliveryservice.html)\.

**ssmmessages API operations**  
Systems Manager uses the `ssmmessages` endpoint for API operations from SSM Agent to Session Manager, a capability of AWS Systems Manager, in the cloud\. This endpoint is required to create and delete session channels with the Session Manager service in the cloud\. For more information, see [Actions, resources, and condition keys for Amazon Session Manager Message Gateway Service](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonsessionmanagermessagegatewayservice.html)\.
=======
**ec2messages API calls**  
Calls to `ec2messages:*` APIs are calls to the Amazon Message Delivery Service endpoint\. Systems Manager uses this endpoint to make calls from Systems Manager Agent \(SSM Agent\) to the Systems Manager service in the cloud\. This endpoint is required to send and receive commands\. For more information, see [Actions, resources, and condition keys for Amazon Message Delivery Service](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonmessagedeliveryservice.html)\.

**ssmmessages API calls**  
Systems Manager uses the `ssmmessages` endpoint to make calls from SSM Agent to Session Manager, a capability of AWS Systems Manager, in the cloud\. This endpoint is required to create and delete session channels with the Session Manager service in the cloud\. For more information, see [Actions, resources, and condition keys for Amazon Session Manager Message Gateway Service](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonsessionmanagermessagegatewayservice.html)\.
>>>>>>> dab311de0fb148e423873c890671c5d5d75b2cc3

**Instance\-related API operations**  
`UpdateInstanceInformation`: SSM Agent calls the Systems Manager service in the cloud every 5 minutes to provide heartbeat information\. This call is necessary to maintain a heartbeat with the agent so that the service knows the agent is functioning as expected\. 

`ListInstanceAssociations`: The agent runs this API operations to see if a new State Manager association is available\. This API operation is required for State Manager, a capability of AWS Systems Manager, to function\.

`DescribeInstanceProperties` and `DescribeDocumentParameters`: Systems Manager runs these API operations to render specific nodes in the Amazon EC2 console\. Results of the `DescribeInstanceProperties` operation are displayed in the **Fleet Manager** node\. Results of the `DescribeDocumentParameters` operation are displayed in the **Documents** node\.