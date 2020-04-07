# Parameter Types and Examples<a name="parameter-store-about-examples"></a>

A Parameter Store parameter is any piece of data that is saved in Parameter Store, such as a block of text, a list of names, a password, an Amazon Machine Image \(AMI\) ID, a license key, and so on\. You can centrally and securely reference this data in your scripts, commands, and SSM documents\.

**Important**  
Do not store sensitive data in a `String` or `StringList` parameter\. For all sensitive data that must remain encrypted, use only the `SecureString` parameter type\.  
For more information, see [SecureString Parameters](sysman-paramstore-securestring.md)\.

When you reference a parameter, you specify the parameter name by using the following convention:

\{\{`ssm:parameter-name`\}\}

**Topics**
+ [Parameter Types](#parameter-types)
+ [Parameter Examples \(AWS CLI\)](#parameter-examples)
+ [Integration Examples from the Community](#community-samples)

## Parameter Types<a name="parameter-types"></a>

Parameter Store provides support for three types of parameters\. `String`, `StringList`, and `SecureString`\. 

String  
The content of a `String` parameter is an unvalidated plain text string\. For example:  
+ `abc123`
+ `Confidential. Do Not Distribute.`
+ `Example Corp.`

StringList  
`StringList` parameters contain a comma\-separated list of values\. For example:  
`Monday,Wednesday,Friday`  
`CSV,TSV,CLF,ELF,JSON`

SecureString  
The `SecureString` parameter type can be used for textual data that you want to encrypt, such as passwords, application secrets, confidential configuration data, or any other types of data you need to protect\. `SecureString` data is encrypted and decrypted using a AWS Key Management Service \(KMS\) key\. You can use either a default KMS key provided by AWS or create and use your own customer master key \(CMK\)\.  
Parameter Store is also integrated with AWS Secrets Manager\. You can retrieve Secrets Manager secrets when using other AWS services that already support references to Parameter Store parameters\. For more information, see [Referencing AWS Secrets Manager Secrets from Parameter Store Parameters](integration-ps-secretsmanager.md) in this guide\.

## Parameter Examples \(AWS CLI\)<a name="parameter-examples"></a>

The following is an example of a Systems Manager parameter named `DNS-IP`\. The value of this parameter is simply the IP address of an instance\. This example uses an AWS CLI command to echo the parameter value\.

```
aws ssm send-command --document-name "AWS-RunPowerShellScript" --document-version "1" --targets "Key=instanceids,Values=i-02573cafcfEXAMPLE" --parameters "commands='echo {{ssm:DNS-IP}}'" --timeout-seconds 600 --max-concurrency "50" --max-errors "0" --region us-east-2
```

The next example command uses a `SecureString` parameter named **SecurePassword**\. The command `commands=['$secure = (Get-SSMParameterValue -Names SecurePassword -WithDecryption $True).Parameters[0].Value','net user administrator $secure']` retrieves and decrypts the value of the `SecureString` parameter, and then resets the local administrator password without having to pass the password in clear text\.

```
aws ssm send-command --document-name "AWS-RunPowerShellScript" --document-version "1" --targets "Key=instanceids,Values=i-02573cafcfEXAMPLE" --parameters "commands=['$secure = (Get-SSMParameterValue -Names SecurePassword -WithDecryption $True).Parameters[0].Value','net user administrator $secure']" --timeout-seconds 600 --max-concurrency "50" --max-errors "0" --region us-east-2
```

You can also reference Systems Manager parameters in the *Parameters* section of an SSM document, as shown in the following example\.

```
{
   "schemaVersion":"2.0",
   "description":"Sample version 2.0 document v2",
   "parameters":{
      "commands" : {
        "type": "StringList",
        "default": ["{{ssm:parameter_name}}"]
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

Don't confuse the similar syntax for *local parameters* used in the `runtimeConfig` section of SSM documents with Parameter Store parameters\. A local parameter isn't the same as a Systems Manager parameter\. You can distinguish local parameters from Systems Manager parameters by the absence of the `ssm:` prefix:

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

**Note**  
SSM documents currently don't support references to `SecureString` parameters\. This means that to use `SecureString` parameters with, for example, Run Command, you have to retrieve the parameter value before passing it to Run Command, as shown in the following examples:  
**AWS CLI**  

```
value=$(aws ssm get-parameters --names parameter_name --with-decryption)
```

```
aws ssm send-command –name AWS-JoinDomain –parameters password=$value –instance-id instance-id
```
**Tools for Windows PowerShell**  

```
$secure = (Get-SSMParameterValue -Names parameter_name -WithDecryption $True).Parameters[0].Value | ConvertTo-SecureString -AsPlainText -Force
```

```
$cred = New-Object System.Management.Automation.PSCredential -argumentlist user_name,$secure
```

## Integration Examples from the Community<a name="community-samples"></a>

The following section provide links to blog posts, articles, and community\-provided examples for using Parameter Store parameters\. 

**Note**  
These links are provided for informational purposes only, and should not be considered either a comprehensive list or an endorsement of the content of the examples\. AWS is not responsible for the content or accuracy of external content\. 
+ [Keep Your Secrets Safe with AWS Systems Manager Parameter Store and Node](https://www.codebyamir.com/blog/keep-your-secrets-safe-with-aws-systems-manager-parameter-store-and-nodejs)

  Amir Boroumand walks us through how to save and retrieve a password using the Parameter Store with Node\.

  *October 19, 2019*
+ [Using SSM Parameters with CloudFormation Templates and Terraform Projects](https://start.jcolemorrison.com/using-ssm-parameters-with-cloudformation-templates-and-terraform-projects/)

  J Cole Morrison demonstrates how to create parameters for use in AWS CloudFormation templates and Terraform projects so you can reference values with less human error\.

  *August 29, 2019*
+ [Using AWS Systems Manager Parameter Store for Configuration](https://github.com/aws-samples/aws-net-guides/tree/master/Communications/ParameterStore-Example)

  Learn how to store and retrieve application configuration settings at runtime for an application instead of hard\-coding the configuration values into a sample application's code and configuration files\. The sample application, a simple \.NET Core console application, shows how to use the AWS SDK for \.NET to retrieve configuration values from Parameter Store\.

  *March 8, 2019*
+ [Using AWS Systems Manager Parameter Store SecureString parameters in AWS CloudFormation templates](http://aws.amazon.com/blogs/mt/using-aws-systems-manager-parameter-store-secure-string-parameters-in-aws-cloudformation-templates/)

  Learn how to use plain text and `SecureString` parameters in your AWS CloudFormation templates through the use of dynamic references to fetch parameter values\.

  *October 5, 2018*
+ [Use parameter labels for easy configuration update across environments](http://aws.amazon.com/blogs/mt/use-parameter-labels-for-easy-configuration-update-across-environments/)

  Learn how to use a parameter label as an alias for a parameter version\. You can group parameter versions across hierarchies using labels, and you can deploy new updates to parameters across environments using hierarchies and labels\.

  *July 26, 2018*
+ [Integrating AWS CloudFormation with AWS Systems Manager Parameter Store](http://aws.amazon.com/blogs/mt/integrating-aws-cloudformation-with-aws-systems-manager-parameter-store/)

  Learn how to use Parameter Store parameters in your AWS CloudFormation templates to simplify stack updates involving parameters and achieve consistency by using values stored in Parameter Store\. With this integration, your code remains untouched while the stack update operation automatically picks up the latest parameter value\.

  *December 28, 2017*
+ [The Right Way to Store Secrets using Parameter Store](http://aws.amazon.com/blogs/mt/the-right-way-to-store-secrets-using-parameter-store/)

  Evan Johnson presents a case study in how Segment centrally and securely manages their secrets using a combination of Parameter Store, lots of Terraform code, and chamber, with a focus on getting up and running with Parameter Store in production\.

  *August 27, 2017*
+ [Join a Microsoft Active Directory Domain with Parameter Store and Amazon EC2 Systems Manager Documents](http://aws.amazon.com/blogs/mt/join-a-microsoft-active-directory-domain-with-parameter-store-and-amazon-ec2-systems-manager-documents/)

  Read about a scenario for centralized configuration management, with an example of joining EC2 instances to a Microsoft Active Directory\. This post shows you how to launch an EC2 instance that consumes and uses configuration values stored as Parameter Store parameters to join your Active Directory domain\.

  *June 19, 2017*
+ [Use Parameter Store to Securely Access Secrets and Config Data in AWS CodeDeploy](http://aws.amazon.com/blogs/mt/use-parameter-store-to-securely-access-secrets-and-config-data-in-aws-codedeploy/)

  Learn how to simplify your CodeDeploy workflows by using Parameter Store to store and reference a configuration secret\. Doing so not only improves your security posture, but also automates your deployment because you don’t have to manually change configuration data in your source code\.

  *March 23, 2017*
+ [Secrets in AWS](https://stp5.net/blog/post/secrets-in-aws/)

  Stephen Price describes how you can use Parameter Store to manage and use secrets in your favorite programming language to handle secrets for cloud\-based architectures, such as microservices or containerized applications\.

  *March 16, 2017*
+ [Simple Secrets Management via AWS’ EC2 Parameter Store](https://medium.com/@mda590/simple-secrets-management-via-aws-ec2-parameter-store-737477e19450#.nv1xue88l)

  Matt Adorjan shows how to protect your AWS environment by securely storing secrets with Parameter Store and controlling access to secrets with AWS Identity and Access Management \(IAM\) AWS Identity and Access with Parameter Store and controlling access to secrets with [AWS Identity and Access Management \(IAM\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/)\.

  *March 15, 2017*
+ [Using Parameter Store with AWS CodePipeline](https://stelligent.com/2017/03/09/using-parameter-store-with-aws-codepipeline/)

  Trey McElhattan demonstrates how to effectively use Parameter Store as part of a continuous delivery pipeline using AWS CodePipeline\. 

  *March 9, 2017*
+ [Managing Secrets for Amazon ECS Applications Using Parameter Store and IAM Roles for Tasks](http://aws.amazon.com/blogs/compute/managing-secrets-for-amazon-ecs-applications-using-parameter-store-and-iam-roles-for-tasks/)

  By using Parameter Store and task IAM roles, you can create a central secret management store and a well\-integrated access layer that allows applications to access only the keys they need, to restrict access on a container basis, and to further encrypt secrets with custom KMS keys\.

  *January 21, 2017*