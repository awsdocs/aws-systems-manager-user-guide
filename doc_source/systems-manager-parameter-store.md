# AWS Systems Manager Parameter Store<a name="systems-manager-parameter-store"></a>

AWS Systems Manager Parameter Store \(Parameter Store\) provides secure, hierarchical storage for configuration data management and secrets management\. You can store data such as passwords, database strings, Amazon Machine Image \(AMI\) IDs, and license codes as parameter values\. You can store values as plain text or encrypted data\. You can reference Systems Manager parameters in your scripts, commands, SSM documents, and configuration and automation workflows by using the unique name that you specified when you created the parameter\. 

**Note**  
To implement password rotation lifecycles, use AWS Secrets Manager\. Secrets Manager allows you to easily rotate, manage, and retrieve database credentials, API keys, and other secrets throughout their lifecycle\. For more information, see [What is AWS Secrets Manager?](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) in the *AWS Secrets Manager Userguide*\.

## How can Parameter Store benefit my organization?<a name="parameter-store-benefits"></a>

Parameter Store offers these benefits:
+ You can use a secure, scalable, hosted secrets management service with no servers to manage\.
+ Improve your security posture by separating your data from your code\.
+ Store configuration data and encrypted strings in hierarchies and track versions\.
+ Control and audit access at granular levels\.

## Who should use Parameter Store?<a name="parameter-store-who"></a>
+ Any AWS customer who wants to have a centralized way to manage configuration data\.
+ Software developers who want to easily store different logins and reference streams\.
+  Administrators who want to be notified when changes have or have not been made to secrets and passwords\.

## What are the features of Parameter Store?<a name="parameter-store-features"></a>
+ **Change notification**

  You can configure change notifications and trigger automated actions for both parameters and parameter policies\. For more information, see [Setting up notifications or trigger actions based on Parameter Store events](sysman-paramstore-cwe.md)\.
+ **Organize and control access**

  You can tag your parameters individually to help you quickly identify one or more parameters based on the tags you've assigned to them\. For example, you can tag parameters for specific environments, departments, users, groups, or periods\. You can also restrict access to parameters by creating an AWS Identity and Access Management \(IAM\) policy that specifies the tags that a user or group can access\. For more information, see [Tagging Systems Manager parameters](tagging-parameters.md)\.
+ **Label versions**

  You can associate an alias for versions of your parameter by creating labels\. Labels can help you remember the purpose of a parameter version when there are multiple versions\. 
+ **Data validation**

  You can create parameters that point to an Amazon Elastic Compute Cloud \(Amazon EC2\) instance and Parameter Store will validate these parameters to ensure that it references expected resource type, that the resource exists, and that the customer has permission to use the resource\. For example, you can create a parameter with Amazon Machine Image \(AMI\) ID as a value with aws:ec2:image data type, and Parameter Store performs an asynchronous validation operation to ensure that the parameter value meets the formatting requirements for an AMI ID, and that the specified AMI is available in your AWS account\. 
+ **Reference secrets**

  Parameter Store is integrated with AWS Secrets Manager so that you can retrieve Secrets Manager secrets when using other AWS services that already support references to Parameter Store parameters\. 
+ **Accessible from other AWS services**

  You can use Parameter Store parameters with other Systems Manager capabilities and AWS services to retrieve secrets and configuration data from a central store\. Parameters work with Systems Manager capabilities such as AWS Systems Manager Run Command \(Run Command\), AWS Systems Manager State Manager \(State Manager\), and AWS Systems Manager Automation \(Automation\)\. You can also reference parameters in a number of other AWS services, including the following:
  + Amazon Elastic Compute Cloud \(Amazon EC2\)
  + Amazon Elastic Container Service \(Amazon ECS\)
  + AWS Secrets Manager \(Secrets Manager\)
  + AWS Lambda \(Lambda\)
  + AWS CloudFormation \(CloudFormation\)
  + AWS CodeBuild \(CodeBuild\)
  + AWS CodePipeline \(CodePipeline\)
  + AWS CodeDeploy \(CodeDeploy\)
+ **Integrate with other AWS services**

  Configure integration with the following AWS services for encryption, notification, monitoring, and auditing:
  + AWS Key Management Service \(AWS KMS\)
  + Amazon Simple Notification Service \(Amazon SNS\)
  + Amazon CloudWatch \(CloudWatch\): For more information, see [Configuring EventBridge for parameters](sysman-paramstore-cwe.md#cwe-parameter-changes)\. 
  + Amazon EventBridge \(EventBridge\): For more information, see [Monitoring Systems Manager status changes using Amazon SNS notifications](monitoring-sns-notifications.md) and [Reference: Amazon EventBridge event patterns and types for Systems Manager](reference-eventbridge-events.md)\. 
  + AWS CloudTrail \(CloudTrail\): For more information, see [Logging AWS Systems Manager API calls with AWS CloudTrail](monitoring-cloudtrail-logs.md)\.

## What is a parameter?<a name="what-is-a-parameter"></a>

A Parameter Store parameter is any piece of data that is saved in Parameter Store, such as a block of text, a list of names, a password, an AMI ID, a license key, and so on\. You can centrally and securely reference this data in your scripts, commands, and SSM documents\.

When you reference a parameter, you specify the parameter name by using the following convention\.

\{\{`ssm:parameter-name`\}\}

**Note**  
Parameters can't be referenced or nested in the values of other parameters\. You can't include `{{}}` or `{{ssm:parameter-name}}` in a parameter value\.

Parameter Store provides support for three types of parameters: `String`, `StringList`, and `SecureString`\. 

With one exception, when you create or update a parameter, you enter the parameter value as plaintext, and Parameter Store performs no validation on the text you enter\. For `String` parameters, however, you can specify the data type as `aws:ec2:image`, and Parameter Store validates that the value you enter is the proper format for an Amazon EC2 AMI; for example: `ami-12345abcdeEXAMPLE`\.

String  
By default, `String` parameters consist of any block of text you enter\. For example:  
+ `abc123`
+ `Example Corp`
+ `<img src="images/bannerImage1.png"/>`

StringList  
`StringList` parameters contain a comma\-separated list of values, as shown in the following examples\.  
`Monday,Wednesday,Friday`  
`CSV,TSV,CLF,ELF,JSON`

SecureString  
A `SecureString` parameter is any sensitive data that needs to be stored and referenced in a secure manner\. If you have data that you don't want users to alter or reference in plaintext, such as passwords or license keys, create those parameters using the `SecureString` datatype\.  
Do not store sensitive data in a `String` or `StringList` parameter\. For all sensitive data that must remain encrypted, use only the `SecureString` parameter type\.  
For more information, see [Create a SecureString parameter \(AWS CLI\)](param-create-cli.md#param-create-cli-securestring)\.
We recommend using `SecureString` parameters for the following scenarios:  
+ You want to use data/parameters across AWS services without exposing the values as plaintext in commands, functions, agent logs, or CloudTrail logs\.
+ You want to control who has access to sensitive data\.
+ You want to be able to audit when sensitive data is accessed \(CloudTrail\)\.
+ You want to encrypt your sensitive data, and you want to bring your own encryption keys to manage access\.
Only the *value* of a `SecureString` parameter is encrypted\. Parameter names, descriptions, and other properties are not encrypted\.
The `SecureString` parameter type can be used for textual data that you want to encrypt, such as passwords, application secrets, confidential configuration data, or any other types of data you need to protect\. `SecureString` data is encrypted and decrypted using an AWS KMS key\. You can use either a default KMS key provided by AWS or create and use your own customer master key \(CMK\)\. \(Use your own CMK if you need to restrict user access to `SecureString` parameters\. For more information, see [IAM permissions for using AWS default keys and customer managed keys](sysman-paramstore-access.md#ps-kms-permissions)\.\)  
You can also use `SecureString` parameters with other AWS services\. In the following example, the Lambda function retrieves a `SecureString` parameter by using the [GetParameters](https://docs.aws.amazon.com/ssm/latest/APIReference/API_GetParameters.html) API\.  

```
from __future__ import print_function
 
import json
import boto3
ssm = boto3.client('ssm', 'us-east-2')
 def get_parameters():
    response = ssm.get_parameters(
        Names=['LambdaSecureString'],WithDecryption=True
    )
    for parameter in response['Parameters']:
        return parameter['Value']
        
def lambda_handler(event, context):
    value = get_parameters()
    print("value1 = " + value)
    return value  # Echo back the first key value
```
Parameter Store is also integrated with Secrets Manager\. You can retrieve Secrets Manager secrets when using other AWS services that already support references to Parameter Store parameters\. For more information, see [Referencing AWS Secrets Manager secrets from Parameter Store parameters](integration-ps-secretsmanager.md) in this guide\.
**AWS KMS encryption and pricing**  
If you choose the `SecureString` parameter type when you create your parameter, Systems Manager uses AWS KMS to encrypt the parameter value\.
There is no charge from Parameter Store to create a `SecureString` parameter, but charges for use of AWS KMS encryption do apply\. For information, see [AWS Key Management Service pricing](https://aws.amazon.com/kms/pricing)\.  
For more information about AWS managed and customer managed CMKs, see [AWS Key Management Service Concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html) in the *AWS Key Management Service Developer Guide*\. For more information about Parameter Store and AWS KMS encryption, see [How AWS Systems Manager Parameter Store Uses AWS KMS](https://docs.aws.amazon.com/kms/latest/developerguide/services-parameter-store.html)\.  
To view an AWS managed CMK, use the AWS KMS `DescribeKey` operation\. This AWS Command Line Interface \(AWS CLI\) example uses `DescribeKey` to view and AWS\-managed CMK\.  

```
aws kms describe-key --key-id alias/aws/ssm
```
**Related topics**  
For an example of how to create and use a `SecureString` parameter, see [Create a SecureString parameter and join an instance to a Domain \(PowerShell\)](sysman-paramstore-walk.md#sysman-param-securestring-walkthrough)\. For more information about using Systems Manager parameters with other AWS services, see the following blog posts:
+ [Managing Secrets for Amazon ECS Applications Using Parameter Store and IAM Roles for Tasks](http://aws.amazon.com/blogs/compute/managing-secrets-for-amazon-ecs-applications-using-parameter-store-and-iam-roles-for-tasks/)
+ [Use Parameter Store to Securely Access Secrets and Config Data in CodeDeploy](http://aws.amazon.com/blogs/mt/use-parameter-store-to-securely-access-secrets-and-config-data-in-aws-codedeploy/)
+ [Interesting Articles on Amazon EC2 Systems Manager Parameter Store](http://aws.amazon.com/blogs/mt/interesting-articles-on-ec2-systems-manager-parameter-store/)