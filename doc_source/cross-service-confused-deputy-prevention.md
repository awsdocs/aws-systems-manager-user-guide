# Cross\-service confused deputy prevention<a name="cross-service-confused-deputy-prevention"></a>

The confused deputy problem is a security issue where an entity that doesn't have permission to perform an action can coerce a more\-privileged entity to perform the action\. In AWS, cross\-service impersonation can result in the confused deputy problem\. Cross\-service impersonation can occur when one service \(the *calling service*\) calls another service \(the *called service*\)\. The calling service can be manipulated to use its permissions to act on another customer's resources in a way it should not otherwise have permission to access\. To prevent this, AWS provides tools that help you protect your data for all services with service principals that have been given access to resources in your account\. 

We recommend using the [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcearn](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcearn) and [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount) global condition context keys in resource policies to limit the permissions that AWS Systems Manager gives another service to the resource\. If the `aws:SourceArn` value does not contain the account ID, such as an Amazon S3 bucket Amazon Resource Name \(ARN\), you must use both global condition context keys to limit permissions\. If you use both global condition context keys and the `aws:SourceArn` value contains the account ID, the `aws:SourceAccount` value and the account in the `aws:SourceArn` value must use the same account ID when used in the same policy statement\. Use `aws:SourceArn` if you want only one resource to be associated with the cross\-service access\. Use `aws:SourceAccount` if you want to allow any resource in that account to be associated with the cross\-service use\.

The following sections provide example policies for AWS Systems Manager capabilities\.

## Hybrid activation policy example<a name="cross-service-confused-deputy-prevention-hybrid"></a>

For service roles used in a hybrid activation, the value of `aws:SourceArn` must be the ARN of the AWS account\. Be sure to specify the AWS Region in the ARN where you created your hybrid activation\. If you don't know the full ARN of the resource or if you're specifying multiple resources, use the `aws:SourceArn` global context condition key with wildcards \(`*`\) for the unknown portions of the ARN\. For example, `arn:aws:ssm:*:AWS Region:123456789012:*`\.

The following example shows how you can use the `aws:SourceArn` and `aws:SourceAccount` global condition context keys for Automation to prevent the confused deputy problem\.

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Sid":"",
         "Effect":"Allow",
         "Principal":{
            "Service":"ssm.amazonaws.com"
         },
         "Action":"sts:AssumeRole",
         "Condition":{
            "StringEquals":{
               "aws:SourceAccount":"123456789012"
            },
            "ArnEquals":{
               "aws:SourceArn":"arn:aws:ssm:us-east-2:123456789012:*"
            }
         }
      }
   ]
}
```

## Resource data sync policy example<a name="cross-service-confused-deputy-prevention-rds"></a>

Systems Manager Inventory, Explorer, and Compliance enable you to create a resource data sync to centralize storage of your operations data \(OpsData\) in a central Amazon Simple Storage Service bucket\. If you want to encrypt a resource data sync by using AWS Key Management Service \(AWS KMS\), then you must either create a new key that includes the following policy, or you must update an existing key and add this policy to it\. The `aws:SourceArn` and `aws:SourceAccount` condition keys in this policy prevent the confused deputy problem\. Here is an example policy\.

```
{
    "Version": "2012-10-17",
    "Id": "ssm-access-policy",
    "Statement": [
        {
            "Sid": "ssm-access-policy-statement",
            "Action": [
                "kms:GenerateDataKey"
            ],
            "Effect": "Allow",
            "Principal": {
                "Service": "ssm.amazonaws.com"
            },
            "Resource": "arn:aws:kms:us-east-2:123456789012:key/KMS_key_id",
            "Condition": {
                "StringLike": {
                    "aws:SourceAccount": "123456789012"
                },
                "ArnLike": {
                    "aws:SourceArn": "arn:aws:ssm:*:123456789012:role/aws-service-role/ssm.amazonaws.com/AWSServiceRoleForAmazonSSM"
                }
            }
        }
    ]
}
```

**Note**  
The ARN in the policy example enables the system to encrypt OpsData from all sources except AWS Security Hub\. If you need to encrypt Security Hub data, for example if you use Explorer to collect Security Hub data, then you must attach an additional policy that specifies the following ARN:  
"aws:SourceArn":"arn:aws:ssm:\*:*123456789012*:role/aws\-service\-role/opsdatasync\.ssm\.amazonaws\.com/AWSServiceRoleForSystemsManagerOpsDataSync" 