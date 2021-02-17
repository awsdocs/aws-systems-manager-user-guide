# Best practices for shared SSM documents<a name="ssm-before-you-share"></a>

Review the following guidelines before you share or use a shared document\. 

**Remove sensitive information**  
Review your SSM document carefully and remove any sensitive information\. For example, verify that the document does not include your AWS credentials\. If you share a document with specific individuals, those users can view the information in the document\. If you share a document publicly, anyone can view the information in the document\.

**Restrict Run Command actions using an IAM user trust policy**  
Create a restrictive AWS Identity and Access Management \(IAM\) user policy for users who will have access to the document\. The IAM policy determines which SSM documents a user can see in either the Amazon EC2 console or by calling `ListDocuments` using the AWS CLI or AWS Tools for Windows PowerShell\. The policy also restricts the actions the user can perform with SSM documents\. You can create a restrictive policy so that a user can only use specific documents\. For more information, see [ Create non\-Admin IAM users and groups for Systems Manager](setup-create-iam-user.md) and [Customer managed policy examples](security_iam_id-based-policy-examples.md#customer-managed-policies)\.

**Use caution when using shared SSM documents**  
Review the contents of every document that is shared with you, especially public documents, to understand the commands that will be run on your instances\. A document could intentionally or unintentionally have negative repercussions after it is run\. If the document references an external network, review the external source before you use the document\. 

**Send commands using the document hash**  
When you share a document, the system creates a Sha\-256 hash and assigns it to the document\. The system also saves a snapshot of the document content\. When you send a command using a shared document, you can specify the hash in your command to ensure that the following conditions are true:  
+ You are running a command from the correct Systems Manager document
+ The content of the document has not changed since it was shared with you\.
If the hash does not match the specified document or if the content of the shared document has changed, the command returns an `InvalidDocument` exception\. Note: The hash cannot verify document content from external locations\.