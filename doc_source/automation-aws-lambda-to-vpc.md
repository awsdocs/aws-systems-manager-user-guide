# AWSConfigRemediation\-MoveLambdaToVPC<a name="automation-aws-lambda-to-vpc"></a>

**Description**

The AWSConfigRemediation\-MoveLambdaToVPC runbook moves an AWS Lambda \(Lambda\) function to an Amazon Virtual Private Cloud \(Amazon VPC\)\.

[Run this Automation \(console\)](https://console.aws.amazon.com/systems-manager/automation/execute/AWSConfigRemediation-MoveLambdaToVPC)

**Document type**

Automation

**Owner**

Amazon

**Platforms**

Linux, macOS, Windows

**Parameters**
+ AutomationAssumeRole

  Type: String

  Description: \(Required\) The Amazon Resource Name \(ARN\) of the AWS Identity and Access Management \(IAM\) role that allows Systems Manager Automation to perform the actions on your behalf\.
+ FunctionName

  Type: String

  Description: \(Required\) The name of the Lambda function to move to an Amazon VPC\.
+ SecurityGroupIds

  Type: String

  Description: \(Required\) The security group IDs you want to assign to the elastic network interfaces \(ENIs\) associated with your Lambda function\.
+ SubnetIds

  Type: String

  Description: \(Required\) The subnet IDs you want to create the elastic network interfaces \(ENIs\) associated with your Lambda function\.

**Required IAM permissions**

The `AutomationAssumeRole` parameter requires the following actions to successfully use the runbook\.
+ `ssm:StartAutomationExecution`
+ `ssm:GetAutomationExecution`
+ `lambda:GetFunction`
+ `lambda:GetFunctionConfiguration`
+ `lambda:UpdateFunctionConfiguration`

**Document Steps**
+ aws:executeAwsApi \- Updates the Amazon VPC configuration for the Lambda function you specify in the `FunctionName` parameter\.
+ aws:waitForAwsResourceProperty \- Waits for the Lambda function `LastUpdateStatus` to be `successful`\.
+ aws:executeScript \- Verifies the Lambda function Amazon VPC configuration has been successfully updated\.