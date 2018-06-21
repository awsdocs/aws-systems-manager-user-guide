# Controlling Access to Systems Manager Parameters<a name="sysman-paramstore-access"></a>

You control access to Systems Manager Parameters by using AWS Identity and Access Management \(IAM\)\. More specifically, you create IAM policies that restrict access to the following API operations:
+ [DeleteParameter](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeleteParameter.html)
+ [DeleteParameters](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeleteParameters.html) \(to delete parameters by using the Amazon EC2 console\)
+ [DescribeParameters](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeParameters.html)
+ [GetParameter](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameter.html)
+ [GetParameters](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html)
+ [PutParameter](http://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutParameter.html)

We recommend that you control access to Systems Manager parameters by creating restrictive IAM policies\. For example, the following policy allows you to call the `DescribeParameters` and `GetParameters` API operations for a specific resource\. This means that you can get information about and use all parameters that begin with prod\-\*\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:DescribeParameters"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetParameters"
            ],
            "Resource": "arn:aws:ssm:us-east-2:123456123:parameter/prod-*"
        }
    ]
}
```

For trusted administrators, you could provide full access to all Systems Manager parameter API operations by using a policy like the following example\. This policy gives the user full access to all production parameters that begin with dbserver\-prod\-\*\.

```
{
   "Version":"2012-10-17",
   "Effect":"Allow",
   "Action":[
      "ssm:DescribeParameters",
      "ssm:PutParameter",
      "ssm:GetParameters",
      "ssm:DeleteParameter"
   ],
   "Resource":[
      "arn:aws:ssm:region:account id:parameter/dbserver-prod-*"
   ]
}
```

**Topics**
+ [Allowing Only Specific Parameters to Run on Instances](#sysman-paramstore-access-inst)
+ [Controlling Access to Parameters Using Tags](#sysman-paramstore-access-tag)

## Allowing Only Specific Parameters to Run on Instances<a name="sysman-paramstore-access-inst"></a>

You can also control access so that instances can only run specific parameters\. The following example enables instances to get a parameter value only for parameters that begin with "prod\-" If the parameter is a secure string, then the instance decrypts the string using AWS KMS\.

**Note**  
If you choose the Secure String data type when you create your parameter, then AWS KMS encrypts the parameter value\. For more information about AWS KMS, see [AWS Key Management Service Developer Guide](http://docs.aws.amazon.com/kms/latest/developerguide/)\.  
Each AWS account is assigned a default AWS KMS key\. You can view your key by executing the following command from the AWS CLI:  

```
aws kms describe-key --key-id alias/aws/ssm
```

```
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action":[
            "ssm:GetParameters"
         ],
         "Resource":[
            "arn:aws:ssm:region:account-id:parameter/prod-*"
         ]
      },
      {
         "Effect":"Allow",
         "Action":[
            "kms:Decrypt"
         ],
         "Resource":[
            "arn:aws:kms:region:account-id:key/CMK"
         ]
      }
   ]
}
```

**Note**  
Instance policies, like in the previous example, are assigned to the instance role in IAM\. For more information about configuring access to Systems Manager features, including how to assign policies to users and instances, see [Configuring Access to Systems Manager](systems-manager-access.md)\.

## Controlling Access to Parameters Using Tags<a name="sysman-paramstore-access-tag"></a>

After you tag a parameter, you can restrict access to it by creating an IAM policy that specifies the tags the user can access\. When a user attempts to use a parameter, the system checks the IAM policy and the tags specified for the parameter\. If the user does not have access to the tags assigned to the parameter, the user receives an access denied error\. Use the following procedure to create an IAM policy that restricts access to parameters by using tags\.

**Before You Begin**  
Create and tag parameters\. For more information, see [Setting Up Systems Manager Parameters](sysman-paramstore-settingup.md)\.

**To restrict a user's access to parameters by using tags**

1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Policies**, and then choose **Create policy**\.

1. Choose the **JSON** tab\.

1. Copy the following sample policy and paste it into the text field, replacing the sample text\. Replace *tag\_key* and *tag\_value* with the key\-value pair for your tag\.

   ```
   {
      "Version":"2012-10-17",
      "Statement":[
         {
            "Effect":"Allow",
            "Action":[
               "ssm:GetParameters"
            ],
            "Resource":"*",
            "Condition":{
               "StringLike":{
                  "ssm:resourceTag/tag_key":[
                     "tag_value"
                  ]
               }
            }
         }
      ]
   }
   ```

   You can specify multiple keys in the policy by using the following **Condition** format\. Specifying multiple keys creates an *AND* relationship for the keys\.

   ```
   "Condition":{
      "StringLike":{
         "ssm:resourceTag/tag_key1":[
                     "tag_value1"
         ],
         "ssm:resourceTag/tag_key2":[
                     "tag_value2"
         ]
      }
   }
   ```

   You can specify multiple values in the policy by using the following **Condition** format\. **ForAnyValue** establishes an *OR* relationship for the values\. You can also specify **ForAllValues** to establish an AND relationship\.

   ```
   "Condition":{
      "ForAnyValue:StringLike":{
         "ssm:resourceTag/tag_key1":[
            "tag_value1",
            "tag_value2"
         ]
      }
   }
   ```

1. Choose **Review policy**\.

1. In the **Name** field, specify a name that identifies this as a user policy for tagged parameters\.

1. Enter a description\.

1. Verify details of the policy in the **Summary** section\.

1. Choose **Create policy**\.

1. Assign the policy to IAM users or groups\. For more information, see [Changing Permissions for an IAM User](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html) and [Attaching a Policy to an IAM Group](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_manage_attach-policy.html)\.

After you attach the policy to the IAM user or group account, if a user tries to use a parameter and the user's policy does not allow the user to access a tag for the parameter \(call the GetParameters API\), the system returns an error\. The error is similar to the following:

User: *user\_name* isn't authorized to perform: ssm:GetParameters on resource: *parameter ARN* with the following command\.

If a parameter has multiple tags, the user will still receive the access denied error if the user does not have permission to access any one of those tags\.