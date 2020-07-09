# Restricting access to Systems Manager parameters using IAM policies<a name="sysman-paramstore-access"></a>

You restrict access to Systems Manager parameters by using AWS Identity and Access Management \(IAM\)\. More specifically, you create IAM policies that restrict access to the following API operations:
+ [DeleteParameter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeleteParameter.html)
+ [DeleteParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DeleteParameters.html)
+ [DescribeParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_DescribeParameters.html)
+ [GetParameter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameter.html)
+ [GetParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html)
+ [GetParameterHistory](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameterHistory.html)
+ [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html)
+ [PutParameter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_PutParameter.html)

When using IAM policies to restrict access to Systems Manager parameters, we recommend that you create and use *restrictive* IAM policies\. For example, the following policy allows a user to call the `DescribeParameters` and `GetParameters` API operations for a limited set of resources\. This means that the user can get information about and use all parameters that begin with `prod-*`\.

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