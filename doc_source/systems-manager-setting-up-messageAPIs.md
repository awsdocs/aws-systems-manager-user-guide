# Reference: Ec2messages and Other API Calls<a name="systems-manager-setting-up-messageAPIs"></a>

If you monitor API calls, you will see calls to the following APIs\.
+ ec2messages:AcknowledgeMessage
+ ec2messages:DeleteMessage
+ ec2messages:FailMessage
+ ec2messages:GetEndpoint
+ ec2messages:GetMessages
+ ec2messages:SendReply
+ UpdateInstanceInformation
+ ListInstanceAssociations
+ DescribeInstanceProperties
+ DescribeDocumentParameters

Calls to `ec2messages:*` APIs are calls to the ec2messages endpoint\. Systems Manager uses this endpoint to make calls from SSM Agent to the Systems Manager service in the cloud\. This endpoint is required to send and receive commands\.

`UpdateInstanceInformation`: SSM Agent calls the Systems Manager service in the cloud every five minutes to provide heartbeat information\. This call is necessary to maintain a heartbeat with the agent so that the service knows the agent is functioning as expected\. 

`ListInstanceAssociations`: The agent calls this API to see if a new Systems Manager State Manager association is available\. This API is required for State Manager to function\.

`DescribeInstanceProperties` and `DescribeDocumentParameters`: Systems Manager calls these APIs to render specific nodes in the Amazon EC2 console\. The `DescribeInstanceProperties` API displays the **Managed Instances** node in the left navigation\. The `DescribeDocumentParameters` API displays the **Documents** node in the left navigation\.