# Reference: ec2messages, ssmmessages, and other API calls<a name="systems-manager-setting-up-messageAPIs"></a>

If you monitor API calls, you might see calls to the following APIs\.
+ ec2messages:AcknowledgeMessage
+ ec2messages:DeleteMessage
+ ec2messages:FailMessage
+ ec2messages:GetEndpoint
+ ec2messages:GetMessages
+ ec2messages:SendReply
+ ssmmessages:CreateControlChannel
+ ssmmessages:CreateDataChannel
+ ssmmessages:OpenControlChannel
+ ssmmessages:OpenDataChannel
+ ssm:UpdateInstanceInformation
+ ssm:ListInstanceAssociations
+ ssm:DescribeInstanceProperties
+ ssm:DescribeDocumentParameters

These special calls are used by Systems Manager for various operations\.

**ec2messages API calls**  
Calls to `ec2messages:*` APIs are calls to the Amazon Message Delivery Service endpoint\. Systems Manager uses this endpoint to make calls from SSM Agent to the Systems Manager service in the cloud\. This endpoint is required to send and receive commands\. For more information, see [Actions, Resources, and Condition Keys for Amazon Message Delivery Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonmessagedeliveryservice.html)\.

**ssmmessages API calls**  
Systems Manager uses the `ssmmessages` endpoint to make calls from SSM Agent to the Session Manager service in the cloud\. This endpoint is required to create and delete session channels with the Session Manager service in the cloud\.

**Instance\-related API calls**  
`UpdateInstanceInformation`: SSM Agent calls the Systems Manager service in the cloud every five minutes to provide heartbeat information\. This call is necessary to maintain a heartbeat with the agent so that the service knows the agent is functioning as expected\. 

`ListInstanceAssociations`: The agent calls this API to see if a new Systems Manager State Manager association is available\. This API is required for State Manager to function\.

`DescribeInstanceProperties` and `DescribeDocumentParameters`: Systems Manager calls these APIs to render specific nodes in the Amazon EC2 console\. The `DescribeInstanceProperties` API displays the **Managed Instances** node in the left navigation\. The `DescribeDocumentParameters` API displays the **Documents** node in the left navigation\.