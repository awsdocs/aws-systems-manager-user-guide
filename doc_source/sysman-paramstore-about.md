# About Systems Manager Parameters<a name="sysman-paramstore-about"></a>

You can reference Systems Manager parameters in your scripts, commands, and configuration and automation workflows\. Parameters work with Systems Manager capabilities such as Run Command, State Manager, and Automation\. You can also reference parameters in other AWS services such as Amazon Elastic Container Service and AWS Lambda\. 

With Systems Manager capabilities, you can reference Systems Manager parameters in your AWS CLI or AWS Tools for Windows PowerShell commands or scripts\. You can also reference parameters in SSM documents\. For more information about SSM documents, see [AWS Systems Manager Documents](sysman-ssm-docs.md)\.

**Topics**
+ [Parameter Usage Examples](#parameter-store-about-examples)
+ [Use Secure String Parameters](#sysman-paramstore-securestring)

## Parameter Usage Examples<a name="parameter-store-about-examples"></a>

The following is an example of a Systems Manager parameter in an AWS CLI command for Run Command\. Systems Manager Parameters are always prefixed with `ssm:`\.

```
aws ssm send-command --instance-ids i-1a2b3c4d5e6f7g8 --document-name AWS-RunPowerShellScript --parameter '{"commands":["echo {{ssm:parameter name}}"]}'
```

You can also reference Systems Manager parameters in the *Parameters* section of an SSM document, as shown in the following example\.

```
{
   "schemaVersion":"2.0",
   "description":"Sample version 2.0 document v2",
   "parameters":{
      "commands" : {
        "type": "StringList",
        "default": ["{{ssm:parameter name}}"]
      }
    },
    "mainSteps":[
       {
          "action":"aws:runShellScript",
          "name":"runShellScript",
          "inputs":{
             "runCommand": "{{commands}}"
          }
       }
    ]
}
```

**Note**  
The runtimeConfig section of SSM documents use similar syntax for *local parameters*\. A local parameter isn't the same as a Systems Manager parameter\. You can distinguish local parameters from Systems Manager parameters by the absence of the `ssm:` prefix\.  

```
 "runtimeConfig":{
        "aws:runShellScript":{
            "properties":[
                {
                    "id":"0.aws:runShellScript",
                    "runCommand":"{{ commands }}",
                    "workingDirectory":"{{ workingDirectory }}",
                    "timeoutSeconds":"{{ executionTimeout }}"
```

SSM documents currently don't support references to Secure String parameters\. This means that to use Secure String parameters with, for example, Run Command, you have to retrieve the parameter value before passing it to Run Command, as shown in the following examples:

**AWS CLI**

```
$value=aws ssm get-parameters --names the parameter name --with-decryption
```

```
aws ssm send-command –name AWS-JoinDomain –parameters password=$value –instance-id the instance ID
```

**Tools for Windows PowerShell**

```
$secure = (Get-SSMParameterValue -Names the parameter name -WithDecryption $True).Parameters[0].Value | ConvertTo-SecureString -AsPlainText -Force
```

```
$cred = New-Object System.Management.Automation.PSCredential -argumentlist user name,$secure
```

## Use Secure String Parameters<a name="sysman-paramstore-securestring"></a>

A Secure String parameter is any sensitive data that needs to be stored and referenced in a secure manner\. If you have data that you don't want users to alter or reference in clear text, such as passwords or license keys, then create those parameters using the Secure String data type\. We recommend using Secure String parameters for the following scenarios\.
+ You want to use data/parameters across AWS services without exposing the values as clear text in commands, functions, agent logs, or AWS CloudTrail logs\.
+ You want to control who has access to sensitive data\.
+ You want to be able to audit when sensitive data is accessed \(AWS CloudTrail\)\.
+ You want AWS\-level encryption for your sensitive data and you want to bring your own encryption keys to manage access\.

If you choose the Secure String data type when you create your parameter, then AWS KMS encrypts the parameter value\. For more information, see [How AWS Systems Manager Parameter Store Uses AWS KMS](http://docs.aws.amazon.com/kms/latest/developerguide/services-parameter-store.html) in the *AWS Key Management Service Developer Guide*\.

**Important**  
Only the value of a secure string parameter is encrypted\. Parameter names, descriptions, and other properties are not encrypted\. Therefore, to make it less obvious which parameters contain passwords, we recommend using a naming system that avoids the actual word "password" in your parameter names\. For illustration, however, we are using the sample parameter name *my\-password* in our examples\.

### Create a Secure String Parameter Using the Default KMS CMK<a name="sysman-param-defaultkms"></a>

Systems Manager includes a default AWS KMS customer master key \(CMK\)\. You can view this key by executing the following command from the AWS CLI:

```
aws kms describe-key --key-id alias/aws/ssm
```

If you create a Secure String parameter using the default KMS CMK, then you *don't* have to provide a value for the `--key-id` parameter\. The following CLI example shows the command to create a new Secure String parameter in Parameter Store without the `--key-id` parameter: 

```
aws ssm put-parameter --name a_name --value "a value" --type SecureString
```

### Create a Secure String Parameter Using a Custom KMS CMK<a name="sysman-param-customkms"></a>

If you want to use a custom KMS CMK instead of the default CMK assigned to your account, then you must specify the custom KMS CMK by using the `--key-id` parameter\. The parameter supports the following AWS KMS parameter formats\.
+ Key ARN example:

   `arn:aws:kms:us-east-2:123456789012:key/12345678-1234-1234-1234-123456789012`
+ Alias ARN example:

  `arn:aws:kms:us-east-2:123456789012:alias/MyAliasName`
+ Globally Unique Key ID example:

  `12345678-1234-1234-1234-123456789012`
+ Alias Name example:

  `alias/MyAliasName`

You can create a custom AWS KMS CMK from the AWS CLI by using the following commands:

```
aws kms create-key
```

Use a command in the following format to create a Secure String parameter using the key you just created\.

```
aws ssm put-parameter --name a_name --value "a value" --type SecureString --key-id arn:aws:kms:us-east-2:123456789012:key/1a2b3c4d-1a2b-1a2b-1a2b-1a2b3c4d5e
```

**Note**  
You can manually create a parameter with an encrypted value\. In this case, because the value is already encrypted, you don’t have to choose the Secure String data type\. If you do choose Secure String, your parameter will be doubly encrypted\.

By default, all Secure String values are displayed as cipher text\. To decrypt a Secure String value, a user must have permission to call the KMS [Decrypt](http://docs.aws.amazon.com/kms/latest/APIReference/API_Decrypt.html) API action\. For information about configuring KMS access control, see [Authentication and Access Control for AWS KMS](http://docs.aws.amazon.com/kms/latest/developerguide/control-access.html) in the *AWS Key Management Service Developer Guide*\.

### Using Secure String Parameters With Other AWS Services<a name="sysman-paramstore-securelam"></a>

You can also use Secure String parameters with other AWS services\. In the following example, the Lambda function retrieves a Secure String parameter by using the [GetParameters](http://docs.aws.amazon.com/ssm/latest/APIReference/API_GetParameters.html) API\.

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

**Related topics**  
For an example of how to create and use a Secure String parameter, see [Walkthrough: Create a Secure String Parameter and Join an Instance to a Domain \(PowerShell\)](sysman-param-securestring-walkthrough.md)\. For more information about using Systems Manager parameters with other AWS services, see the following blogpost\.
+ [Managing Secrets for Amazon ECS Applications Using Parameter Store and IAM Roles for Tasks](https://aws.amazon.com/blogs/compute/managing-secrets-for-amazon-ecs-applications-using-parameter-store-and-iam-roles-for-tasks/)
+ [Use Parameter Store to Securely Access Secrets and Config Data in AWS CodeDeploy](https://aws.amazon.com/blogs/mt/use-parameter-store-to-securely-access-secrets-and-config-data-in-aws-codedeploy/)
+ [Interesting Articles on Amazon EC2 Systems Manager Parameter Store](https://aws.amazon.com/blogs/mt/interesting-articles-on-ec2-systems-manager-parameter-store/)