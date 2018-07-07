# Working with Predefined Automation Documents<a name="automation-awsdocs"></a>

To help you get started quickly, Systems Manager provides the following pre\-defined Automation documents\. These documents are maintained by Amazon Web Services\. 

**Note**  
Any document name that includes *WithApproval*, means the document includes the [aws:approve](automation-actions.md#automation-action-approve) action\. This action temporarily pauses an Automation execution until designated principals either approve or reject the action\. After the required number of approvals is reached, the Automation execution resumes\. 

You can view the JSON for these document in the Systems Manager console\.

1. Open the AWS Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

   \-or\-

   If the AWS Systems Manager home page opens first, choose the menu icon \(![\[Image NOT FOUND\]](http://docs.aws.amazon.com/systems-manager/latest/userguide/images/menu-icon-small.png)\) to open the navigation pane, and then choose **Documents** in the navigation pane\.

1. Choose a document, and then choose **View details**\.

1. Choose the **Content** tab\.


****  

| Document Name\(s\) | Purpose | 
| --- | --- | 
|  AWS\-DeleteCloudFormationStack AWS\-DeleteCloudFormationStackWithApproval  |  Use this document to delete a AWS CloudFormation stack\. You must specify the stack ID\.  | 
|  AWS\-RestartEC2Instance AWS\-RestartEC2InstanceWithApproval  |  Use this document to restart one or more Amazon EC2 instances \(Windows or Linux\)\. Separate instance IDs with quotation marks and commas\. For example, \["i\-0123abcd0123abcd0", "i\-abcd0123abcd0123a"\]\. If you want to change the state of one or more instances from a custom Automation document, then specify the [aws:changeInstanceState](automation-actions.md#automation-action-changestate) action in your document\.  | 
|  AWS\-StartEC2Instance AWS\-StartEC2InstanceWithApproval  |  Use this document to start one or more Amazon EC2 instances \(Windows or Linux\)\. Separate instance IDs with quotation marks and commas\. For example, \["i\-0123abcd0123abcd0", "i\-abcd0123abcd0123a"\]\. If you want to change the state of one or more instances from a custom Automation document, then specify the [aws:changeInstanceState](automation-actions.md#automation-action-changestate) action in your document\.  | 
|  AWS\-StopEC2Instance AWS\-StopEC2InstanceWithApproval  |  Use this document to stop one or more Amazon EC2 instances \(Windows or Linux\)\. Separate instance IDs with quotation marks and commas\. For example, \["i\-0123abcd0123abcd0", "i\-abcd0123abcd0123a"\]\. If you want to change the state of one or more instances from a custom Automation document, then specify the [aws:changeInstanceState](automation-actions.md#automation-action-changestate) action in your document\.  | 
|  AWS\-TerminateEC2Instance AWS\-TerminateEC2InstanceWithApproval  |  Use this document to terminate one or more Amazon EC2 instances \(Windows or Linux\)\. Separate instance IDs with quotation marks and commas\. For example, \["i\-0123abcd0123abcd0", "i\-abcd0123abcd0123a"\]\. If you want to change the state of one or more instances from a custom Automation document, then specify the [aws:changeInstanceState](automation-actions.md#automation-action-changestate) action in your document\.  | 
|  AWS\-UpdateCloudFormationStack AWS\-UpdateCloudFormationStackWithApproval  |  Use this document to perform an update operation on an existing stack by using a specified template\. You must specify a URL to the template that will be used to update the stack\. These documents run a AWS Lambda function\. You must provide an Amazon Resource Name \(ARN\) for an IAM role that Lambda can use to run the function\.  | 
|  AWS\-UpdateLinuxAmi  |  Use this document to automate image\-maintenance tasks\. For more information, see [Walkthrough: Customize and Update Linux AMIs Using AWS\-UpdateLinuxAmi](automation-awsdocs-linux.md)  | 
|  AWS\-UpdateWindowsAmi  |  Use this document to automate image\-maintenance tasks\. For more information, see [Walkthrough: Customize and Update Windows AMIs Using AWS\-UpdateWindowsAmi](automation-awsdocs-win.md)  | 
|  AWSSupport\-ExecuteEC2Rescue  |  Use this document to diagnose and troubleshoot problems on Amazon EC2 instances\. For more information, see [Run the EC2Rescue Tool on Unreachable Instances](automation-ec2rescue.md)\.  | 
|  AWSSupport\-ResetAccess  |  Use this document to automatically reenable local Administrator password generation on Amazon EC2 Windows instances\. For more information, see [Reset Passwords and SSH Keys on Amazon EC2 Instances](automation-ec2reset.md)\.  | 

**Topics**
+ [Walkthrough: Customize and Update Linux AMIs Using AWS\-UpdateLinuxAmi](automation-awsdocs-linux.md)
+ [Walkthrough: Customize and Update Windows AMIs Using AWS\-UpdateWindowsAmi](automation-awsdocs-win.md)