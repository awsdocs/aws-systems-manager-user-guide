# Public parameters<a name="parameter-store-public-parameters"></a>

Some AWS services publish information about common artifacts as Systems Manager *public* parameters\. For example, the Amazon Elastic Compute Cloud \(Amazon EC2\) service publishes information about Amazon Machines Images \(AMIs\) as public parameters\.

You can call this information from your scripts and code by using the [GetParametersByPath](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParametersByPath.html), [GetParameter](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameter.html), and [GetParameters](https://docs.aws.amazon.com/systems-manager/latest/APIReference/API_GetParameters.html) API actions\.

**Related blog posts**
+ [Query for AWS Regions, Endpoints, and More Using AWS Systems Manager Parameter Store](http://aws.amazon.com/blogs/aws/new-query-for-aws-regions-endpoints-and-more-using-aws-systems-manager-parameter-store/)
+ [Query for the latest Amazon Linux AMI IDs using AWS Systems Manager Parameter Store](http://aws.amazon.com/blogs/compute/query-for-the-latest-amazon-linux-ami-ids-using-aws-systems-manager-parameter-store/)
+ [Query for the Latest Windows AMI Using AWS Systems Manager Parameter Store](http://aws.amazon.com/blogs/mt/query-for-the-latest-windows-ami-using-systems-manager-parameter-store/)

The remainder of this topic describes how to call public parameters by using the AWS CLI\.

**Topics**
+ [Calling AMI public parameters](#parameter-store-public-parameters-ami)
+ [Calling the ECS optimized AMI public parameter](#parameter-store-public-parameters-ecs)
+ [Calling the EKS optimized AMI public parameter](#parameter-store-public-parameters-eks)
+ [Calling public parameters for AWS services, Regions, endpoints, availability zones, and local zones](#parameter-store-public-parameters-global-infrastructure)

## Calling AMI public parameters<a name="parameter-store-public-parameters-ami"></a>

Amazon EC2 AMI public parameters are available from the following paths:
+ /aws/service/ami\-amazon\-linux\-latest
+ /aws/service/ami\-windows\-latest

You can view a list of all Linux AMIs in the current AWS Region by using the following command in the AWS CLI\.

------
#### [ Linux ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/ami-amazon-linux-latest \
    --query 'Parameters[].Name'
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/ami-amazon-linux-latest ^
    --query Parameters[].Name
```

------

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

------
#### [ Linux ]

```
aws ssm get-parameters-by-path \
    --path "/aws/service/ami-amazon-linux-latest" \
    --region region
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path "/aws/service/ami-amazon-linux-latest" ^
    --region region
```

------

*region* represents the identifier for an AWS Region supported by AWS Systems Manager, such as `us-east-2` for the US East \(Ohio\) Region\. For a list of supported *region* values, see the **Region** column in [Systems Manager service endpoints](https://docs.aws.amazon.com/general/latest/gr/ssm.html#ssm_region) in the *Amazon Web Services General Reference*\.

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

------
#### [ Linux ]

```
aws ssm get-parameters \
    --names /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 \
    --region us-west-2
```

------
#### [ Windows ]

```
aws ssm get-parameters ^
    --names /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 ^
    --region us-west-2
```

------

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

------
#### [ Linux ]

```
aws ssm get-parameters \
    --names /aws/service/ecs/optimized-ami/amazon-linux/recommended
```

------
#### [ Windows ]

```
aws ssm get-parameters ^
    --names /aws/service/ecs/optimized-ami/amazon-linux/recommended
```

------

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

## Calling the EKS optimized AMI public parameter<a name="parameter-store-public-parameters-eks"></a>

The Amazon Elastic Kubernetes Service \(Amazon EKS\) service publishes the name of the latest Amazon EKS optimized AMI as a public parameter\. Users are encouraged to use this AMI when adding nodes to an Amazon EKS cluster, as new releases include Kubernetes patches and security updates\. Previously, to ensure you were using the latest AMI meant checking the Amazon EKS documentation and manually updating any deployment templates or resources with the new AMI ID\.

Use the following command to view the name of the latest Amazon EKS optimized AMI\.

------
#### [ Linux ]

```
aws ssm get-parameters \
    --names /aws/service/eks/optimized-ami/1.14/amazon-linux-2/recommended
```

------
#### [ Windows ]

```
aws ssm get-parameters ^
    --names /aws/service/eks/optimized-ami/1.14/amazon-linux-2/recommended
```

------

The command returns information like the following\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/eks/optimized-ami/1.14/amazon-linux-2/recommended",
            "Type": "String",
            "Value": "{\"schema_version\":\"2\",\"image_id\":\"ami-0b89776dcfa5f2dee\",\"image_name\":\"amazon-eks-node-1.14-v20200507\",\"release_version\":\"1.14.9-20200507\"}",
            "Version": 11,
            "LastModifiedDate": "2020-05-11T17:34:51.990000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/eks/optimized-ami/1.14/amazon-linux-2/recommended"
        }
    ],
    "InvalidParameters": []
}
```

## Calling public parameters for AWS services, Regions, endpoints, availability zones, and local zones<a name="parameter-store-public-parameters-global-infrastructure"></a>

You can call AWS service, Region, endpoint, and availability zone public parameters by using the following path\.

/aws/service/global\-infrastructure

**View active AWS Regions**  
You can view a list of all active AWS Regions by using the following command in the AWS CLI\.

------
#### [ Linux ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/regions \
    --query 'Parameters[].Name'
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/regions ^
    --query Parameters[].Name
```

------

The command returns information like the following\.

```
[
    "/aws/service/global-infrastructure/regions/af-south-1",
    "/aws/service/global-infrastructure/regions/ap-east-1",
    "/aws/service/global-infrastructure/regions/ap-northeast-3",
    "/aws/service/global-infrastructure/regions/ap-southeast-1",
    "/aws/service/global-infrastructure/regions/ca-central-1",
    "/aws/service/global-infrastructure/regions/cn-north-1",
    "/aws/service/global-infrastructure/regions/eu-west-2",
    "/aws/service/global-infrastructure/regions/eu-west-3",
    "/aws/service/global-infrastructure/regions/us-east-1",
    "/aws/service/global-infrastructure/regions/us-gov-west-1",
    "/aws/service/global-infrastructure/regions/ap-northeast-2",
    "/aws/service/global-infrastructure/regions/ap-south-1",
    "/aws/service/global-infrastructure/regions/ap-southeast-2",
    "/aws/service/global-infrastructure/regions/cn-northwest-1",
    "/aws/service/global-infrastructure/regions/eu-south-1",
    "/aws/service/global-infrastructure/regions/me-south-1",
    "/aws/service/global-infrastructure/regions/sa-east-1",
    "/aws/service/global-infrastructure/regions/us-east-2",
    "/aws/service/global-infrastructure/regions/us-gov-east-1",
    "/aws/service/global-infrastructure/regions/us-west-1",
    "/aws/service/global-infrastructure/regions/ap-northeast-1",
    "/aws/service/global-infrastructure/regions/eu-central-1",
    "/aws/service/global-infrastructure/regions/eu-north-1",
    "/aws/service/global-infrastructure/regions/eu-west-1",
    "/aws/service/global-infrastructure/regions/us-west-2"
]
```

**View available AWS services**  
You can view a complete list of all available AWS services and sort them into alphabetical order by using the following command\. This example output has been truncated for space\.

------
#### [ Linux ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/services \
    --query 'Parameters[].Name | sort(@)'
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/services ^
    --query "Parameters[].Name | sort(@)"
```

------

The command returns information like the following\.

```
[
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

**View supported Regions for an AWS service**  
You can view a list of AWS Regions where a service is available\. This example uses Systems Manager \(ssm\)\.

------
#### [ Linux ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/services/ssm/regions \
    --query 'Parameters[].Value'
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/services/ssm/regions ^
    --query Parameters[].Value
```

------

The command returns information like the following\.

```
[
    "ap-south-1",
    "ca-central-1",
    "cn-north-1",
    "eu-central-1",
    "eu-west-1",
    "eu-west-2",
    "eu-west-3",
    "me-south-1",
    "us-east-2",
    "us-gov-west-1",
    "af-south-1",
    "ap-east-1",
    "ap-northeast-1",
    "ap-northeast-2",
    "ap-southeast-1",
    "ap-southeast-2",
    "eu-north-1",
    "eu-south-1",
    "us-gov-east-1",
    "us-west-1",
    "cn-northwest-1",
    "sa-east-1",
    "us-east-1",
    "us-west-2"
]
```

**View the regional endpoint for a service**  
You can view a regional endpoint for a service by using the following command\.

------
#### [ Linux ]

```
aws ssm get-parameter \
    --name /aws/service/global-infrastructure/regions/us-west-1/services/ssm/endpoint \
    --query 'Parameter.Value'
```

------
#### [ Windows ]

```
aws ssm get-parameter ^
    --name /aws/service/global-infrastructure/regions/us-west-1/services/ssm/endpoint ^
    --query Parameter.Value
```

------

The command returns information like the following\.

```
"ssm.us-west-1.amazonaws.com"
```

**View complete availability zone details**  
You can view availability zones by using the following command\.

------
#### [ Linux ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/availability-zones/
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/availability-zones/
```

------

The command returns information like the following\. This example has been truncated for space\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/global-infrastructure/availability-zones/apne3-az3",
            "Type": "String",
            "Value": "apne3-az3",
            "Version": 1,
            "LastModifiedDate": "2020-04-03T14:29:28.995000-07:00",
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/global-infrastructure/availability-zones/apne3-az3"
        },
        {
            "Name": "/aws/service/global-infrastructure/availability-zones/aps1-az2",
            "Type": "String",
            "Value": "aps1-az2",
            "Version": 1,
            "LastModifiedDate": "2020-04-03T14:21:02.690000-07:00",
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/global-infrastructure/availability-zones/aps1-az2"
        },
        {
            "Name": "/aws/service/global-infrastructure/availability-zones/cnn1-az2",
            "Type": "String",
            "Value": "cnn1-az2",
            "Version": 1,
            "LastModifiedDate": "2020-04-03T14:19:57.254000-07:00",
            "ARN": "arn:aws:ssm:us-west-2::parameter/aws/service/global-infrastructure/availability-zones/cnn1-az2"
        },
```

**View availability zone names only**  
You can view the names of availability zones only by using the following command\.

------
#### [ Linux ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/availability-zones \
    --query 'Parameters[].Name | sort(@)'
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/availability-zones ^
    --query "Parameters[].Name | sort(@)"
```

------

The command returns information like the following\. This example has been truncated for space\.

```
[
    "/aws/service/global-infrastructure/availability-zones/afs1-az1",
    "/aws/service/global-infrastructure/availability-zones/afs1-az2",
    "/aws/service/global-infrastructure/availability-zones/afs1-az3",
    "/aws/service/global-infrastructure/availability-zones/ape1-az1",
    "/aws/service/global-infrastructure/availability-zones/ape1-az2",
    "/aws/service/global-infrastructure/availability-zones/ape1-az3",
    "/aws/service/global-infrastructure/availability-zones/apne1-az1",
    "/aws/service/global-infrastructure/availability-zones/apne1-az2",
    "/aws/service/global-infrastructure/availability-zones/apne1-az3",
    "/aws/service/global-infrastructure/availability-zones/apne1-az4",
```

**View names of availability zones in a single Region**  
You can view the names of the availability zones in one Region \(`us-east-1`, in this example\) using the following command\.

------
#### [ Linux ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/regions/us-east-1/availability-zones \
    --query 'Parameters[].Name | sort(@)'
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/regions/us-east-1/availability-zones ^
    --query "Parameters[].Name | sort(@)"
```

------

The command returns information like the following\. This example has been truncated for space\.

```
[
    "/aws/service/global-infrastructure/regions/us-east-1/availability-zones/use1-az1",
    "/aws/service/global-infrastructure/regions/us-east-1/availability-zones/use1-az2",
    "/aws/service/global-infrastructure/regions/us-east-1/availability-zones/use1-az3",
    "/aws/service/global-infrastructure/regions/us-east-1/availability-zones/use1-az4",
    "/aws/service/global-infrastructure/regions/us-east-1/availability-zones/use1-az5",
    "/aws/service/global-infrastructure/regions/us-east-1/availability-zones/use1-az6"
```

**View availability zone ARNs only**  
You can view the ARNs of availability zones only by using the following command\. 

------
#### [ Linux ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/availability-zones \
    --query 'Parameters[].ARN | sort(@)'
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/availability-zones ^
    --query "Parameters[].ARN | sort(@)"
```

------

The command returns information like the following\.

```
[
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/availability-zones/afs1-az1",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/availability-zones/afs1-az2",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/availability-zones/afs1-az3",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/availability-zones/ape1-az1",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/availability-zones/ape1-az2",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/availability-zones/ape1-az3",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/availability-zones/apne1-az1",
```

**View local zone details**  
You can view local zones by using the following command\.

**Note**  
Only one local zone is currently available\.

------
#### [ Linux ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/local-zones/
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/local-zones/
```

------

The command returns information like the following\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1",
            "Type": "String",
            "Value": "usw2-lax1-az1",
            "Version": 1,
            "LastModifiedDate": "2020-04-29T20:27:03.291000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1"
        }
    ]
}
```

**View all parameters and values under a local zone**  
You can view all parameter data for a local zone by using the following command\.

------
#### [ Linux ]

```
aws ssm get-parameters-by-path \
    --path "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/"
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/"
```

------

The command returns information like the following\.

```
{
    "Parameters": [
        {
            "Name": "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/geolocationCountry",
            "Type": "String",
            "Value": "US",
            "Version": 1,
            "LastModifiedDate": "2020-04-29T20:27:03.430000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/geolocationCountry"
        },
        {
            "Name": "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/geolocationRegion",
            "Type": "String",
            "Value": "US-CA",
            "Version": 1,
            "LastModifiedDate": "2020-04-29T20:27:03.489000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/geolocationRegion"
        },
        {
            "Name": "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/network-border-group",
            "Type": "String",
            "Value": "us-west-2-lax-1",
            "Version": 1,
            "LastModifiedDate": "2020-04-29T20:27:03.611000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/network-border-group"
        },
        {
            "Name": "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/parent-availability-zone",
            "Type": "String",
            "Value": "usw2-az2",
            "Version": 1,
            "LastModifiedDate": "2020-04-29T20:27:03.736000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/parent-availability-zone"
        },
        {
            "Name": "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/parent-region",
            "Type": "String",
            "Value": "us-west-2",
            "Version": 1,
            "LastModifiedDate": "2020-04-29T20:27:03.670000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/parent-region"
        },
        {
            "Name": "/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/zone-group",
            "Type": "String",
            "Value": "us-west-2-lax-1",
            "Version": 1,
            "LastModifiedDate": "2020-04-29T20:27:03.552000-07:00",
            "ARN": "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/zone-group"
        }
    ]
}
```

**View local zone parameter names only**  
You can view just the names of local zone parameters by using the following command\.

------
#### [ Linux ]

```
aws ssm get-parameters-by-path \
    --path /aws/service/global-infrastructure/local-zones/usw2-lax1-az1 \
    --query 'Parameters[].Name | sort(@)'
```

------
#### [ Windows ]

```
aws ssm get-parameters-by-path ^
    --path /aws/service/global-infrastructure/local-zones/usw2-lax1-az1 ^
    --query "Parameters[].Name | sort(@)"
```

------

The command returns information like the following\. 

```
[
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/geolocationCountry",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/geolocationRegion",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/network-border-group",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/parent-availability-zone",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/parent-region",
    "arn:aws:ssm:us-east-2::parameter/aws/service/global-infrastructure/local-zones/usw2-lax1-az1/zone-group"
]
```