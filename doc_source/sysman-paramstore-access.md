# Controlling access to Systems Manager parameters<a name="sysman-paramstore-access"></a>

You control access to Systems Manager Parameters by using AWS Identity and Access Management \(IAM\)\. More specifically, you create IAM policies that restrict access to the following API operations:
+ [DeleteParameter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeleteParameter.html)
+ [DeleteParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeleteParameters.html)
+ [DescribeParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeParameters.html)
+ [GetParameter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameter.html)
+ [GetParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html)
+ [GetParameterHistory](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameterHistory.html)
+ [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html)
+ [PutParameter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutParameter.html)

We recommend that you control access to Systems Manager parameters by creating restrictive IAM policies\. For example, the following policy allows a user to call the `DescribeParameters` and `GetParameters` API operations for a limited set of resources\. This means that the user can get information about and use all parameters that begin with `prod-*`\.

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
            "Resource": "arn:aws:ssm:us-east-2:123456789012:parameter/prod-*"
        }
    ]
}
```

For trusted administrators, you can provide access to all Systems Manager parameter API operations by using a policy similar to the following example\. This policy gives the user full access to all production parameters that begin with `dbserver-prod-*`\.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "ssm:PutParameter",
                "ssm:DeleteParameter",
                "ssm:GetParameterHistory",
                "ssm:GetParametersByPath",
                "ssm:GetParameters",
                "ssm:GetParameter",
                "ssm:DeleteParameters"
            ],
            "Resource": "arn:aws:ssm:region:account-id:parameter/dbserver-prod-*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "ssm:DescribeParameters",
            "Resource": "*"
        }
    ]
}
```

**Topics**
+ [Allowing only specific parameters to run on instances](#sysman-paramstore-access-inst)
+ [Controlling access to parameters using tags](#sysman-paramstore-access-tag)

## Allowing only specific parameters to run on instances<a name="sysman-paramstore-access-inst"></a>

You can control access so that instances can run only parameters that you specify\. 

If you choose the `SecureString` parameter type when you create your parameter, Systems Manager uses AWS Key Management Service \(KMS\) to encrypt the parameter value\. AWS KMS encrypts the value by using either an AWS\-managed customer master key \(CMK\) or a customer managed CMK\. For more information about AWS KMS and CMKs, see the *[AWS Key Management Service Developer Guide](https://docs.aws.amazon.com/kms/latest/developerguide/)*\.

You can view the AWS\-managed CMK by running the following command from the AWS CLI:

```
aws kms describe-key --key-id alias/aws/ssm
```

The following example enables instances to get a parameter value only for parameters that begin with "prod\-" If the parameter is a `SecureString` parameter, then the instance decrypts the string using AWS KMS\.

**Note**  
Instance policies, like in the following example, are assigned to the instance role in IAM\. For more information about configuring access to Systems Manager features, including how to assign policies to users and instances, see [Setting up AWS Systems Manager](systems-manager-setting-up.md)\.

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

## Controlling access to parameters using tags<a name="sysman-paramstore-access-tag"></a>

After you tag a parameter, you can restrict access to it by creating an IAM policy that specifies the tags the user can access\. When a user attempts to use a parameter, the system checks the IAM policy and the tags specified for the parameter\. If the user does not have access to the tags assigned to the parameter, the user receives an *Access Denied* error\.

Currently, you can restrict access to the following `Get*` parameter\-related API actions:
+ [GetParameter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameter.html)
+ [GetParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html)
+ [GetParameterHistory ](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameterHistory.html)

Use the following procedure to create an IAM policy that restricts access to parameters by using tags\.

**Before You Begin**  
Create and tag parameters\. For more information, see [Getting started with Parameter Store](sysman-paramstore-settingup.md)\.

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

   This sample policy restricts access to only the `GetParameters` API action\. You can restrict access to multiple API actions by using the following format in the Action block:

   ```
   "Action":[
               "ssm:GetParameters",
               "ssm:GetParameter",
               "ssm:GetParameterHistory",
            ],
   ```

   You can specify multiple keys in the policy by using the following **Condition** format\. Specifying multiple keys creates an `AND` relationship for the keys\.

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

   You can specify multiple values in the policy by using the following **Condition** format\. **ForAnyValue** establishes an `OR` relationship for the values\. You can also specify **ForAllValues** to establish an `AND`** relationship\.

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

1. For **Name**, specify a name that identifies this as a user policy for tagged parameters\.

1. \(Optional\) For **Description**, enter a description\.

1. Verify details of the policy in the **Summary** section\.

1. Choose **Create policy**\.

1. Assign the policy to IAM users or groups\. For more information, see [Changing Permissions for an IAM User](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_change-permissions.html) and [Attaching a Policy to an IAM Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups_manage_attach-policy.html) in the *IAM User Guide*\.

After you attach the policy to the IAM user or group account, if a user tries to use a parameter and the user's policy does not allow the user to access a tag for the parameter \(call the `GetParameters` API action\), the system returns an error\. The error is similar to the following:

```
User: user-name isn't authorized to perform: ssm:GetParameters on resource: parameter-ARN with the following command.
```

If a parameter has multiple tags, the user will still receive the *Access Denied* error if the user does not have permission to access any one of those tags\.