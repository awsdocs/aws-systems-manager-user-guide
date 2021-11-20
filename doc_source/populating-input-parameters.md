# Creating input parameters that populate AWS resources<a name="populating-input-parameters"></a>

Automation, a capability of Systems Manager, populates AWS resources in the AWS Management Console that match the resource type you define for an input parameter\. Resources in your AWS account that match the resource type are displayed in a dropdown list for you to choose\. You can define input parameter types for Amazon Elastic Compute Cloud \(Amazon EC2\) instances, Amazon Simple Storage Service \(Amazon S3\) buckets, and AWS Identity and Access Management \(IAM\) roles\. The supported type definitions and the regular expressions used to locate matching resources are as follows:
+ `AWS::EC2::Instance::Id` \- `^m?i-[a-z0-9]{8,17}$`
+ `List<AWS::EC2::Instance::Id>` \- `^m?i-[a-z0-9]{8,17}$`
+ `AWS::S3::Bucket::Name` \- `^[0-9a-z][a-z0-9\\-\\.]{3,63}$`
+ `List<AWS::S3::Bucket::Name>` \- `^[0-9a-z][a-z0-9\\-\\.]{3,63}$`
+ `AWS::IAM::Role::Arn` \- `^arn:(aws|aws-cn|aws-us-gov|aws-iso|aws-iso-b):iam::[0-9]{12}:role/.*$`
+ `List<AWS::IAM::Role::Arn>` \- `^arn:(aws|aws-cn|aws-us-gov|aws-iso|aws-iso-b):iam::[0-9]{12}:role/.*$`

The following is an example of input parameter types defined in runbook content\.

------
#### [ YAML ]

```
description: Enables encryption on an Amazon S3 bucket
schemaVersion: '0.3'
assumeRole: '{{ AutomationAssumeRole }}'
parameters:
  BucketName:
    type: 'AWS::S3::Bucket::Name'
    description: (Required) The name of the Amazon S3 bucket you want to encrypt.
  SSEAlgorithm:
    type: String
    description: (Optional) The server-side encryption algorithm to use for the default encryption.
    default: AES256
  AutomationAssumeRole:
    type: 'AWS::IAM::Role::Arn'
    description: (Optional) The Amazon Resource Name (ARN) of the role that allows Automation to perform the actions on your behalf.
    default: ''
mainSteps:
  - name: enableBucketEncryption
    action: 'aws:executeAwsApi'
    inputs:
      Service: s3
      Api: PutBucketEncryption
      Bucket: '{{BucketName}}'
      ServerSideEncryptionConfiguration:
        Rules:
          - ApplyServerSideEncryptionByDefault:
              SSEAlgorithm: '{{SSEAlgorithm}}'
    isEnd: true
```

------
#### [ JSON ]

```
{
   "description": "Enables encryption on an Amazon S3 bucket",
   "schemaVersion": "0.3",
   "assumeRole": "{{ AutomationAssumeRole }}",
   "parameters": {
      "BucketName": {
         "type": "AWS::S3::Bucket::Name",
         "description": "(Required) The name of the Amazon S3 bucket you want to encrypt."
      },
      "SSEAlgorithm": {
         "type": "String",
         "description": "(Optional) The server-side encryption algorithm to use for the default encryption.",
         "default": "AES256"
      },
      "AutomationAssumeRole": {
         "type": "AWS::IAM::Role::Arn",
         "description": "(Optional) The Amazon Resource Name (ARN) of the role that allows Automation to perform the actions on your behalf.",
         "default": ""
      }
   },
   "mainSteps": [
      {
         "name": "enableBucketEncryption",
         "action": "aws:executeAwsApi",
         "inputs": {
            "Service": "s3",
            "Api": "PutBucketEncryption",
            "Bucket": "{{BucketName}}",
            "ServerSideEncryptionConfiguration": {
               "Rules": [
                  {
                     "ApplyServerSideEncryptionByDefault": {
                        "SSEAlgorithm": "{{SSEAlgorithm}}"
                     }
                  }
               ]
            }
         },
         "isEnd": true
      }
   ]
}
```

------