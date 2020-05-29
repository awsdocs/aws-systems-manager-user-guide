# Troubleshooting Systems Manager Automation<a name="automation-troubleshooting"></a>

Use the following information to help you troubleshoot problems with the Automation service\. This topic includes specific tasks to resolve issues based on Automation error messages\.

**Topics**
+ [Common Automation errors](#automation-trbl-common)
+ [Automation execution failed to start](#automation-trbl-access)
+ [Execution started, but status is failed](#automation-trbl-exstrt)
+ [Execution started, but timed out](#automation-trbl-to)

## Common Automation errors<a name="automation-trbl-common"></a>

This section includes information about common Automation errors\.

### VPC not defined 400<a name="automation-trbl-common-vpc"></a>

By default, when Automation runs either the `AWS-UpdateLinuxAmi` document or the `AWS-UpdateWindowsAmi` document, the system creates a temporary instance in the default VPC \(172\.30\.0\.0/16\)\. If you deleted the default VPC, you will receive the following error:

VPC not defined 400

To solve this problem, you must specify a value for the `SubnetId` input parameter\.

## Automation execution failed to start<a name="automation-trbl-access"></a>

An Automation execution can fail with an access denied error or an invalid assume role error if you have not properly configured IAM users, roles, and policies for Automation\.

### Access denied<a name="automation-trbl-access-denied"></a>

The following examples describe situations when an Automation execution failed to start with an access denied error\.

**Access Denied to Systems Manager API**  
**Error message**: `User: user arn is not authorized to perform: ssm:StartAutomationExecution on resource: document arn (Service: AWSSimpleSystemsManagement; Status Code: 400; Error Code: AccessDeniedException; Request ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)`
+ Possible cause 1: The IAM user attempting to start the Automation execution does not have permission to invoke the `StartAutomationExecution` API\. To resolve this issue, attach the required IAM policy to the user account that was used to start the execution\. For more information, see [Task 3: Configure user access to Automation](automation-permissions.md#automation-passrole)\. 
+ Possible cause 2: The IAM user attempting to start the Automation execution has permission to invoke the `StartAutomationExecution` API, but does not have permission to invoke the API by using the specific Automation document\. To resolve this issue, attach the required IAM policy to the user account that was used to start the execution\. For more information, see [Task 3: Configure user access to Automation](automation-permissions.md#automation-passrole)\.

**Access Denied Because of Missing PassRole Permissions**  
**Error message**: `User: user arn is not authorized to perform: iam:PassRole on resource: automation assume role arn (Service: AWSSimpleSystemsManagement; Status Code: 400; Error Code: AccessDeniedException; Request ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)`

The IAM user attempting to start the Automation execution does not have PassRole permission for the assume role\. To resolve this issue, attach the iam:PassRole policy to the role of the IAM user attempting to start the Automation execution\. For more information, see [Task 2: Attach the iam:PassRole policy to your Automation role](automation-permissions.md#automation-passpolicy)\.

### Invalid assume role<a name="automation-trbl-ar"></a>

When you run an Automation, an assume role is either provided in the document or passed as a parameter value for the document\. Different types of errors can occur if the assume role is not specified or configured properly\.

**Malformed Assume Role**  
**Error message**: `The format of the supplied assume role ARN is invalid.` The assume role is improperly formatted\. To resolve this issue, verify that a valid assume role is specified in your Automation document or as a runtime parameter when running the Automation\.

**Assume Role Can't Be Assumed**  
**Error message**: `The defined assume role is unable to be assumed. (Service: AWSSimpleSystemsManagement; Status Code: 400; Error Code: InvalidAutomationExecutionParametersException; Request ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)`
+ Possible cause 1: The assume role does not exist\. To resolve this issue, create the role\. For more information, see [Getting started with Automation](automation-setup.md)\. Specific details for creating this role are described in the following topic, [Task 1: Create a service role for Automation](automation-permissions.md#automation-role)\.
+ Possible cause 2: The assume role does not have a trust relationship with the Systems Manager service\. To resolve this issue, create the trust relationship\. For more information, see [I Can't Assume A Role](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_roles.html#troubleshoot_roles_cant-assume-role) in the *IAM User Guide*\. 

## Execution started, but status is failed<a name="automation-trbl-exstrt"></a>

### Action\-specific failures<a name="automation-trbl-actspec"></a>

Automation documents contain steps and steps run in order\. Each step invokes one or more AWS service APIs\. The APIs determine the inputs, behavior, and outputs of the step\. There are multiple places where an error can cause a step to fail\. Failure messages indicate when and where an error occurred\.

To see a failure message in the EC2 console, choose the **View Outputs** link of the failed step\. To see a failure message from the AWS CLI, call `get-automation-execution` and look for the `FailureMessage` attribute in a failed `StepExecution`\.

In the following examples, a step associated with the `aws:runInstance` action failed\. Each example explores a different type of error\.

**Missing Image**  
**Error message**: `Automation Step Execution fails when it is launching the instance(s). Get Exception from RunInstances API of ec2 Service. Exception Message from RunInstances API: [The image id '[ami id]' does not exist (Service: AmazonEC2; Status Code: 400; Error Code: InvalidAMIID.NotFound; Request ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)]. Please refer to Automation Service Troubleshooting Guide for more diagnosis details.`

The `aws:runInstances` action received input for an `ImageId` that doesn't exist\. To resolve this problem, update the automation document or parameter values with the correct AMI ID\.

**Assume Role Policy Doesn't Have Sufficient Permissions**  
**Error message**: `Automation Step Execution fails when it is launching the instance(s). Get Exception from RunInstances API of ec2 Service. Exception Message from RunInstances API: [You are not authorized to perform this operation. Encoded authorization failure message: xxxxxxx (Service: AmazonEC2; Status Code: 403; Error Code: UnauthorizedOperation; Request ID: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)]. Please refer to Automation Service Troubleshooting Guide for more diagnosis details.`

The assume role doesn't have sufficient permission to invoke the `RunInstances` API on EC2 instances\. To resolve this problem, attach an IAM policy to the assume role that has permission to invoke the `RunInstances` API\. For more information, see the [Method 2: Use IAM to configure roles for Automation](automation-permissions.md)\.

**Unexpected State**  
**Error message**: `Step fails when it is verifying launched instance(s) are ready to be used. Instance i-xxxxxxxxx entered unexpected state: shutting-down. Please refer to Automation Service Troubleshooting Guide for more diagnosis details.`
+ Possible cause 1: There is a problem with the instance or the Amazon EC2 service\. To resolve this problem, login to the instance or review the instance system log to understand why the instance started shutting down\.
+ Possible cause 2: The user data script specified for the `aws:runInstances` action has a problem or incorrect syntax\. Verify the syntax of the user data script\. Also, verify that the user data scripts doesn't shut down the instance, or invoke other scripts that shut down the instance\.

**Action\-Specific Failures Reference**  
When a step fails, the failure message might indicate which service was being invoked when the failure occurred\. The following table lists the services invoked by each action\. The table also provides links to information about each service\.


****  

| Action | AWS Service\(s\) invoked by this action | For information about this service | Troubleshooting content | 
| --- | --- | --- | --- | 
| aws:runInstances | Amazon EC2 | [ Amazon EC2 User Guide for Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/) | [Troubleshooting EC2 Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-troubleshoot.html) | 
| aws:changeInstanceState | Amazon EC2 | [ Amazon EC2 User Guide for Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/) | [Troubleshooting EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-troubleshoot.html) | 
| aws:runCommand | Systems Manager |  [AWS Systems Manager Run Command](execute-remote-commands.md) |  [Troubleshooting Systems Manager Run Command](troubleshooting-remote-commands.md) | 
| aws:createImage | Amazon EC2 | [Amazon Machines Images](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) |  | 
| aws:createStack | AWS CloudFormation | [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) | [Troubleshooting AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html) | 
| aws:deleteStack | AWS CloudFormation | [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html) | [Troubleshooting AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html) | 
| aws:deleteImage | Amazon EC2 | [Amazon Machines Images](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) |  | 
| aws:copyImage | Amazon EC2 | [Amazon Machines Images](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) |  | 
| aws:createTag | Amazon EC2, Systems Manager | [EC2 Resource and Tags](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_Resources.html) |  | 
| aws:invokeLambdaFunction | AWS Lambda | [AWS Lambda Developer Guide](https://docs.aws.amazon.com/lambda/latest/dg/) | [Troubleshooting Lambda](https://docs.aws.amazon.com/lambda/latest/dg/monitoring-functions.html) | 

### Automation service internal error<a name="automation-trbl-err"></a>

**Error message**: `Internal Server Error. Please refer to Automation Service Troubleshooting Guide for more diagnosis details.`

A problem with the Automation service is preventing the specified Automation document from running correctly\. To resolve this issue, contact AWS Support\. Provide the execution ID and customer ID, if available\.

## Execution started, but timed out<a name="automation-trbl-to"></a>

**Error message**: `Step timed out while step is verifying launched instance(s) are ready to be used. Please refer to Automation Service Troubleshooting Guide for more diagnosis details.`

A step in the `aws:runInstances` action timed out\. This can happen if the step action takes longer to run than the value specified for `timeoutSeconds` in the step\. To resolve this issue, specify a longer value for `timeoutSeconds`\. If that does not solve the problem, investigate why the step takes longer to run than expected\.