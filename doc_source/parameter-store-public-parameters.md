# Using public parameters<a name="parameter-store-public-parameters"></a>

Some AWS services publish information about common artifacts as Systems Manager *public* parameters\. For example, the Amazon Elastic Compute Cloud \(Amazon EC2\) service publishes information about Amazon Machines Images \(AMIs\) as public parameters\. You can call this information from your scripts and code by using the [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html), [GetParameter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameter.html), and [GetParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html) API actions\.

This topic describes how to call public parameters by using the AWS CLI\.

**Topics**
+ [Calling AMI public parameters](#parameter-store-public-parameters-ami)
+ [Calling the ECS optimized AMI public parameter](#parameter-store-public-parameters-ecs)
+ [Calling AWS service, Region, and endpoint public parameters](#parameter-store-public-parameters-global-infrastructure)

## Calling AMI public parameters<a name="parameter-store-public-parameters-ami"></a>

Amazon EC2 AMI public parameters are available from the following paths:
+ /aws/service/ami\-amazon\-linux\-latest
+ /aws/service/ami\-windows\-latest

You can view a list of all Linux AMIs in the current AWS Region by using the following command in the AWS CLI\.

```
aws ssm get-parameters-by-path --path /aws/service/ami-amazon-linux-latest --query Parameters[].Name
```

The command returns information like the following\.

```
[
    "/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-ebs",
    "/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-gp2",
    "/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-s3",
    "/aws/service/ami-amazon-linux-latest/amzn-ami-minimal-hvm-x86_64-s3",
    "/aws/service/ami-amazon-linux-latest/amzn-ami-minimal-pv-x86_64-s3",
    "/aws/service/ami-amazon-linux-latest/amzn-ami-pv-x86_64-s3",
    "/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-arm64-gp2",
    "/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-ebs",
    "/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2",
    "/aws/service/ami-amazon-linux-latest/amzn2-ami-minimal-hvm-arm64-ebs",
    "/aws/service/ami-amazon-linux-latest/amzn-ami-minimal-hvm-x86_64-ebs",
    "/aws/service/ami-amazon-linux-latest/amzn-ami-minimal-pv-x86_64-ebs",
    "/aws/service/ami-amazon-linux-latest/amzn-ami-pv-x86_64-ebs",
    "/aws/service/ami-amazon-linux-latest/amzn2-ami-minimal-hvm-x86_64-ebs"
]
```

You can view details about these AMIs, including the AMI IDs and Amazon Resource Names \(ARNs\), by using the following command\.

```
aws ssm get-parameters-by-path --path "/aws/service/ami-amazon-linux-latest" --region Region
```

The command returns information like the following\. This example output has been truncated for space\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-ebs",
            "Type": "String",
            "Value": "ami-0d75cc1d706735521",
            "Version": 7,
            "LastModifiedDate": 1543873943.358,
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-ebs"
        },
        {
            "Name": "/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-gp2",
            "Type": "String",
            "Value": "ami-0cd3dfa4e37921605",
            "Version": 7,
            "LastModifiedDate": 1543873943.47,
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-gp2"
        },
        {
            "Name": "/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-s3",
            "Type": "String",
            "Value": "ami-0a0e3ff8af8d19497",
            "Version": 7,
            "LastModifiedDate": 1543873943.576,
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/ami-amazon-linux-latest/amzn-ami-hvm-x86_64-s3"
        },
        {
            "Name": "/aws/service/ami-amazon-linux-latest/amzn-ami-minimal-hvm-x86_64-ebs",
            "Type": "String",
            "Value": "ami-0786a9626196d6dac",
            "Version": 7,
            "LastModifiedDate": 1543873943.682,
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/ami-amazon-linux-latest/amzn-ami-minimal-hvm-x86_64-ebs"
        },
```

You can view details of a specific AMI by using the [GetParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html) API action with the full AMI name, including the path\. Here is an example command\.

```
aws ssm get-parameters --names /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 --region us-west-2
```

The command returns the following information\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2",
            "Type": "String",
            "Value": "ami-061392db613a6357b",
            "Version": 16,
            "LastModifiedDate": 1552519670.776,
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2"
        }
    ],
    "InvalidParameters": []
}
```

## Calling the ECS optimized AMI public parameter<a name="parameter-store-public-parameters-ecs"></a>

The Amazon Elastic Container Service \(Amazon ECS\) service publishes the name of the latest Amazon ECS optimized AMI as a public parameter\. Users are encouraged to use this AMI when creating a new Amazon EC2 cluster for Amazon ECS because the optimized AMI includes bug fixes and feature updates\. Use the following command to view the name of the latest Amazon ECS optimized AMI\.

```
aws ssm get-parameters --names /aws/service/ecs/optimized-ami/amazon-linux/recommended
```

The command returns information like the following\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/ecs/optimized-ami/amazon-linux/recommended",
            "Type": "String",
            "Value": "{\"schema_version\":1,\"image_name\":\"amzn-ami-2018.03.p-amazon-ecs-optimized\",\"image_id\":\"ami-01a82c3fce2c3ba58\",\"os\":\"Amazon Linux\",\"ecs_runtime_version\":\"Docker version 18.06.1-ce\",\"ecs_agent_version\":\"1.27.0\"}",
            "Version": 22,
            "LastModifiedDate": 1555434745.425,
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/ecs/optimized-ami/amazon-linux/recommended"
        }
    ],
    "InvalidParameters": []
}
```

## Calling AWS service, Region, and endpoint public parameters<a name="parameter-store-public-parameters-global-infrastructure"></a>

You can call AWS service, Region, and endpoint public parameters by using the following path\.

/aws/service/global\-infrastructure

You can view a list of all active AWS Regions by using the following command in the AWS CLI\.

```
aws ssm get-parameters-by-path --path /aws/service/global-infrastructure/regions --query Parameters[].Name
```

The command returns information like the following\.

```
"/aws/service/global-infrastructure/regions/ap-northeast-1"
"/aws/service/global-infrastructure/regions/eu-central-1"
"/aws/service/global-infrastructure/regions/eu-north-1"
"/aws/service/global-infrastructure/regions/eu-west-1"
"/aws/service/global-infrastructure/regions/eu-west-3"
"/aws/service/global-infrastructure/regions/sa-east-1"
"/aws/service/global-infrastructure/regions/us-east-2"
"/aws/service/global-infrastructure/regions/us-gov-east-1"
"/aws/service/global-infrastructure/regions/us-gov-west-1"
"/aws/service/global-infrastructure/regions/us-west-1"
"/aws/service/global-infrastructure/regions/ap-northeast-2"
"/aws/service/global-infrastructure/regions/ap-northeast-3"
"/aws/service/global-infrastructure/regions/ap-south-1"
"/aws/service/global-infrastructure/regions/ap-southeast-1"
"/aws/service/global-infrastructure/regions/ap-southeast-2"
"/aws/service/global-infrastructure/regions/ca-central-1"
"/aws/service/global-infrastructure/regions/cn-north-1"
"/aws/service/global-infrastructure/regions/cn-northwest-1"
"/aws/service/global-infrastructure/regions/eu-west-2"
"/aws/service/global-infrastructure/regions/us-west-2"
"/aws/service/global-infrastructure/regions/us-east-1"
```

You can view a complete list of all available AWS services and sort them into alphabetical order by using the following command\. This example output has been truncated for space\.

```
aws ssm get-parameters-by-path --path /aws/service/global-infrastructure/services --query Parameters[].Name | sort
```

The command returns information like the following\.

```
"/aws/service/global-infrastructure/services/acm-pca",
    "/aws/service/global-infrastructure/services/acm",
    "/aws/service/global-infrastructure/services/alexaforbusiness",
    "/aws/service/global-infrastructure/services/apigateway",
    "/aws/service/global-infrastructure/services/application-autoscaling",
    "/aws/service/global-infrastructure/services/appmesh",
    "/aws/service/global-infrastructure/services/appstream",
    "/aws/service/global-infrastructure/services/appsync",
    "/aws/service/global-infrastructure/services/athena",
    "/aws/service/global-infrastructure/services/autoscaling-plans",
    "/aws/service/global-infrastructure/services/autoscaling",
    "/aws/service/global-infrastructure/services/backup",
    "/aws/service/global-infrastructure/services/batch",
    "/aws/service/global-infrastructure/services/budgets",
    "/aws/service/global-infrastructure/services/ce",
    "/aws/service/global-infrastructure/services/chime",
    "/aws/service/global-infrastructure/services/cloud9",
    "/aws/service/global-infrastructure/services/cloudformation",
    "/aws/service/global-infrastructure/services/cloudfront",
    "/aws/service/global-infrastructure/services/cloudhsm",
    "/aws/service/global-infrastructure/services/cloudhsmv2",
    "/aws/service/global-infrastructure/services/cloudsearch",
    "/aws/service/global-infrastructure/services/cloudtrail",
    "/aws/service/global-infrastructure/services/cloudwatch",
    "/aws/service/global-infrastructure/services/codebuild",
```

You can view a list of Regions where a service is available\. This example uses Systems Manager \(ssm\)\.

```
aws ssm get-parameters-by-path --path /aws/service/global-infrastructure/services/ssm/regions --query Parameters[].Value
```

The command returns information like the following\.

```
[
    "ap-east-1",
    "ap-northeast-2",
    "ap-south-1",
    "ca-central-1",
    "cn-northwest-1",
    "eu-west-2",
    "sa-east-1",
    "us-east-2",
    "us-gov-east-1",
    "us-west-2",
    "ap-northeast-1",
    "ap-southeast-1",
    "ap-southeast-2",
    "cn-north-1",
    "eu-central-1",
    "eu-north-1",
    "eu-west-1",
    "us-east-1",
    "us-gov-west-1",
    "us-west-1",
    "eu-west-3"
]
```

You can view a regional endpoint for a service by using the following command\.

```
aws ssm get-parameter --name /aws/service/global-infrastructure/regions/us-west-1/services/ssm/endpoint --query Parameter.Value
```

The command returns information like the following\.

```
"ssm.us-west-1.amazonaws.com"
```

**Related blog posts**
+ [Query for AWS Regions, Endpoints, and More Using AWS Systems Manager Parameter Store](http://aws.amazon.com/blogs/aws/new-query-for-aws-regions-endpoints-and-more-using-aws-systems-manager-parameter-store/)
+ [Query for the latest Amazon Linux AMI IDs using AWS Systems Manager Parameter Store](http://aws.amazon.com/blogs/compute/query-for-the-latest-amazon-linux-ami-ids-using-aws-systems-manager-parameter-store/)
+ [Query for the Latest Windows AMI Using AWS Systems Manager Parameter Store](http://aws.amazon.com/blogs/mt/query-for-the-latest-windows-ami-using-systems-manager-parameter-store/)